# 0. Links

||||
|---|---|---|
|[Reddit](https://www.reddit.com/r/oscp/comments/qsde6o/oscp_like_windows_vm_untroubled_can_you_crack/)|[YouTube](https://www.youtube.com/watch?v=FhXPLXfbPdo)|[Download](https://drive.google.com/file/d/1j2XB-dRoeaSps_rgov4NUiqnTvig58oL/view)|

# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.48
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-26 07:50 +08
Nmap scan report for 10.0.88.48
Host is up (0.049s latency).
Not shown: 65521 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
21/tcp    open  ftp           FileZilla ftpd 0.9.41 beta
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
| ftp-syst:
|_  SYST: UNIX emulated by FileZilla
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 10 Pro 14393 microsoft-ds (workgroup: ITSL)
2390/tcp  open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows
|_http-server-header: Microsoft-IIS/10.0
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: PRINTPRESS
|   NetBIOS_Domain_Name: PRINTPRESS
|   NetBIOS_Computer_Name: PRINTPRESS
|   DNS_Domain_Name: PrintPress
|   DNS_Computer_Name: PrintPress
|   Product_Version: 10.0.14393
|_  System_Time: 2022-11-25T23:51:22+00:00
| ssl-cert: Subject: commonName=PrintPress
| Not valid before: 2022-11-24T23:47:21
|_Not valid after:  2023-05-26T23:47:21
|_ssl-date: 2022-11-25T23:51:31+00:00; 0s from scanner time.
8021/tcp  open  ftp-proxy?
| fingerprint-strings:
|   Kerberos, NULL, SSLSessionReq, TLSSessionReq, WMSRequest:
|     Content-Type: text/rude-rejection
|     Content-Length: 24
|     Access Denied, go away.
|     Content-Type: text/disconnect-notice
|     Content-Length: 67
|     Disconnected, goodbye.
|_    ClueCon! http://www.cluecon.com/
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49678/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8021-TCP:V=7.93%I=7%D=11/26%Time=638154C5%P=x86_64-pc-linux-gnu%r(N
SF:ULL,CA,"Content-Type:\x20text/rude-rejection\nContent-Length:\x2024\n\n
SF:Access\x20Denied,\x20go\x20away\.\nContent-Type:\x20text/disconnect-not
SF:ice\nContent-Length:\x2067\n\nDisconnected,\x20goodbye\.\nSee\x20you\x2
SF:0at\x20ClueCon!\x20http://www\.cluecon\.com/\n")%r(SSLSessionReq,CA,"Co
SF:ntent-Type:\x20text/rude-rejection\nContent-Length:\x2024\n\nAccess\x20
SF:Denied,\x20go\x20away\.\nContent-Type:\x20text/disconnect-notice\nConte
SF:nt-Length:\x2067\n\nDisconnected,\x20goodbye\.\nSee\x20you\x20at\x20Clu
SF:eCon!\x20http://www\.cluecon\.com/\n")%r(TLSSessionReq,CA,"Content-Type
SF::\x20text/rude-rejection\nContent-Length:\x2024\n\nAccess\x20Denied,\x2
SF:0go\x20away\.\nContent-Type:\x20text/disconnect-notice\nContent-Length:
SF:\x2067\n\nDisconnected,\x20goodbye\.\nSee\x20you\x20at\x20ClueCon!\x20h
SF:ttp://www\.cluecon\.com/\n")%r(Kerberos,CA,"Content-Type:\x20text/rude-
SF:rejection\nContent-Length:\x2024\n\nAccess\x20Denied,\x20go\x20away\.\n
SF:Content-Type:\x20text/disconnect-notice\nContent-Length:\x2067\n\nDisco
SF:nnected,\x20goodbye\.\nSee\x20you\x20at\x20ClueCon!\x20http://www\.clue
SF:con\.com/\n")%r(WMSRequest,CA,"Content-Type:\x20text/rude-rejection\nCo
SF:ntent-Length:\x2024\n\nAccess\x20Denied,\x20go\x20away\.\nContent-Type:
SF:\x20text/disconnect-notice\nContent-Length:\x2067\n\nDisconnected,\x20g
SF:oodbye\.\nSee\x20you\x20at\x20ClueCon!\x20http://www\.cluecon\.com/\n");
Service Info: Host: PRINTPRESS; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h36m00s, deviation: 3h34m40s, median: 0s
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2022-11-25T23:51:26
|_  start_date: 2022-11-25T23:47:21
| smb-os-discovery:
|   OS: Windows 10 Pro 14393 (Windows 10 Pro 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: PrintPress
|   NetBIOS computer name: PRINTPRESS\x00
|   Workgroup: ITSL\x00
|_  System time: 2022-11-25T15:51:24-08:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 71.46 seconds
```
