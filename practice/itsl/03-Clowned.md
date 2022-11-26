# 0. Links

||||
|---|---|---|
|[Reddit](https://www.reddit.com/r/oscp/comments/qi25ax/another_free_practice_windows_machine_i_challenge/)|[YouTube](https://www.youtube.com/watch?v=W57Gw5cdV4M)|[Download](https://drive.google.com/file/d/1xxvmgUgL-uftdJ9noOpxDUN3UZeIcTxU/view)|

# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.47
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-26 07:50 +08
Nmap scan report for 10.0.88.47
Host is up (0.0012s latency).
Not shown: 65520 closed tcp ports (conn-refused)
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
2121/tcp  open  ftp                Microsoft ftpd
| ftp-syst:
|_  SYST: Windows_NT
3000/tcp  open  ppp?
| fingerprint-strings:
|   GenericLines, Help, RTSPRequest:
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest:
|     HTTP/1.0 302 Found
|     Cache-Control: no-cache
|     Content-Type: text/html; charset=utf-8
|     Expires: -1
|     Location: /login
|     Pragma: no-cache
|     Set-Cookie: redirect_to=%252F; Path=/; HttpOnly
|     X-Frame-Options: deny
|     Date: Sat, 26 Nov 2022 14:50:44 GMT
|     Content-Length: 29
|     href="/login">Found</a>.
|   HTTPOptions:
|     HTTP/1.0 404 Not Found
|     Cache-Control: no-cache
|     Content-Type: text/html; charset=UTF-8
|     Expires: -1
|     Pragma: no-cache
|     X-Frame-Options: deny
|     Date: Sat, 26 Nov 2022 14:50:49 GMT
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
|     <meta name="viewport" content="width=device-width">
|     <meta name="theme-color" content="#000">
|     <title>Grafana</title>
|     <base href="/" />
|     <link rel="preload" href="public/fonts/roboto/RxZJdnzeo3R5zSexge8UUVtXRa8TVwTICgirnJhmVJw.woff2" as="font" crossorigin />
|     <link rel="icon" type="image/png" href="public/img/fav32.png">
|     <link rel="mask-icon" href="public/img/grafana_mask_icon.svg" color="#F05A28">
|     <link rel="apple-touch-icon" sizes="180x180" href="public/img/apple-touch-icon.png">
|_    <link rel="stylesheet" href="public/build/grafana.dark.e402a76d33a475eefbc0.css">
3389/tcp  open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=WIN-KBP5VDTN99V
| Not valid before: 2022-11-25T14:47:16
|_Not valid after:  2023-05-27T14:47:16
|_ssl-date: 2022-11-26T14:52:11+00:00; +14h59m59s from scanner time.
| rdp-ntlm-info:
|   Target_Name: WIN-KBP5VDTN99V
|   NetBIOS_Domain_Name: WIN-KBP5VDTN99V
|   NetBIOS_Computer_Name: WIN-KBP5VDTN99V
|   DNS_Domain_Name: WIN-KBP5VDTN99V
|   DNS_Computer_Name: WIN-KBP5VDTN99V
|   Product_Version: 6.2.9200
|_  System_Time: 2022-11-26T14:52:05+00:00
5353/tcp  open  http               Microsoft IIS httpd 8.0
|_http-title: Microsoft Internet Information Services 8
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/8.0
5985/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
47001/tcp open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49158/tcp open  msrpc              Microsoft Windows RPC
49159/tcp open  msrpc              Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.93%I=7%D=11/26%Time=638154D5%P=x86_64-pc-linux-gnu%r(G
SF:enericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20
SF:text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\
SF:x20Request")%r(GetRequest,126,"HTTP/1\.0\x20302\x20Found\r\nCache-Contr
SF:ol:\x20no-cache\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nExpi
SF:res:\x20-1\r\nLocation:\x20/login\r\nPragma:\x20no-cache\r\nSet-Cookie:
SF:\x20redirect_to=%252F;\x20Path=/;\x20HttpOnly\r\nX-Frame-Options:\x20de
SF:ny\r\nDate:\x20Sat,\x2026\x20Nov\x202022\x2014:50:44\x20GMT\r\nContent-
SF:Length:\x2029\r\n\r\n<a\x20href=\"/login\">Found</a>\.\n\n")%r(Help,67,
SF:"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20
SF:charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(
SF:HTTPOptions,15A8,"HTTP/1\.0\x20404\x20Not\x20Found\r\nCache-Control:\x2
SF:0no-cache\r\nContent-Type:\x20text/html;\x20charset=UTF-8\r\nExpires:\x
SF:20-1\r\nPragma:\x20no-cache\r\nX-Frame-Options:\x20deny\r\nDate:\x20Sat
SF:,\x2026\x20Nov\x202022\x2014:50:49\x20GMT\r\n\r\n<!DOCTYPE\x20html>\n<h
SF:tml\x20lang=\"en\">\n\n<head>\n\x20\x20<meta\x20charset=\"utf-8\">\n\x2
SF:0\x20<meta\x20http-equiv=\"X-UA-Compatible\"\x20content=\"IE=edge,chrom
SF:e=1\">\n\x20\x20<meta\x20name=\"viewport\"\x20content=\"width=device-wi
SF:dth\">\n\x20\x20<meta\x20name=\"theme-color\"\x20content=\"#000\">\n\n\
SF:x20\x20<title>Grafana</title>\n\n\x20\x20<base\x20href=\"/\"\x20/>\n\n\
SF:x20\x20<link\x20rel=\"preload\"\x20href=\"public/fonts/roboto/RxZJdnzeo
SF:3R5zSexge8UUVtXRa8TVwTICgirnJhmVJw\.woff2\"\x20as=\"font\"\x20crossorig
SF:in\x20/>\n\x20\x20<link\x20rel=\"icon\"\x20type=\"image/png\"\x20href=\
SF:"public/img/fav32\.png\">\n\x20\x20<link\x20rel=\"mask-icon\"\x20href=\
SF:"public/img/grafana_mask_icon\.svg\"\x20color=\"#F05A28\">\n\x20\x20<li
SF:nk\x20rel=\"apple-touch-icon\"\x20sizes=\"180x180\"\x20href=\"public/im
SF:g/apple-touch-icon\.png\">\n\n\x20\x20<link\x20rel=\"stylesheet\"\x20hr
SF:ef=\"public/build/grafana\.dark\.e402a76d33a475eefbc0\.css\">\n")%r(RTS
SF:PRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20tex
SF:t/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20
SF:Request");
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   300:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2022-11-26T14:52:04
|_  start_date: 2022-11-26T14:47:14
|_nbstat: NetBIOS name: WIN-KBP5VDTN99V, NetBIOS user: <unknown>, NetBIOS MAC: 00155d4ba940 (Microsoft)
| smb-security-mode:
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 14h59m58s, deviation: 0s, median: 14h59m58s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 115.59 seconds
```
