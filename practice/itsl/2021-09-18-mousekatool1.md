# 0. Sources

|||
|---|---|
|[YouTube](https://www.youtube.com/watch?v=MD5GIbatPg8)|[Download](https://drive.google.com/file/d/1K5_YA76DVtICHst2FlLVjq68dgNkMgRX/view)|

# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.33
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-27 08:15 +08
Nmap scan report for 10.0.88.33
Host is up (0.017s latency).
Not shown: 65524 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 10 Pro 14393 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2022-11-27T00:16:10+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=mousekatoo
| Not valid before: 2022-11-26T00:13:17
|_Not valid after:  2023-05-28T00:13:17
| rdp-ntlm-info:
|   Target_Name: MOUSEKATOO
|   NetBIOS_Domain_Name: MOUSEKATOO
|   NetBIOS_Computer_Name: MOUSEKATOO
|   DNS_Domain_Name: mousekatoo
|   DNS_Computer_Name: mousekatoo
|   Product_Version: 10.0.14393
|_  System_Time: 2022-11-27T00:16:01+00:00
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49690/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: MOUSEKATOO; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h36m00s, deviation: 3h34m42s, median: 0s
| smb2-time:
|   date: 2022-11-27T00:16:02
|_  start_date: 2022-11-27T00:13:17
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
| smb-os-discovery:
|   OS: Windows 10 Pro 14393 (Windows 10 Pro 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: mousekatoo
|   NetBIOS computer name: MOUSEKATOO\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2022-11-26T16:16:06-08:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 68.49 seconds
```
