# 1. Nmap Scan

<details>
  <summary>TCP Scan</summary>

```console
┌──(root㉿kali)-[~]
└─# nmap -p- -A 10.0.88.36
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-20 13:00 +08
Nmap scan report for 10.0.88.36
Host is up (0.0018s latency).
Not shown: 65522 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey:
|   2048 4d8fbc014975830065a953a975c65733 (RSA)
|   256 92f704e209aad0d7e6fd21671fbd64ce (ECDSA)
|_  256 fb08cde8458c1ac1061b247333a5e477 (ED25519)
53/tcp    open  domain      dnsmasq 2.76
| dns-nsid:
|_  bind.version: dnsmasq-2.76
80/tcp    open  http        Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16)
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16
|_http-title: Apache HTTP Server Test Page powered by CentOS
| http-methods:
|_  Potentially risky methods: TRACE
111/tcp   open  rpcbind     2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100003  3,4         2049/udp   nfs
|   100003  3,4         2049/udp6  nfs
|   100005  1,2,3      20048/tcp   mountd
|   100005  1,2,3      20048/tcp6  mountd
|   100005  1,2,3      20048/udp   mountd
|   100005  1,2,3      20048/udp6  mountd
|   100021  1,3,4      38077/tcp   nlockmgr
|   100021  1,3,4      38975/udp   nlockmgr
|   100021  1,3,4      45370/tcp6  nlockmgr
|   100021  1,3,4      58779/udp6  nlockmgr
|   100024  1          33020/udp6  status
|   100024  1          38684/udp   status
|   100024  1          42942/tcp   status
|   100024  1          43664/tcp6  status
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
443/tcp   open  ssl/http    Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16)
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16
|_ssl-date: TLS randomness does not represent time
| http-methods:
|_  Potentially risky methods: TRACE
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2018-06-10T15:53:25
|_Not valid after:  2019-06-10T15:53:25
|_http-title: Apache HTTP Server Test Page powered by CentOS
445/tcp   open  netbios-ssn Samba smbd 4.7.1 (workgroup: WORKGROUP)
2049/tcp  open  nfs_acl     3 (RPC #100227)
3306/tcp  open  mysql       MariaDB (unauthorized)
8080/tcp  open  http        nginx 1.12.2
|_http-server-header: nginx/1.12.2
|_http-title: Welcome to Bravery! This is SPARTA!
| http-robots.txt: 4 disallowed entries
|_/cgi-bin/ /qwertyuiop.html /private /public
|_http-open-proxy: Proxy might be redirecting requests
20048/tcp open  mountd      1-3 (RPC #100005)
38077/tcp open  nlockmgr    1-4 (RPC #100021)
42942/tcp open  status      1 (RPC #100024)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.93%E=4%D=12/20%OT=22%CT=1%CU=41956%PV=Y%DS=2%DC=T%G=Y%TM=63A141
OS:AD%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=109%TI=Z%TS=A)OPS(O1=M5B4S
OS:T11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5=M5B4ST11NW7%O6=M
OS:5B4ST11)WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=7120)ECN(R=N)T1(R
OS:=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=N)T5(R=Y%DF=Y%T=40
OS:%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=N)T7(R=N)U1(R=Y%DF=N%T=40%IPL=164%UN=
OS:0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=N)

Network Distance: 2 hops
Service Info: Host: BRAVERY

Host script results:
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time:
|   date: 2022-12-20T05:01:25
|_  start_date: N/A
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: BRAVERY, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.7.1)
|   Computer name: localhost
|   NetBIOS computer name: BRAVERY\x00
|   Domain name: \x00
|   FQDN: localhost
|_  System time: 2022-12-20T00:01:25-05:00

TRACEROUTE (using port 8888/tcp)
HOP RTT     ADDRESS
1   0.39 ms 192.168.17.1
2   1.88 ms 10.0.88.36

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 40.47 seconds
```

</details>

<details>
  <summary>UDP Scan</summary>

