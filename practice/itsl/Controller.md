# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.50
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-26 07:50 +08
Nmap scan report for 10.0.88.50
Host is up (0.018s latency).
Not shown: 65509 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-11-25 23:50:43Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: windomain.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds (workgroup: WINDOMAIN)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: windomain.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2022-11-25T23:52:28+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=dc.windomain.local
| Not valid before: 2022-08-06T03:13:07
|_Not valid after:  2023-02-05T03:13:07
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
8080/tcp  open  http-proxy
|_http-title: 401 Access Denied
| http-auth:
| HTTP/1.1 401 Access Denied\x0D
|_  Digest opaque=Yicke2NvabGm6RA0QmLyRpSOvK1HsBSr8X nonce=moJd1F/r5UBo2REBX+vlQA== qop=auth realm=ThinVNC
| fingerprint-strings:
|   FourOhFourRequest:
|     HTTP/1.1 404 Not Found
|     Content-Type: text/html
|     Content-Length: 177
|     Connection: Keep-Alive
|     <HTML><HEAD><TITLE>404 Not Found</TITLE></HEAD><BODY><H1>404 Not Found</H1>The requested URL nice%20ports%2C/Tri%6Eity.txt%2ebak was not found on this server.<P></BODY></HTML>
|   GetRequest:
|     HTTP/1.1 401 Access Denied
|     Content-Type: text/html
|     Content-Length: 144
|     Connection: Keep-Alive
|     WWW-Authenticate: Digest realm="ThinVNC", qop="auth", nonce="xL9Ey1/r5UAo1xEBX+vlQA==", opaque="8GcQ58c314gPrMFqOCtY168jbSPTdubBE3"
|_    <HTML><HEAD><TITLE>401 Access Denied</TITLE></HEAD><BODY><H1>401 Access Denied</H1>The requested URL requires authorization.<P></BODY></HTML>
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
49681/tcp open  msrpc         Microsoft Windows RPC
49734/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.93%I=7%D=11/26%Time=638154D8%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,179,"HTTP/1\.1\x20401\x20Access\x20Denied\r\nContent-Type:\x2
SF:0text/html\r\nContent-Length:\x20144\r\nConnection:\x20Keep-Alive\r\nWW
SF:W-Authenticate:\x20Digest\x20realm=\"ThinVNC\",\x20qop=\"auth\",\x20non
SF:ce=\"xL9Ey1/r5UAo1xEBX\+vlQA==\",\x20opaque=\"8GcQ58c314gPrMFqOCtY168jb
SF:SPTdubBE3\"\r\n\r\n<HTML><HEAD><TITLE>401\x20Access\x20Denied</TITLE></
SF:HEAD><BODY><H1>401\x20Access\x20Denied</H1>The\x20requested\x20URL\x20\
SF:x20requires\x20authorization\.<P></BODY></HTML>\r\n")%r(FourOhFourReque
SF:st,111,"HTTP/1\.1\x20404\x20Not\x20Found\r\nContent-Type:\x20text/html\
SF:r\nContent-Length:\x20177\r\nConnection:\x20Keep-Alive\r\n\r\n<HTML><HE
SF:AD><TITLE>404\x20Not\x20Found</TITLE></HEAD><BODY><H1>404\x20Not\x20Fou
SF:nd</H1>The\x20requested\x20URL\x20nice%20ports%2C/Tri%6Eity\.txt%2ebak\
SF:x20was\x20not\x20found\x20on\x20this\x20server\.<P></BODY></HTML>\r\n");
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode:
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
|_clock-skew: mean: 0s, deviation: 1s, median: 0s
| smb2-security-mode:
|   311:
|_    Message signing enabled and required
| smb-os-discovery:
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: dc
|   NetBIOS computer name: DC\x00
|   Domain name: windomain.local
|   Forest name: windomain.local
|   FQDN: dc.windomain.local
|_  System time: 2022-11-25T23:52:21+00:00
| smb2-time:
|   date: 2022-11-25T23:52:20
|_  start_date: 2022-11-25T23:47:54

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 120.18 seconds
```
