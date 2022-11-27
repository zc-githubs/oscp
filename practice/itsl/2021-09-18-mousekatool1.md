# 0. Sources

|||
|---|---|
|[YouTube](https://www.youtube.com/watch?v=MD5GIbatPg8)|[Download](https://drive.google.com/file/d/1K5_YA76DVtICHst2FlLVjq68dgNkMgRX/view)|

# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.33
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-27 10:40 +08
Nmap scan report for 10.0.88.33
Host is up (0.017s latency).
Not shown: 65522 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 10 Pro 14393 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=mousekatoo
| Not valid before: 2022-11-26T02:39:44
|_Not valid after:  2023-05-28T02:39:44
|_ssl-date: 2022-11-27T02:42:53+00:00; 0s from scanner time.
| rdp-ntlm-info:
|   Target_Name: MOUSEKATOO
|   NetBIOS_Domain_Name: MOUSEKATOO
|   NetBIOS_Computer_Name: MOUSEKATOO
|   DNS_Domain_Name: mousekatoo
|   DNS_Computer_Name: mousekatoo
|   Product_Version: 10.0.14393
|_  System_Time: 2022-11-27T02:42:39+00:00
5357/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
8039/tcp  open  unknown
| fingerprint-strings:
|   FourOhFourRequest, GetRequest, HTTPOptions, RTSPRequest, SIPOptions:
|     HTTP/1.1 404 OK
|     Server: bruce_wy/1.0.0
|     Access-Control-Allow-Methods: POST,GET,TRACE,OPTIONS
|     Access-Control-Allow-Headers: Content-Type,Origin,Accept
|     Access-Control-Allow-Origin: *
|     Access-Control-Allow-Credentials: true
|     P3P: CP=CAO PSA OUR
|     Content-Type: text/html
|     Content-Range: bytes 0-0/-1
|_    Content-Length : 4294967295
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49689/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8039-TCP:V=7.93%I=7%D=11/27%Time=6382CE29%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,14B,"HTTP/1\.1\x20404\x20OK\r\nServer:\x20bruce_wy/1\.0\.0\r\
SF:nAccess-Control-Allow-Methods:\x20POST,GET,TRACE,OPTIONS\r\nAccess-Cont
SF:rol-Allow-Headers:\x20Content-Type,Origin,Accept\r\nAccess-Control-Allo
SF:w-Origin:\x20\*\r\nAccess-Control-Allow-Credentials:\x20true\r\nP3P:\x2
SF:0CP=CAO\x20PSA\x20OUR\r\nContent-Type:\x20text/html\r\nContent-Range:\x
SF:20bytes\x200-0/-1\r\nContent-Length\x20:\x204294967295\r\n\r\n")%r(HTTP
SF:Options,14B,"HTTP/1\.1\x20404\x20OK\r\nServer:\x20bruce_wy/1\.0\.0\r\nA
SF:ccess-Control-Allow-Methods:\x20POST,GET,TRACE,OPTIONS\r\nAccess-Contro
SF:l-Allow-Headers:\x20Content-Type,Origin,Accept\r\nAccess-Control-Allow-
SF:Origin:\x20\*\r\nAccess-Control-Allow-Credentials:\x20true\r\nP3P:\x20C
SF:P=CAO\x20PSA\x20OUR\r\nContent-Type:\x20text/html\r\nContent-Range:\x20
SF:bytes\x200-0/-1\r\nContent-Length\x20:\x204294967295\r\n\r\n")%r(RTSPRe
SF:quest,14B,"HTTP/1\.1\x20404\x20OK\r\nServer:\x20bruce_wy/1\.0\.0\r\nAcc
SF:ess-Control-Allow-Methods:\x20POST,GET,TRACE,OPTIONS\r\nAccess-Control-
SF:Allow-Headers:\x20Content-Type,Origin,Accept\r\nAccess-Control-Allow-Or
SF:igin:\x20\*\r\nAccess-Control-Allow-Credentials:\x20true\r\nP3P:\x20CP=
SF:CAO\x20PSA\x20OUR\r\nContent-Type:\x20text/html\r\nContent-Range:\x20by
SF:tes\x200-0/-1\r\nContent-Length\x20:\x204294967295\r\n\r\n")%r(FourOhFo
SF:urRequest,14B,"HTTP/1\.1\x20404\x20OK\r\nServer:\x20bruce_wy/1\.0\.0\r\
SF:nAccess-Control-Allow-Methods:\x20POST,GET,TRACE,OPTIONS\r\nAccess-Cont
SF:rol-Allow-Headers:\x20Content-Type,Origin,Accept\r\nAccess-Control-Allo
SF:w-Origin:\x20\*\r\nAccess-Control-Allow-Credentials:\x20true\r\nP3P:\x2
SF:0CP=CAO\x20PSA\x20OUR\r\nContent-Type:\x20text/html\r\nContent-Range:\x
SF:20bytes\x200-0/-1\r\nContent-Length\x20:\x204294967295\r\n\r\n")%r(SIPO
SF:ptions,14B,"HTTP/1\.1\x20404\x20OK\r\nServer:\x20bruce_wy/1\.0\.0\r\nAc
SF:cess-Control-Allow-Methods:\x20POST,GET,TRACE,OPTIONS\r\nAccess-Control
SF:-Allow-Headers:\x20Content-Type,Origin,Accept\r\nAccess-Control-Allow-O
SF:rigin:\x20\*\r\nAccess-Control-Allow-Credentials:\x20true\r\nP3P:\x20CP
SF:=CAO\x20PSA\x20OUR\r\nContent-Type:\x20text/html\r\nContent-Range:\x20b
SF:ytes\x200-0/-1\r\nContent-Length\x20:\x204294967295\r\n\r\n");
Service Info: Host: MOUSEKATOO; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time:
|   date: 2022-11-27T02:42:42
|_  start_date: 2022-11-27T02:39:44
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
|_clock-skew: mean: 1h36m00s, deviation: 3h34m41s, median: 0s
| smb-os-discovery:
|   OS: Windows 10 Pro 14393 (Windows 10 Pro 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: mousekatoo
|   NetBIOS computer name: MOUSEKATOO\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2022-11-26T18:42:43-08:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 145.22 seconds
```