```console
┌──(root㉿kali)-[~]
└─# nmap -sU -A --top-ports 100 10.0.88.36
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-20 13:01 +08
Nmap scan report for 10.0.88.36
Host is up (0.0021s latency).
Not shown: 52 closed udp ports (port-unreach), 43 open|filtered udp ports (no-response)
PORT     STATE SERVICE    VERSION
53/udp   open  domain     dnsmasq 2.76
| dns-nsid:
|_  bind.version: dnsmasq-2.76
|_dns-recursion: Recursion appears to be enabled
111/udp  open  rpcbind    2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100003  3,4         2049/udp   nfs
|   100003  3,4         2049/udp6  nfs
|   100005  1,2,3      20048/tcp   mountd
|   100005  1,2,3      20048/tcp6  mountd
|   100005  1,2,3      20048/udp   mountd
|   100005  1,2,3      20048/udp6  mountd
|   100021  1,3,4      38077/tcp   nlockmgr
|   100021  1,3,4      38975/udp   nlockmgr
|   100021  1,3,4      45370/tcp6  nlockmgr
|   100021  1,3,4      58779/udp6  nlockmgr
|   100024  1          33020/udp6  status
|   100024  1          38684/udp   status
|   100024  1          42942/tcp   status
|   100024  1          43664/tcp6  status
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
137/udp  open  netbios-ns Samba nmbd netbios-ns (workgroup: WORKGROUP)
| nbns-interfaces:
|   hostname: BRAVERY
|   interfaces:
|_    10.0.88.36
2049/udp open  nfs_acl    3 (RPC #100227)
5353/udp open  mdns       DNS-based service discovery
| dns-service-discovery:
|   9/tcp workstation
|     Address=10.0.88.36 fe80::4f33:8495:4f81:3b53
|   445/tcp smb
|_    Address=10.0.88.36 fe80::4f33:8495:4f81:3b53
Too many fingerprints match this host to give specific OS details
Network Distance: 2 hops
Service Info: Host: BRAVERY

Host script results:
|_nbstat: NetBIOS name: BRAVERY, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)

TRACEROUTE (using port 497/udp)
HOP RTT     ADDRESS
1   0.83 ms 192.168.17.1
2   1.33 ms 10.0.88.36

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 336.69 seconds
```

</details>

# 2. Enumeration

<details>
  <summary>HTTP <code>80</code></summary>

```console

```

</details>

<details>
  <summary>HTTP <code>8080</code></summary>

```console
┌──(root㉿kali)-[~]
└─# gobuster dir -u http://10.0.88.36:8080 -w /usr/share/seclists/Discovery/Web-Content/combined_words.txt
===============================================================
Gobuster v3.3
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.0.88.36:8080
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/combined_words.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.3
[+] Timeout:                 10s
===============================================================
2022/12/20 21:01:11 Starting gobuster in directory enumeration mode
===============================================================
/about                (Status: 200) [Size: 503]
/index.html           (Status: 200) [Size: 2637]
/private              (Status: 301) [Size: 185] [--> http://10.0.88.36:8080/private/]
/public               (Status: 301) [Size: 185] [--> http://10.0.88.36:8080/public/]
/robots.txt           (Status: 200) [Size: 103]
/.                    (Status: 301) [Size: 185] [--> http://10.0.88.36:8080/./]
Progress: 128265 / 128338 (99.94%)===============================================================
2022/12/20 21:02:52 Finished
===============================================================
```

</details>

<details>
  <summary>NFS <code>111</code>,<code>2049</code></summary>

