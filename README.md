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

enum4linux

### 2.4. LDAP `389`

## 3. Shells

### 3.1. Listener

```console
rlwrap nc -nlvp 4444
```

### 3.X. Various reverse shells

https://highon.coffee/blog/reverse-shell-cheat-sheet/

### 3.X. Web shells

### 3.X. Payloads

### 3.X. Windows direct connection

evil-winrm
impacket-psexec

### 3.X. Upgrade to Full TTY

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
