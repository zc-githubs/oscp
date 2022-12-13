<details>
  <summary><h1>1. Nmap Scan</h1></summary>

```console
┌──(root㉿kali)-[~]
└─# nmap -p- -A 10.0.88.34
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-13 11:34 +08
Nmap scan report for 10.0.88.34
Host is up (0.0010s latency).
Not shown: 65523 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         ProFTPD
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxr-x   2 ftp      ftp          4096 Jan  6  2019 download
|_drwxrwxr-x   2 ftp      ftp          4096 Jan 10  2019 upload
22/tcp  open  ssh         Dropbear sshd 0.34 (protocol 2.0)
25/tcp  open  smtp        Postfix smtpd
|_smtp-commands: JOY.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=JOY
| Subject Alternative Name: DNS:JOY
| Not valid before: 2018-12-23T14:29:24
|_Not valid after:  2028-12-20T14:29:24
80/tcp  open  http        Apache httpd 2.4.25
|_http-title: Index of /
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2016-07-19 20:03  ossec/
|_
|_http-server-header: Apache/2.4.25 (Debian)
110/tcp open  pop3        Dovecot pop3d
| ssl-cert: Subject: commonName=JOY/organizationName=Good Tech Pte. Ltd/stateOrProvinceName=Singapore/countryName=SG
| Not valid before: 2019-01-27T17:23:23
|_Not valid after:  2032-10-05T17:23:23
|_pop3-capabilities: PIPELINING SASL STLS UIDL AUTH-RESP-CODE TOP RESP-CODES CAPA
|_ssl-date: TLS randomness does not represent time
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=JOY/organizationName=Good Tech Pte. Ltd/stateOrProvinceName=Singapore/countryName=SG
| Not valid before: 2019-01-27T17:23:23
|_Not valid after:  2032-10-05T17:23:23
|_imap-capabilities: IMAP4rev1 more Pre-login STARTTLS have LOGIN-REFERRALS IDLE LITERAL+ post-login ID listed ENABLE capabilities OK LOGINDISABLEDA0001 SASL-IR
445/tcp open  netbios-ssn Samba smbd 4.5.12-Debian (workgroup: WORKGROUP)
465/tcp open  smtp        Postfix smtpd
|_smtp-commands: JOY.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=JOY
| Subject Alternative Name: DNS:JOY
| Not valid before: 2018-12-23T14:29:24
|_Not valid after:  2028-12-20T14:29:24
587/tcp open  smtp        Postfix smtpd
|_ssl-date: TLS randomness does not represent time
|_smtp-commands: JOY.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8
| ssl-cert: Subject: commonName=JOY
| Subject Alternative Name: DNS:JOY
| Not valid before: 2018-12-23T14:29:24
|_Not valid after:  2028-12-20T14:29:24
993/tcp open  ssl/imap    Dovecot imapd
| ssl-cert: Subject: commonName=JOY/organizationName=Good Tech Pte. Ltd/stateOrProvinceName=Singapore/countryName=SG
| Not valid before: 2019-01-27T17:23:23
|_Not valid after:  2032-10-05T17:23:23
|_ssl-date: TLS randomness does not represent time
|_imap-capabilities: IMAP4rev1 more Pre-login have LOGIN-REFERRALS IDLE LITERAL+ AUTH=PLAINA0001 ID post-login ENABLE listed capabilities OK SASL-IR
995/tcp open  ssl/pop3    Dovecot pop3d
|_pop3-capabilities: PIPELINING USER SASL(PLAIN) UIDL AUTH-RESP-CODE TOP RESP-CODES CAPA
| ssl-cert: Subject: commonName=JOY/organizationName=Good Tech Pte. Ltd/stateOrProvinceName=Singapore/countryName=SG
| Not valid before: 2019-01-27T17:23:23
|_Not valid after:  2032-10-05T17:23:23
|_ssl-date: TLS randomness does not represent time
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.93%E=4%D=12/13%OT=21%CT=1%CU=39585%PV=Y%DS=2%DC=T%G=Y%TM=6397F2
OS:E7%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=105%TI=Z%TS=8)OPS(O1=M5B4S
OS:T11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5=M5B4ST11NW7%O6=M
OS:5B4ST11)WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=7120)ECN(R=N)T1(R
OS:=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=N)T5(R=Y%DF=Y%T=40
OS:%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=N)T7(R=N)U1(R=Y%DF=N%T=40%IPL=164%UN=
OS:0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=N)

Network Distance: 2 hops
Service Info: Hosts: The,  JOY.localdomain, 127.0.1.1, JOY; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: JOY, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.5.12-Debian)
|   Computer name: joy
|   NetBIOS computer name: JOY\x00
|   Domain name: \x00
|   FQDN: joy
|_  System time: 2022-12-13T11:34:32+08:00
| smb2-security-mode:
|   311:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2022-12-13T03:34:32
|_  start_date: N/A
|_clock-skew: mean: -2h40m00s, deviation: 4h37m07s, median: 0s

TRACEROUTE (using port 1025/tcp)
HOP RTT     ADDRESS
1   0.47 ms 192.168.17.1
2   1.14 ms 10.0.88.34

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 59.13 seconds
```


</details>

# 2. Checking out the FTP

<details>
  <summary><b>Listing and retrieving files</b></summary>

