# 1. nmap scan

```console
┌──(kali㉿kali)-[~]
└─$ nmap -p- -A 10.0.88.46
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-26 07:50 +08
Nmap scan report for 10.0.88.46
Host is up (0.013s latency).
Not shown: 65523 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 10 Pro 14393 microsoft-ds (workgroup: ITSL)
3306/tcp  open  mysql         MariaDB (unauthorized)
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: CARS
|   NetBIOS_Domain_Name: CARS
|   NetBIOS_Computer_Name: CARS
|   DNS_Domain_Name: cars
|   DNS_Computer_Name: cars
|   Product_Version: 10.0.14393
|_  System_Time: 2022-11-25T23:51:12+00:00
| ssl-cert: Subject: commonName=cars
| Not valid before: 2022-11-24T23:47:01
|_Not valid after:  2023-05-26T23:47:01
|_ssl-date: 2022-11-25T23:51:21+00:00; 0s from scanner time.
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49695/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: CARS; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery:
|   OS: Windows 10 Pro 14393 (Windows 10 Pro 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: cars
|   NetBIOS computer name: CARS\x00
|   Workgroup: ITSL\x00
|_  System time: 2022-11-25T15:51:13-08:00
|_clock-skew: mean: 1h35m59s, deviation: 3h34m40s, median: 0s
| smb-security-mode:
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2022-11-25T23:51:17
|_  start_date: 2022-11-25T23:47:01

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 68.93 seconds
```
