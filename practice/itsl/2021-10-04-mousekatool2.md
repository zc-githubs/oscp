# 0. Sources

|||
|---|---|
|[YouTube](https://www.youtube.com/watch?v=unDGYDe1ljw)|[Download](https://drive.google.com/file/d/1uTJDtFDX9WkhUt-HEuZ5iNS-3CmHMP0m/view)|

# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.34
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-27 08:15 +08
Nmap scan report for 10.0.88.34
Host is up (0.0012s latency).
Not shown: 65515 closed tcp ports (conn-refused)
PORT      STATE SERVICE        VERSION
21/tcp    open  ftp            FileZilla ftpd 0.9.41 beta
| ftp-syst:
|_  SYST: UNIX emulated by FileZilla
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxr-xr-x 1 ftp ftp              0 Sep 14  2021 AccountPictures
| drwxr-xr-x 1 ftp ftp              0 Oct 01  2021 Desktop
| -r--r--r-- 1 ftp ftp            174 Jul 16  2016 desktop.ini
| drwxr-xr-x 1 ftp ftp              0 Sep 14  2021 Documents
| drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Downloads
| drwxr-xr-x 1 ftp ftp              0 Oct 01  2021 FTP
| drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Libraries
| drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Music
| drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Pictures
|_drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Videos
80/tcp    open  http           Apache httpd 2.4.17 ((Win32) OpenSSL/1.0.2d PHP/5.6.14)
| http-title: Welcome to XAMPP
|_Requested resource was http://10.0.88.34/dashboard/
|_http-server-header: Apache/2.4.17 (Win32) OpenSSL/1.0.2d PHP/5.6.14
135/tcp   open  msrpc          Microsoft Windows RPC
139/tcp   open  netbios-ssn    Microsoft Windows netbios-ssn
443/tcp   open  ssl/http       Apache httpd 2.4.17 ((Win32) OpenSSL/1.0.2d PHP/5.6.14)
| tls-alpn:
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
|_http-server-header: Apache/2.4.17 (Win32) OpenSSL/1.0.2d PHP/5.6.14
| http-title: Welcome to XAMPP
|_Requested resource was https://10.0.88.34/dashboard/
445/tcp   open  microsoft-ds   Windows 10 Pro 14393 microsoft-ds (workgroup: ITSL)
1978/tcp  open  unisql?
| fingerprint-strings:
|   DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, JavaRMI, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NULL, RTSPRequest, SIPOptions, SSLSessionReq, TLSSessionReq, ms-sql-s:
|_    SIN 15win nop nop 300
1979/tcp  open  unisql-java?
1980/tcp  open  pearldoc-xact?
3306/tcp  open  mysql          MariaDB (unauthorized)
3389/tcp  open  ms-wbt-server  Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: MOUSEKATOOL2
|   NetBIOS_Domain_Name: MOUSEKATOOL2
|   NetBIOS_Computer_Name: MOUSEKATOOL2
|   DNS_Domain_Name: mousekatool2
|   DNS_Computer_Name: mousekatool2
|   Product_Version: 10.0.14393
|_  System_Time: 2022-11-27T00:17:50+00:00
| ssl-cert: Subject: commonName=mousekatool2
| Not valid before: 2022-11-26T15:13:31
|_Not valid after:  2023-05-28T15:13:31
|_ssl-date: 2022-11-27T00:18:18+00:00; 0s from scanner time.
5357/tcp  open  http           Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
8021/tcp  open  ftp-proxy?
| fingerprint-strings:
|   NULL:
|     Content-Type: text/rude-rejection
|     Content-Length: 24
|     Access Denied, go away.
|     Content-Type: text/disconnect-notice
|     Content-Length: 67
|     Disconnected, goodbye.
|_    ClueCon! http://www.cluecon.com/
49664/tcp open  msrpc          Microsoft Windows RPC
49665/tcp open  msrpc          Microsoft Windows RPC
49666/tcp open  msrpc          Microsoft Windows RPC
49667/tcp open  msrpc          Microsoft Windows RPC
49668/tcp open  msrpc          Microsoft Windows RPC
49669/tcp open  msrpc          Microsoft Windows RPC
49683/tcp open  msrpc          Microsoft Windows RPC
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port1978-TCP:V=7.93%I=7%D=11/27%Time=6382AC17%P=x86_64-pc-linux-gnu%r(N
SF:ULL,15,"SIN\x2015win\x20nop\x20nop\x20300")%r(GenericLines,15,"SIN\x201
SF:5win\x20nop\x20nop\x20300")%r(GetRequest,15,"SIN\x2015win\x20nop\x20nop
SF:\x20300")%r(HTTPOptions,15,"SIN\x2015win\x20nop\x20nop\x20300")%r(RTSPR
SF:equest,15,"SIN\x2015win\x20nop\x20nop\x20300")%r(DNSVersionBindReqTCP,1
SF:5,"SIN\x2015win\x20nop\x20nop\x20300")%r(Help,15,"SIN\x2015win\x20nop\x
SF:20nop\x20300")%r(SSLSessionReq,15,"SIN\x2015win\x20nop\x20nop\x20300")%
SF:r(TLSSessionReq,15,"SIN\x2015win\x20nop\x20nop\x20300")%r(FourOhFourReq
SF:uest,15,"SIN\x2015win\x20nop\x20nop\x20300")%r(LPDString,15,"SIN\x2015w
SF:in\x20nop\x20nop\x20300")%r(LDAPSearchReq,15,"SIN\x2015win\x20nop\x20no
SF:p\x20300")%r(LDAPBindReq,15,"SIN\x2015win\x20nop\x20nop\x20300")%r(SIPO
SF:ptions,15,"SIN\x2015win\x20nop\x20nop\x20300")%r(LANDesk-RC,15,"SIN\x20
SF:15win\x20nop\x20nop\x20300")%r(JavaRMI,15,"SIN\x2015win\x20nop\x20nop\x
SF:20300")%r(ms-sql-s,15,"SIN\x2015win\x20nop\x20nop\x20300");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port8021-TCP:V=7.93%I=7%D=11/27%Time=6382AC11%P=x86_64-pc-linux-gnu%r(N
SF:ULL,CA,"Content-Type:\x20text/rude-rejection\nContent-Length:\x2024\n\n
SF:Access\x20Denied,\x20go\x20away\.\nContent-Type:\x20text/disconnect-not
SF:ice\nContent-Length:\x2067\n\nDisconnected,\x20goodbye\.\nSee\x20you\x2
SF:0at\x20ClueCon!\x20http://www\.cluecon\.com/\n");
Service Info: Host: MOUSEKATOOL2; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 1h36m00s, deviation: 3h34m42s, median: 0s
| smb-os-discovery:
|   OS: Windows 10 Pro 14393 (Windows 10 Pro 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: mousekatool2
|   NetBIOS computer name: MOUSEKATOOL2\x00
|   Workgroup: ITSL\x00
|_  System time: 2022-11-26T16:17:54-08:00
| smb2-time:
|   date: 2022-11-27T00:17:50
|_  start_date: 2022-11-27T00:13:34

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 191.98 seconds
```
