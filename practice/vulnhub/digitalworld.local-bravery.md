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
