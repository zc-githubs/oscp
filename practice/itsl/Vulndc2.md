# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.49
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-26 07:50 +08
Nmap scan report for 10.0.88.49
Host is up (0.055s latency).
Not shown: 65507 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/7.4.27)
|_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/7.4.27
| http-robots.txt: 1 disallowed entry
|_1
|_http-generator: RiteCMS 3.0
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-title: RiteCMS 3.0 demo - home
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-11-25 23:50:40Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: windomain.local, Site: Default-First-Site-Name)
443/tcp   open  ssl/http      Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/7.4.27)
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-generator: RiteCMS 3.0
| http-robots.txt: 1 disallowed entry
|_1
|_http-title: RiteCMS 3.0 demo - home
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_ssl-date: TLS randomness does not represent time
| tls-alpn:
|_  http/1.1
|_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/7.4.27
445/tcp   open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds (workgroup: WINDOMAIN)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: windomain.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3306/tcp  open  mysql?
| fingerprint-strings:
|   DNSStatusRequestTCP, Help, NULL, SIPOptions, SSLSessionReq, TLSSessionReq:
|_    Host '192.168.17.10' is not allowed to connect to this MariaDB server
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: WINDOMAIN
|   NetBIOS_Domain_Name: WINDOMAIN
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: windomain.local
|   DNS_Computer_Name: dc.windomain.local
|   DNS_Tree_Name: windomain.local
|   Product_Version: 10.0.14393
|_  System_Time: 2022-11-25T23:51:35+00:00
| ssl-cert: Subject: commonName=dc.windomain.local
| Not valid before: 2022-11-24T23:47:36
|_Not valid after:  2023-05-26T23:47:36
|_ssl-date: 2022-11-25T23:51:43+00:00; 0s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49737/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3306-TCP:V=7.93%I=7%D=11/26%Time=638154CA%P=x86_64-pc-linux-gnu%r(N
SF:ULL,4C,"H\0\0\x01\xffj\x04Host\x20'192\.168\.17\.10'\x20is\x20not\x20al
SF:lowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(DNSStatus
SF:RequestTCP,4C,"H\0\0\x01\xffj\x04Host\x20'192\.168\.17\.10'\x20is\x20no
SF:t\x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(He
SF:lp,4C,"H\0\0\x01\xffj\x04Host\x20'192\.168\.17\.10'\x20is\x20not\x20all
SF:owed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(SSLSession
SF:Req,4C,"H\0\0\x01\xffj\x04Host\x20'192\.168\.17\.10'\x20is\x20not\x20al
SF:lowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(TLSSessio
SF:nReq,4C,"H\0\0\x01\xffj\x04Host\x20'192\.168\.17\.10'\x20is\x20not\x20a
SF:llowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(SIPOptio
SF:ns,4C,"H\0\0\x01\xffj\x04Host\x20'192\.168\.17\.10'\x20is\x20not\x20all
SF:owed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server");
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   311:
|_    Message signing enabled and required
| smb2-time:
|   date: 2022-11-25T23:51:35
|_  start_date: 2022-11-25T23:47:45
| smb-security-mode:
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
|_clock-skew: mean: 0s, deviation: 1s, median: 0s
| smb-os-discovery:
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: dc
|   NetBIOS computer name: DC\x00
|   Domain name: windomain.local
|   Forest name: windomain.local
|   FQDN: dc.windomain.local
|_  System time: 2022-11-25T23:51:36+00:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 81.77 seconds
```