```console
┌──(root㉿kali)-[~]
└─# rpcinfo -p 10.0.88.36
   program vers proto   port  service
    100000    4   tcp    111  portmapper
    100000    3   tcp    111  portmapper
    100000    2   tcp    111  portmapper
    100000    4   udp    111  portmapper
    100000    3   udp    111  portmapper
    100000    2   udp    111  portmapper
    100005    1   udp  20048  mountd
    100005    1   tcp  20048  mountd
    100024    1   udp  42914  status
    100024    1   tcp  38347  status
    100005    2   udp  20048  mountd
    100005    2   tcp  20048  mountd
    100005    3   udp  20048  mountd
    100005    3   tcp  20048  mountd
    100003    3   tcp   2049  nfs
    100003    4   tcp   2049  nfs
    100227    3   tcp   2049  nfs_acl
    100003    3   udp   2049  nfs
    100003    4   udp   2049  nfs
    100227    3   udp   2049  nfs_acl
    100021    1   udp  53170  nlockmgr
    100021    3   udp  53170  nlockmgr
    100021    4   udp  53170  nlockmgr
    100021    1   tcp  44073  nlockmgr
    100021    3   tcp  44073  nlockmgr
    100021    4   tcp  44073  nlockmgr

┌──(root㉿kali)-[~]
└─# showmount -e 10.0.88.36
Export list for 10.0.88.36:
/var/nfsshare *

┌──(root㉿kali)-[~]
└─# nmap -p 111 --script nfs* 10.0.88.36
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-20 21:02 +08
Nmap scan report for 10.0.88.36
Host is up (0.00063s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-statfs:
|   Filesystem     1K-blocks   Used       Available   Use%  Maxfilesize  Maxlink
|_  /var/nfsshare  17811456.0  4670420.0  13141036.0  27%   8388608.0T   255
| nfs-showmount:
|_  /var/nfsshare *
| nfs-ls: Volume /var/nfsshare
|   access: Read Lookup Modify Extend Delete NoExecute
| PERMISSION  UID    GID    SIZE  TIME                 FILENAME
| rwxrwxrwx   65534  65534  146   2018-12-26T06:25:11  .
| rwxr-xr-x   0      0      4096  2018-12-26T03:16:33  ..
| rw-r--r--   0      0      15    2018-12-26T06:15:59  README.txt
| rw-r--r--   0      0      29    2018-12-26T06:23:33  discovery
| rw-r--r--   0      0      51    2018-12-26T06:25:03  enumeration
| rw-r--r--   0      0      20    2018-12-26T06:24:50  explore
| rwxr-xr-x   0      0      19    2018-12-26T06:43:30  itinerary
| rw-r--r--   0      0      104   2018-12-26T06:24:01  password.txt
| rw-r--r--   0      0      67    2018-12-26T06:23:24  qwertyuioplkjhgfdsazxcvbnm
|_

Nmap done: 1 IP address (1 host up) scanned in 0.52 seconds

┌──(root㉿kali)-[~]
└─# mkdir /mnt/nfs

┌──(root㉿kali)-[~]
└─# mount -t nfs 10.0.88.36:/var/nfsshare /mnt/nfs

┌──(root㉿kali)-[~]
└─# ls -lRa /mnt/nfs
/mnt/nfs:
total 28
drwxrwxrwx 3 nobody nogroup  146 Dec 26  2018 .
drwxr-xr-x 3 root   root    4096 Dec 20 21:01 ..
-rw-r--r-- 1 root   root      29 Dec 26  2018 discovery
-rw-r--r-- 1 root   root      51 Dec 26  2018 enumeration
-rw-r--r-- 1 root   root      20 Dec 26  2018 explore
drwxr-xr-x 2 root   root      19 Dec 26  2018 itinerary
-rw-r--r-- 1 root   root     104 Dec 26  2018 password.txt
-rw-r--r-- 1 root   root      67 Dec 26  2018 qwertyuioplkjhgfdsazxcvbnm
-rw-r--r-- 1 root   root      15 Dec 26  2018 README.txt

/mnt/nfs/itinerary:
total 4
drwxr-xr-x 2 root   root      19 Dec 26  2018 .
drwxrwxrwx 3 nobody nogroup  146 Dec 26  2018 ..
-rw-r--r-- 1 root   root    1733 Dec 26  2018 david

┌──(root㉿kali)-[~]
└─# cat /mnt/nfs/discovery
Remember to LOOK AROUND YOU!

┌──(root㉿kali)-[~]
└─# cat /mnt/nfs/enumeration
Enumeration is at the heart of a penetration test!

┌──(root㉿kali)-[~]
└─# cat /mnt/nfs/explore
Exploration is fun!

┌──(root㉿kali)-[~]
└─# cat /mnt/nfs/password.txt
Passwords should not be stored in clear-text, written in post-its or written on files on the hard disk!

┌──(root㉿kali)-[~]
└─# cat /mnt/nfs/qwertyuioplkjhgfdsazxcvbnm
Sometimes, the answer you seek may be right before your very eyes.

┌──(root㉿kali)-[~]
└─# cat /mnt/nfs/README.txt
read me first!

┌──(root㉿kali)-[~]
└─# cat /mnt/nfs/itinerary/david
David will need to fly to various cities for various conferences. Here is his schedule.

1 January 2019 (Tuesday):
New Year's Day. Spend time with family.

2 January 2019 (Wednesday):
0900: Depart for airport.
0945: Check in at Changi Airport, Terminal 3.
1355 - 2030 hrs (FRA time): Board flight (SQ326) and land in Frankfurt.
2230: Check into hotel.

3 January 2019 (Thursday):
0800: Leave hotel.
0900 - 1700: Attend the Banking and Enterprise Conference.
1730 - 2130: Private reception with the Chancellor.
2230: Retire in hotel.

4 January 2019 (Friday):
0800: Check out from hotel.
0900: Check in at Frankfurt Main.
1305 - 1355: Board flight (LH1190) and land in Zurich.
1600 - 1900: Dinner reception
2000: Check into hotel.

5 January 2019 (Saturday):
0800: Leave hotel.
0930 - 1230: Visit University of Zurich.
1300 - 1400: Working lunch with Mr. Pandelson
1430 - 1730: Dialogue with students at the University of Zurich.
1800 - 2100: Working dinner with Mr. Robert James Miller and wife.
2200: Check into hotel.

6 January 2019 (Sunday):
0730: Leave hotel.
0800 - 1100: Give a lecture on Software Security and Design at the University of Zurich.
1130: Check in at Zurich.
1715 - 2025: Board flight (LX18) and land in Newark.
2230: Check into hotel.

7 January 2019 (Monday):
0800: Leave hotel.
0900 - 1200: Visit Goldman Sachs HQ
1230 - 1330: Working lunch with Bill de Blasio
1400 - 1700: Visit McKinsey HQ
1730 - 1830: Visit World Trade Center Memorial
2030: Return to hotel.

8 January 2019 (Tuesday):
0630: Check out from hotel.
0730: Check in at Newark.
0945 - 1715 (+1): Board flight (SQ21)

9 January 2019 (Wednesday):
1715: Land in Singapore.
1815 - 2015: Dinner with wife.
2100: Clear local emails and head to bed.
```