```console
┌──(root㉿kali)-[~]
└─# lftp anonymous:anonymous@10.0.88.34
lftp anonymous@10.0.88.34:~> find
./
./download/
./upload/
./upload/directory
./upload/project_armadillo
./upload/project_bravado
./upload/project_desperado
./upload/project_emilio
./upload/project_flamingo
./upload/project_indigo
./upload/project_komodo
./upload/project_luyano
./upload/project_malindo
./upload/project_okacho
./upload/project_polento
./upload/project_ronaldinho
./upload/project_sicko
./upload/project_toto
./upload/project_uno
./upload/project_vivino
./upload/project_woranto
./upload/project_yolo
./upload/project_zoo
./upload/reminder
lftp anonymous@10.0.88.34:/> mirror upload/
New: 21 files, 0 symlinks
2608 bytes transferred
lftp anonymous@10.0.88.34:/>
```

</details>

<details>
  <summary><b>Reading retrieved files</b></summary>

```console
┌──(root㉿kali)-[~]
└─# tail -n +1 upload/*
==> upload/directory <==
Patrick's Directory

total 112
drwxr-xr-x 18 patrick patrick 4096 Dec 14 07:20 .
drwxr-xr-x  4 root    root    4096 Jan  6  2019 ..
-rw-r--r--  1 patrick patrick   24 Dec 14 07:20 aq3iBV9o5XUEERXqGcwj3wexzmlrjlWmlkSnLvtMCcA7GmXz49pJqrUD4FG8lk9F.txt
-rw-------  1 patrick patrick  185 Jan 28  2019 .bash_history
-rw-r--r--  1 patrick patrick  220 Dec 23  2018 .bash_logout
-rw-r--r--  1 patrick patrick 3526 Dec 23  2018 .bashrc
drwx------  7 patrick patrick 4096 Jan 10  2019 .cache
drwx------ 10 patrick patrick 4096 Dec 26  2018 .config
-rw-r--r--  1 patrick patrick    0 Dec 14 07:20 deL4hBNDbBU2rUqD6IIgAqJm7wAWM4X4.txt
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Desktop
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Documents
drwxr-xr-x  3 patrick patrick 4096 Jan  6  2019 Downloads
-rw-r--r--  1 patrick patrick    0 Dec 14 07:15 DrUFLcBQAoIvGNoONrLy0ZPjEWRUJeJg.txt
drwx------  3 patrick patrick 4096 Dec 26  2018 .gnupg
-rwxrwxrwx  1 patrick patrick    0 Jan  9  2019 haha
-rw-------  1 patrick patrick 8532 Jan 28  2019 .ICEauthority
drwxr-xr-x  3 patrick patrick 4096 Dec 26  2018 .local
drwx------  5 patrick patrick 4096 Dec 28  2018 .mozilla
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Music
drwxr-xr-x  2 patrick patrick 4096 Jan  8  2019 .nano
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Pictures
-rw-r--r--  1 patrick patrick  675 Dec 23  2018 .profile
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Public
d---------  2 root    root    4096 Jan  9  2019 script
drwx------  2 patrick patrick 4096 Dec 26  2018 .ssh
-rw-r--r--  1 patrick patrick    0 Jan  6  2019 Sun
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Templates
-rw-r--r--  1 patrick patrick   24 Dec 14 07:15 TKR7nfdtZQtf2v5D7d2kYD7hbq0EokeFTCMQviV57Iwo1xKSFylNo2iCLODB7oqo.txt
-rw-r--r--  1 patrick patrick    0 Jan  6  2019 .txt
-rw-r--r--  1 patrick patrick  407 Jan 27  2019 version_control
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Videos

You should know where the directory can be accessed.

Information of this Machine!

Linux JOY 4.9.0-8-amd64 #1 SMP Debian 4.9.130-2 (2018-10-27) x86_64 GNU/Linux

==> upload/project_armadillo <==

==> upload/project_bravado <==
This is a brave project!

==> upload/project_desperado <==
What happens when you have no idea what you are doing? Bang your head against the wall.

==> upload/project_emilio <==

==> upload/project_flamingo <==

==> upload/project_indigo <==
colour

==> upload/project_komodo <==

==> upload/project_luyano <==

==> upload/project_malindo <==
airline

==> upload/project_okacho <==

==> upload/project_polento <==

==> upload/project_ronaldinho <==
skilled footballer!

==> upload/project_sicko <==
Perhaps the head of development is secretly a sicko...

==> upload/project_toto <==
either a dog name, or the name of a lottery in singapore

==> upload/project_uno <==
ONE!

==> upload/project_vivino <==
wine app

==> upload/project_woranto <==

==> upload/project_yolo <==
you only live once!

==> upload/project_zoo <==
dog
cat
ant
bird
fish
hare
snake
mouse
eagle
rabbit
jaguar
python
penguin
peacock
phoenix
kangaroo
parakeet
mosquito
mousedeer
woodlouse
cockroach
kingfisher
rhinoceros
pondskater

==> upload/reminder <==
Lock down this machine!
```

</details>

The FTP return several files, but only the `upload/directory` file appear to be interesting

It appears to be the directory listing for Patrick's home directory

There are some hash-like filenames and a file called `version_control` in Patrick's home directory

There appears to be nothing much else to look at now, so let's **try harder** elsewhere
