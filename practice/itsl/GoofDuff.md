# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.45
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-26 07:50 +08
Nmap scan report for 10.0.88.45
Host is up (0.017s latency).
Not shown: 65520 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
21/tcp    open  ftp           FileZilla ftpd 0.9.41 beta
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxr-xr-x 1 ftp ftp              0 Sep 14  2021 AccountPictures
| drwxr-xr-x 1 ftp ftp              0 Oct 01  2021 Desktop
| -r--r--r-- 1 ftp ftp            174 Jul 16  2016 desktop.ini
| drwxr-xr-x 1 ftp ftp              0 Sep 14  2021 Documents
| drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Downloads
| drwxr-xr-x 1 ftp ftp              0 Oct 14  2021 FTP
| drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Libraries
| drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Music
| drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Pictures
|_drwxr-xr-x 1 ftp ftp              0 Jul 16  2016 Videos
| ftp-syst:
|_  SYST: UNIX emulated by FileZilla
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-title: IIS Windows
|_http-server-header: Microsoft-IIS/10.0
| http-methods:
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 10 Pro 14393 microsoft-ds (workgroup: ITSL)
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: GOOFDUFF
|   NetBIOS_Domain_Name: GOOFDUFF
|   NetBIOS_Computer_Name: GOOFDUFF
|   DNS_Domain_Name: GoofDuff
|   DNS_Computer_Name: GoofDuff
|   Product_Version: 10.0.14393
|_  System_Time: 2022-11-25T23:51:07+00:00
|_ssl-date: 2022-11-25T23:51:15+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=GoofDuff
| Not valid before: 2022-11-24T23:46:51
|_Not valid after:  2023-05-26T23:46:51
8021/tcp  open  ftp-proxy?
| fingerprint-strings:
|   FourOhFourRequest, NULL, SSLSessionReq:
|     Content-Type: text/rude-rejection
|     Content-Length: 24
|     Access Denied, go away.
|     Content-Type: text/disconnect-notice
|     Content-Length: 67
|     Disconnected, goodbye.
|_    ClueCon! http://www.cluecon.com/
9450/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| http-methods:
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49688/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8021-TCP:V=7.93%I=7%D=11/26%Time=638154B5%P=x86_64-pc-linux-gnu%r(N
SF:ULL,CA,"Content-Type:\x20text/rude-rejection\nContent-Length:\x2024\n\n
SF:Access\x20Denied,\x20go\x20away\.\nContent-Type:\x20text/disconnect-not
SF:ice\nContent-Length:\x2067\n\nDisconnected,\x20goodbye\.\nSee\x20you\x2
SF:0at\x20ClueCon!\x20http://www\.cluecon\.com/\n")%r(SSLSessionReq,CA,"Co
SF:ntent-Type:\x20text/rude-rejection\nContent-Length:\x2024\n\nAccess\x20
SF:Denied,\x20go\x20away\.\nContent-Type:\x20text/disconnect-notice\nConte
SF:nt-Length:\x2067\n\nDisconnected,\x20goodbye\.\nSee\x20you\x20at\x20Clu
SF:eCon!\x20http://www\.cluecon\.com/\n")%r(FourOhFourRequest,CA,"Content-
SF:Type:\x20text/rude-rejection\nContent-Length:\x2024\n\nAccess\x20Denied
SF:,\x20go\x20away\.\nContent-Type:\x20text/disconnect-notice\nContent-Len
SF:gth:\x2067\n\nDisconnected,\x20goodbye\.\nSee\x20you\x20at\x20ClueCon!\
SF:x20http://www\.cluecon\.com/\n");
Service Info: Host: GOOFDUFF; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time:
|   date: 2022-11-25T23:51:09
|_  start_date: 2022-11-25T23:46:51
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
| smb-os-discovery:
|   OS: Windows 10 Pro 14393 (Windows 10 Pro 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: GoofDuff
|   NetBIOS computer name: GOOFDUFF\x00
|   Workgroup: ITSL\x00
|_  System time: 2022-11-25T15:51:08-08:00
|_clock-skew: mean: 1h36m00s, deviation: 3h34m40s, median: 0s
| smb-security-mode:
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 68.83 seconds
```