</details>

<details>
  <summary>SMB <code>443</code></summary>

```console
┌──(root㉿kali)-[~]
└─# enum4linux 10.0.88.36
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Tue Dec 20 21:17:54 2022

 =========================================( Target Information )=========================================

Target ........... 10.0.88.36
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 =============================( Enumerating Workgroup/Domain on 10.0.88.36 )=============================


[+] Got domain/workgroup name: WORKGROUP


 =================================( Nbtstat Information for 10.0.88.36 )=================================

Looking up status of 10.0.88.36
        BRAVERY         <00> -         B <ACTIVE>  Workstation Service
        BRAVERY         <03> -         B <ACTIVE>  Messenger Service
        BRAVERY         <20> -         B <ACTIVE>  File Server Service
        WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 ====================================( Session Check on 10.0.88.36 )====================================


[+] Server 10.0.88.36 allows sessions using username '', password ''


 =================================( Getting domain SID for 10.0.88.36 )=================================

Domain Name: WORKGROUP
Domain Sid: (NULL SID)

[+] Can't determine if host is part of domain or part of a workgroup


 ====================================( OS information on 10.0.88.36 )====================================


[E] Can't get OS info with smbclient


[+] Got OS info for 10.0.88.36 from srvinfo:
        BRAVERY        Wk Sv PrQ Unx NT SNT Samba Server 4.7.1
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03


 ========================================( Users on 10.0.88.36 )========================================

index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: david    Name: david     Desc:
index: 0x2 RID: 0x3e9 acb: 0x00000010 Account: rick     Name:   Desc:

user:[david] rid:[0x3e8]
user:[rick] rid:[0x3e9]

 ==================================( Share Enumeration on 10.0.88.36 )==================================


        Sharename       Type      Comment
        ---------       ----      -------
        anonymous       Disk
        secured         Disk
        IPC$            IPC       IPC Service (Samba Server 4.7.1)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            BRAVERY

[+] Attempting to map shares on 10.0.88.36

//10.0.88.36/anonymous  Mapping: OK Listing: OK Writing: N/A
//10.0.88.36/secured    Mapping: DENIED Listing: N/A Writing: N/A

[E] Can't understand response:

NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*
//10.0.88.36/IPC$       Mapping: N/A Listing: N/A Writing: N/A

 =============================( Password Policy Information for 10.0.88.36 )=============================



[+] Attaching to 10.0.88.36 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

        [+] BRAVERY
        [+] Builtin

[+] Password Info for Domain: BRAVERY

        [+] Minimum password length: 5
        [+] Password history length: None
        [+] Maximum password age: 37 days 6 hours 21 minutes
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
        [+] Forced Log off Time: 37 days 6 hours 21 minutes



[+] Retieved partial password policy with rpcclient:


Password Complexity: Disabled
Minimum Password Length: 5


 ========================================( Groups on 10.0.88.36 )========================================


[+] Getting builtin groups:


[+]  Getting builtin group memberships:


[+]  Getting local groups:


[+]  Getting local group memberships:


[+]  Getting domain groups:


[+]  Getting domain group memberships:


 ===================( Users on 10.0.88.36 via RID cycling (RIDS: 500-550,1000-1050) )===================


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

[+] Enumerating users using SID S-1-22-1 and logon username '', password ''

S-1-22-1-1000 Unix User\david (Local User)
S-1-22-1-1001 Unix User\ossec (Local User)
S-1-22-1-1002 Unix User\ossecm (Local User)
S-1-22-1-1003 Unix User\ossecr (Local User)
S-1-22-1-1004 Unix User\rick (Local User)

[+] Enumerating users using SID S-1-5-21-3095209315-73392370-3986610237 and logon username '', password ''

S-1-5-21-3095209315-73392370-3986610237-501 BRAVERY\nobody (Local User)
S-1-5-21-3095209315-73392370-3986610237-513 BRAVERY\None (Domain Group)
S-1-5-21-3095209315-73392370-3986610237-1000 BRAVERY\david (Local User)
S-1-5-21-3095209315-73392370-3986610237-1001 BRAVERY\rick (Local User)

[+] Enumerating users using SID S-1-5-32 and logon username '', password ''

S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)

 ================================( Getting printer info for 10.0.88.36 )================================

No printers returned.


enum4linux complete on Tue Dec 20 21:18:09 2022


┌──(root㉿kali)-[~]
└─# smbclient -N -L 10.0.88.36

        Sharename       Type      Comment
        ---------       ----      -------
        anonymous       Disk
        secured         Disk
        IPC$            IPC       IPC Service (Samba Server 4.7.1)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            BRAVERY

┌──(root㉿kali)-[~]
└─# smbclient -N //10.0.88.36/secured
tree connect failed: NT_STATUS_ACCESS_DENIED

┌──(root㉿kali)-[~]
└─# smbclient -N //10.0.88.36/anonymous
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Fri Sep 28 21:01:35 2018
  ..                                  D        0  Fri Jun 15 00:30:39 2018
  patrick's folder                    D        0  Fri Sep 28 20:38:27 2018
  qiu's folder                        D        0  Fri Sep 28 21:27:20 2018
  genevieve's folder                  D        0  Fri Sep 28 21:08:31 2018
  david's folder                      D        0  Wed Dec 26 10:19:51 2018
  kenny's folder                      D        0  Fri Sep 28 20:52:49 2018
  qinyi's folder                      D        0  Fri Sep 28 20:45:22 2018
  sara's folder                       D        0  Fri Sep 28 21:34:23 2018
  readme.txt                          N      489  Fri Sep 28 21:54:03 2018

                17811456 blocks of size 1024. 13117072 blocks available
smb: \> ^C

┌──(root㉿kali)-[~]
└─# mkdir /mnt/smb

┌──(root㉿kali)-[~]
└─# mount -t cifs //10.0.88.36/anonymous /mnt/smb
Password for root@//10.0.88.36/anonymous:

┌──(root㉿kali)-[~]
└─# find /mnt/smb -type f -size +0
/mnt/smb/patrick's folder/work!/present_for_qiu/present
/mnt/smb/patrick's folder/work!/samba/david_secured_share/readme/readme.txt
/mnt/smb/genevieve's folder/CMS/migration/important!
/mnt/smb/genevieve's folder/email/spear
/mnt/smb/kenny's folder/vuln_assessment_team/windows/XP_disclaimer
/mnt/smb/sara's folder/gossip_corner/gossip5
/mnt/smb/sara's folder/gossip_corner/gossip18
/mnt/smb/sara's folder/gossip_corner/gossip23
/mnt/smb/sara's folder/gossip_corner/gossip27
/mnt/smb/sara's folder/email/2048
/mnt/smb/readme.txt
```

</details>
