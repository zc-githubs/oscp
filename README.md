## 1. Discovery

### 1.1. Port scan

|Scan|Command|
|---|---|
|Initial network sweep|`nmap -sn $TARGET_RANGE`|
|TCP |`nmap -Pn -sT -A $TARGET_IP`|
|UDP |`nmap -sU -A $TARGET_IP`|

## 2. Enumeration

### 2.1. FTP `21`

lftp find

### 2.2. HTTP/HTTPS `80`/`443`/`8080`

dirb

gobuster

worldlists

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
