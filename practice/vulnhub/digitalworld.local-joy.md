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

<details>
  <summary><h1>3. Nmap Scan (UDP)</h1></summary>

```console
┌──(root㉿kali)-[~]
└─# nmap -sU -A --top-ports 100 10.0.88.34
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-14 08:07 +08
Nmap scan report for 10.0.88.34
Host is up (0.0011s latency).
Not shown: 94 closed udp ports (port-unreach)
PORT     STATE         SERVICE     VERSION
68/udp   open|filtered dhcpc
123/udp  open          ntp         NTP v4 (secondary server)
| ntp-info:
|_
137/udp  open          netbios-ns  Samba nmbd netbios-ns (workgroup: WORKGROUP)
| nbns-interfaces:
|   hostname: JOY
|   interfaces:
|_    10.0.88.34
138/udp  open|filtered netbios-dgm
161/udp  open          snmp        SNMPv1 server; net-snmp SNMPv3 server (public)
| snmp-interfaces:
|   lo
|     IP address: 127.0.0.1  Netmask: 255.0.0.0
|     Type: softwareLoopback  Speed: 10 Mbps
|     Traffic stats: 3.43 Kb sent, 3.43 Kb received
|   Microsoft Corporation Device 0003
|     IP address: 10.0.88.34  Netmask: 255.255.255.0
|     MAC address: 00155d4ba992 (Microsoft)
|     Type: ethernetCsmacd  Speed: 4 Gbps
|_    Traffic stats: 4.12 Mb sent, 357.40 Mb received
| snmp-processes:
|   1:
|     Name: systemd
|     Path: /lib/systemd/systemd
|     Params: --system --deserialize 23
|   2:
|     Name: kthreadd
|   3:
|     Name: ksoftirqd/0
|   5:
|     Name: kworker/0:0H
|   7:
|     Name: rcu_sched
|   8:
|     Name: rcu_bh
|   9:
|     Name: migration/0
|   10:
|     Name: lru-add-drain
|   11:
|     Name: watchdog/0
|   12:
|     Name: cpuhp/0
|   13:
|     Name: kdevtmpfs
|   14:
|     Name: netns
|   15:
|     Name: khungtaskd
|   16:
|     Name: oom_reaper
|   17:
|     Name: writeback
|   18:
|     Name: kcompactd0
|   19:
|     Name: ksmd
|   21:
|     Name: khugepaged
|   22:
|     Name: crypto
|   23:
|     Name: kintegrityd
|   24:
|     Name: bioset
|   25:
|     Name: kblockd
|   26:
|     Name: devfreq_wq
|   27:
|     Name: watchdogd
|   28:
|     Name: kswapd0
|   29:
|     Name: vmstat
|   41:
|     Name: kthrotld
|   43:
|     Name: ipv6_addrconf
|   86:
|     Name: bioset
|   87:
|     Name: hv_vmbus_con
|   88:
|     Name: bioset
|   89:
|     Name: bioset
|   90:
|     Name: bioset
|   91:
|     Name: ata_sff
|   92:
|     Name: bioset
|   94:
|     Name: scsi_eh_0
|   95:
|     Name: bioset
|   97:
|     Name: scsi_tmf_0
|   98:
|     Name: bioset
|   99:
|     Name: scsi_eh_1
|   101:
|     Name: scsi_tmf_1
|   102:
|     Name: bioset
|   104:
|     Name: scsi_eh_2
|   105:
|     Name: kworker/u2:1
|   107:
|     Name: scsi_tmf_2
|   108:
|     Name: kworker/u2:2
|   115:
|     Name: bioset
|   133:
|     Name: kworker/0:1H
|   161:
|     Name: kworker/u3:0
|   170:
|     Name: jbd2/sda1-8
|   171:
|     Name: ext4-rsv-conver
|   201:
|     Name: systemd-journal
|     Path: /lib/systemd/systemd-journald
|   206:
|     Name: kauditd
|   272:
|     Name: hv_balloon
|   377:
|     Name: ModemManager
|     Path: /usr/sbin/ModemManager
|   378:
|     Name: systemd-logind
|     Path: /lib/systemd/systemd-logind
|   379:
|     Name: accounts-daemon
|     Path: /usr/lib/accountsservice/accounts-daemon
|   382:
|     Name: dbus-daemon
|     Path: /usr/bin/dbus-daemon
|     Params: --system --address=systemd: --nofork --nopidfile --systemd-activation
|   403:
|     Name: NetworkManager
|     Path: /usr/sbin/NetworkManager
|     Params: --no-daemon
|   407:
|     Name: rtkit-daemon
|     Path: /usr/lib/rtkit/rtkit-daemon
|   593:
|     Name: dhclient
|     Path: /sbin/dhclient
|     Params: -d -q -sf /usr/lib/NetworkManager/nm-dhcp-helper -pf /var/run/dhclient-eth0.pid -lf /var/lib/NetworkManager/dhclient-784d0bd9-f4
|   751:
|     Name: in.tftpd
|     Path: /usr/sbin/in.tftpd
|     Params: --listen --user tftp --address 0.0.0.0:36969 --secure /home/patrick
|   755:
|     Name: proftpd
|     Path: proftpd: (accepting connections)
|   760:
|     Name: dropbear
|     Path: dropbear
|   771:
|     Name: gdm3
|     Path: /usr/sbin/gdm3
|   779:
|     Name: ntpd
|     Path: /usr/sbin/ntpd
|     Params: -p /var/run/ntpd.pid -g -u 121:126
|   864:
|     Name: ossec-execd
|     Path: /var/ossec/bin/ossec-execd
|   874:
|     Name: ossec-analysisd
|     Path: /var/ossec/bin/ossec-analysisd
|   888:
|     Name: ossec-logcollec
|     Path: /var/ossec/bin/ossec-logcollector
|   906:
|     Name: ossec-syscheckd
|     Path: /var/ossec/bin/ossec-syscheckd
|   918:
|     Name: ossec-monitord
|     Path: /var/ossec/bin/ossec-monitord
|   1058:
|     Name: master
|     Path: /usr/lib/postfix/sbin/master
|     Params: -w
|   1061:
|     Name: pickup
|     Path: pickup
|     Params: -l -t unix -u -c
|   1062:
|     Name: qmgr
|     Path: qmgr
|     Params: -l -t unix -u
|   1092:
|     Name: gdm-session-wor
|     Path: gdm-session-worker [pam/gdm-launch-environment]
|   1113:
|     Name: systemd
|     Path: /lib/systemd/systemd
|     Params: --user
|   1115:
|     Name: (sd-pam)
|     Path: (sd-pam)
|   1122:
|     Name: gdm-x-session
|     Path: /usr/lib/gdm3/gdm-x-session
|     Params: gnome-session --autostart /usr/share/gdm/greeter/autostart
|   1125:
|     Name: Xorg
|     Path: /usr/lib/xorg/Xorg
|     Params: vt1 -displayfd 3 -auth /run/user/116/gdm/Xauthority -background none -noreset -keeptty -verbose 3
|   1152:
|     Name: dbus-daemon
|     Path: /usr/bin/dbus-daemon
|     Params: --session --address=systemd: --nofork --nopidfile --systemd-activation
|   1158:
|     Name: gnome-session-b
|     Path: /usr/lib/gnome-session/gnome-session-binary
|     Params: --autostart /usr/share/gdm/greeter/autostart
|   1168:
|     Name: at-spi-bus-laun
|     Path: /usr/lib/at-spi2-core/at-spi-bus-launcher
|   1173:
|     Name: dbus-daemon
|     Path: /usr/bin/dbus-daemon
|     Params: --config-file=/usr/share/defaults/at-spi2/accessibility.conf --nofork --print-address 3
|   1176:
|     Name: at-spi2-registr
|     Path: /usr/lib/at-spi2-core/at-spi2-registryd
|     Params: --use-gnome-session
|   1185:
|     Name: gnome-shell
|     Path: /usr/bin/gnome-shell
|   1190:
|     Name: upowerd
|     Path: /usr/lib/upower/upowerd
|   1196:
|     Name: pulseaudio
|     Path: /usr/bin/pulseaudio
|     Params: --daemonize=no
|   1269:
|     Name: gnome-settings-
|     Path: /usr/lib/gnome-settings-daemon/gnome-settings-daemon
|   1276:
|     Name: wpa_supplicant
|     Path: /sbin/wpa_supplicant
|     Params: -u -s -O /run/wpa_supplicant
|   1283:
|     Name: colord
|     Path: /usr/lib/colord/colord
|   4203:
|     Name: cron
|     Path: /usr/sbin/cron
|     Params: -f
|   4249:
|     Name: rsyslogd
|     Path: /usr/sbin/rsyslogd
|     Params: -n
|   7462:
|     Name: snmpd
|     Path: /usr/sbin/snmpd
|     Params: -Lsd -Lf /dev/null -u Debian-snmp -g Debian-snmp -I -smux mteTrigger mteTriggerConf -f
|   8220:
|     Name: mysqld
|     Path: /usr/sbin/mysqld
|   8405:
|     Name: polkitd
|     Path: /usr/lib/policykit-1/polkitd
|     Params: --no-debug
|   9558:
|     Name: apache2
|     Path: /usr/sbin/apache2
|     Params: -k start
|   9575:
|     Name: apache2
|     Path: /usr/sbin/apache2
|     Params: -k start
|   9576:
|     Name: apache2
|     Path: /usr/sbin/apache2
|     Params: -k start
|   9577:
|     Name: apache2
|     Path: /usr/sbin/apache2
|     Params: -k start
|   9578:
|     Name: apache2
|     Path: /usr/sbin/apache2
|     Params: -k start
|   9579:
|     Name: apache2
|     Path: /usr/sbin/apache2
|     Params: -k start
|   10519:
|     Name: avahi-daemon
|     Path: avahi-daemon: running [JOY.local]
|   10520:
|     Name: avahi-daemon
|     Path: avahi-daemon: chroot helper
|   10606:
|     Name: smbd
|     Path: /usr/sbin/smbd
|   10607:
|     Name: smbd-notifyd
|     Path: /usr/sbin/smbd
|   10608:
|     Name: cleanupd
|     Path: /usr/sbin/smbd
|   10611:
|     Name: lpqd
|     Path: /usr/sbin/smbd
|   10651:
|     Name: nmbd
|     Path: /usr/sbin/nmbd
|   14688:
|     Name: dovecot
|     Path: /usr/sbin/dovecot
|   14690:
|     Name: anvil
|     Path: dovecot/anvil
|   14691:
|     Name: log
|     Path: dovecot/log
|   14693:
|     Name: config
|     Path: dovecot/config
|   14716:
|     Name: packagekitd
|     Path: /usr/lib/packagekit/packagekitd
|   16248:
|     Name: kworker/0:2
|   16934:
|     Name: kworker/0:0
|   16954:
|     Name: systemd-udevd
|     Path: /lib/systemd/systemd-udevd
|   17005:
|     Name: kworker/0:1
|   17062:
|_    Name: kworker/u2:0
| snmp-sysdescr: Linux JOY 4.9.0-8-amd64 #1 SMP Debian 4.9.130-2 (2018-10-27) x86_64
|_  System uptime: 53m33.25s (321325 timeticks)
| snmp-netstat:
|   TCP  0.0.0.0:21           0.0.0.0:0
|   TCP  0.0.0.0:22           0.0.0.0:0
|   TCP  0.0.0.0:25           0.0.0.0:0
|   TCP  0.0.0.0:110          0.0.0.0:0
|   TCP  0.0.0.0:139          0.0.0.0:0
|   TCP  0.0.0.0:143          0.0.0.0:0
|   TCP  0.0.0.0:445          0.0.0.0:0
|   TCP  0.0.0.0:465          0.0.0.0:0
|   TCP  0.0.0.0:587          0.0.0.0:0
|   TCP  0.0.0.0:993          0.0.0.0:0
|   TCP  0.0.0.0:995          0.0.0.0:0
|   TCP  127.0.0.1:3306       0.0.0.0:0
|   UDP  0.0.0.0:68           *:*
|   UDP  0.0.0.0:123          *:*
|   UDP  0.0.0.0:137          *:*
|   UDP  0.0.0.0:138          *:*
|   UDP  0.0.0.0:161          *:*
|   UDP  0.0.0.0:5353         *:*
|   UDP  0.0.0.0:33199        *:*
|   UDP  0.0.0.0:36969        *:*
|   UDP  0.0.0.0:59086        *:*
|   UDP  10.0.88.34:123       *:*
|   UDP  10.0.88.34:137       *:*
|   UDP  10.0.88.34:138       *:*
|   UDP  10.0.88.255:137      *:*
|   UDP  10.0.88.255:138      *:*
|_  UDP  127.0.0.1:123        *:*
| snmp-info:
|   enterprise: net-snmp
|   engineIDFormat: unknown
|   engineIDData: d1785e76ec962f5c00000000
|   snmpEngineBoots: 30
|_  snmpEngineTime: 53m33s
| snmp-win32-software:
|   accountsservice-0.6.43-1; 0-01-01T00:00:00
|   acl-2.2.52-3+b1; 0-01-01T00:00:00
|   adduser-3.115; 0-01-01T00:00:00
|   adwaita-icon-theme-3.22.0-1+deb9u1; 0-01-01T00:00:00
|   alsa-utils-1.1.3-1; 0-01-01T00:00:00
|   anacron-2.3-24; 0-01-01T00:00:00
|   apache2-2.4.25-3+deb9u13; 0-01-01T00:00:00
|   apache2-bin-2.4.25-3+deb9u13; 0-01-01T00:00:00
|   apache2-data-2.4.25-3+deb9u13; 0-01-01T00:00:00
|   apache2-utils-2.4.25-3+deb9u13; 0-01-01T00:00:00
|   apg-2.2.3.dfsg.1-4+b1; 0-01-01T00:00:00
|   appstream-0.10.6-2; 0-01-01T00:00:00
|   apt-1.4.11; 0-01-01T00:00:00
|   apt-listchanges-3.10; 0-01-01T00:00:00
|   apt-transport-https-1.4.11; 0-01-01T00:00:00
|   apt-utils-1.4.11; 0-01-01T00:00:00
|   argyll-1.9.2+repack-1+b1; 0-01-01T00:00:00
|   argyll-ref-1.9.2+repack-1; 0-01-01T00:00:00
|   aspell-0.60.7~20110707-3+deb9u1; 0-01-01T00:00:00
|   aspell-en-2016.11.20-0-0.1; 0-01-01T00:00:00
|   at-spi2-core-2.22.0-6+deb9u1; 0-01-01T00:00:00
|   attr-1:2.4.47-2+b2; 0-01-01T00:00:00
|   avahi-daemon-0.6.32-2+deb9u1; 0-01-01T00:00:00
|   baobab-3.22.1-1; 0-01-01T00:00:00
|   base-files-9.9+deb9u6; 0-01-01T00:00:00
|   base-passwd-3.5.43; 0-01-01T00:00:00
|   bash-4.4-5; 0-01-01T00:00:00
|   bash-completion-1:2.1-4.3; 0-01-01T00:00:00
|   bc-1.06.95-9+b3; 0-01-01T00:00:00
|   bind9-host-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   binutils-2.28-5; 0-01-01T00:00:00
|   bluez-5.43-2+deb9u5; 0-01-01T00:00:00
|   bluez-obexd-5.43-2+deb9u5; 0-01-01T00:00:00
|   bogofilter-1.2.4+dfsg1-9; 0-01-01T00:00:00
|   bogofilter-bdb-1.2.4+dfsg1-9; 0-01-01T00:00:00
|   bogofilter-common-1.2.4+dfsg1-9; 0-01-01T00:00:00
|   bsdmainutils-9.0.12+nmu1; 0-01-01T00:00:00
|   bsdutils-1:2.29.2-1+deb9u1; 0-01-01T00:00:00
|   build-essential-12.3; 0-01-01T00:00:00
|   busybox-1:1.22.0-19+deb9u2; 0-01-01T00:00:00
|   bzip2-1.0.6-8.1; 0-01-01T00:00:00
|   ca-certificates-20200601~deb9u2; 0-01-01T00:00:00
|   ca-certificates-java-20170531+nmu1; 0-01-01T00:00:00
|   caribou-0.4.21-1+deb9u1; 0-01-01T00:00:00
|   cheese-common-3.22.1-1; 0-01-01T00:00:00
|   chrome-gnome-shell-8-4; 0-01-01T00:00:00
|   coinor-libcgl1-0.58.9-1+b1; 0-01-01T00:00:00
|   coinor-libcoinmp1v5-1.7.6+dfsg1-2; 0-01-01T00:00:00
|   coinor-libosi1v5-0.106.9-2+b1; 0-01-01T00:00:00
|   colord-1.3.3-2; 0-01-01T00:00:00
|   colord-data-1.3.3-2; 0-01-01T00:00:00
|   console-setup-1.164; 0-01-01T00:00:00
|   console-setup-linux-1.164; 0-01-01T00:00:00
|   coreutils-8.26-3; 0-01-01T00:00:00
|   cpio-2.11+dfsg-6; 0-01-01T00:00:00
|   cpp-4:6.3.0-4; 0-01-01T00:00:00
|   cpp-6-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   cracklib-runtime-2.9.2-5; 0-01-01T00:00:00
|   crda-3.18-1; 0-01-01T00:00:00
|   cron-3.0pl1-128+deb9u2; 0-01-01T00:00:00
|   cups-2.2.1-8+deb9u2; 0-01-01T00:00:00
|   cups-browsed-1.11.6-3+deb9u1; 0-01-01T00:00:00
|   cups-bsd-2.2.1-8+deb9u8; 0-01-01T00:00:00
|   cups-common-2.2.1-8+deb9u8; 0-01-01T00:00:00
|   cups-core-drivers-2.2.1-8+deb9u8; 0-01-01T00:00:00
|   cups-daemon-2.2.1-8+deb9u8; 0-01-01T00:00:00
|   cups-filters-1.11.6-3; 0-01-01T00:00:00
|   cups-pk-helper-0.2.6-1+b1; 0-01-01T00:00:00
|   dash-0.5.8-2.4; 0-01-01T00:00:00
|   dbus-1.10.28-0+deb9u1; 0-01-01T00:00:00
|   dbus-user-session-1.10.28-0+deb9u1; 0-01-01T00:00:00
|   dbus-x11-1.10.28-0+deb9u1; 0-01-01T00:00:00
|   dc-1.06.95-9+b3; 0-01-01T00:00:00
|   dconf-cli-0.26.0-2+b1; 0-01-01T00:00:00
|   dconf-gsettings-backend-0.26.0-2+b1; 0-01-01T00:00:00
|   dconf-service-0.26.0-2+b1; 0-01-01T00:00:00
|   debconf-1.5.61; 0-01-01T00:00:00
|   debconf-i18n-1.5.61; 0-01-01T00:00:00
|   debian-archive-keyring-2017.5+deb9u2; 0-01-01T00:00:00
|   debian-faq-8.1; 0-01-01T00:00:00
|   debianutils-4.8.1.1; 0-01-01T00:00:00
|   desktop-base-9.0.2+deb9u1; 0-01-01T00:00:00
|   desktop-file-utils-0.23-1; 0-01-01T00:00:00
|   dh-python-2.20170125; 0-01-01T00:00:00
|   dictionaries-common-1.27.2; 0-01-01T00:00:00
|   diffutils-1:3.5-3; 0-01-01T00:00:00
|   discover-2.1.2-7.1+deb9u1; 0-01-01T00:00:00
|   discover-data-2.2013.01.11; 0-01-01T00:00:00
|   distro-info-data-0.36; 0-01-01T00:00:00
|   dleyna-server-0.4.0-1.1; 0-01-01T00:00:00
|   dmidecode-3.0-4; 0-01-01T00:00:00
|   dmsetup-2:1.02.137-2; 0-01-01T00:00:00
|   dns-root-data-2017072601~deb9u1; 0-01-01T00:00:00
|   dnsmasq-base-2.76-5+deb9u3; 0-01-01T00:00:00
|   doc-debian-6.4; 0-01-01T00:00:00
|   docbook-xml-4.5-8; 0-01-01T00:00:00
|   dosfstools-4.1-1; 0-01-01T00:00:00
|   dovecot-core-1:2.2.27-3+deb9u7; 0-01-01T00:00:00
|   dovecot-imapd-1:2.2.27-3+deb9u7; 0-01-01T00:00:00
|   dovecot-pop3d-1:2.2.27-3+deb9u7; 0-01-01T00:00:00
|   dpkg-1.18.26; 0-01-01T00:00:00
|   dpkg-dev-1.18.26; 0-01-01T00:00:00
|   e2fslibs-1.43.4-2+deb9u2; 0-01-01T00:00:00
|   e2fsprogs-1.43.4-2+deb9u2; 0-01-01T00:00:00
|   eject-2.1.5+deb1+cvs20081104-13.2; 0-01-01T00:00:00
|   emacsen-common-2.0.8; 0-01-01T00:00:00
|   enchant-1.6.0-11+b1; 0-01-01T00:00:00
|   eog-3.20.5-1+b1; 0-01-01T00:00:00
|   espeak-ng-data-1.49.0+dfsg-11; 0-01-01T00:00:00
|   evince-3.22.1-3+deb9u2; 0-01-01T00:00:00
|   evince-common-3.22.1-3+deb9u2; 0-01-01T00:00:00
|   evolution-3.22.6-1+deb9u2; 0-01-01T00:00:00
|   evolution-common-3.22.6-1+deb9u2; 0-01-01T00:00:00
|   evolution-data-server-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   evolution-data-server-common-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   evolution-plugins-3.22.6-1+deb9u2; 0-01-01T00:00:00
|   exfat-fuse-1.2.5-2; 0-01-01T00:00:00
|   exfat-utils-1.2.5-2; 0-01-01T00:00:00
|   expect-5.45-7+deb9u1; 0-01-01T00:00:00
|   fakeroot-1.21-3.1; 0-01-01T00:00:00
|   fig2dev-1:3.2.6a-2+deb9u4; 0-01-01T00:00:00
|   file-1:5.30-1+deb9u3; 0-01-01T00:00:00
|   file-roller-3.22.3-1+deb9u1; 0-01-01T00:00:00
|   findutils-4.6.0+git+20161106-2; 0-01-01T00:00:00
|   firefox-esr-91.11.0esr-1~deb9u1; 0-01-01T00:00:00
|   fontconfig-2.11.0-6.7+b1; 0-01-01T00:00:00
|   fontconfig-config-2.11.0-6.7; 0-01-01T00:00:00
|   fonts-cantarell-0.0.25-2; 0-01-01T00:00:00
|   fonts-crosextra-caladea-20130214-1; 0-01-01T00:00:00
|   fonts-crosextra-carlito-20130920-1; 0-01-01T00:00:00
|   fonts-dejavu-2.37-1; 0-01-01T00:00:00
|   fonts-dejavu-core-2.37-1; 0-01-01T00:00:00
|   fonts-dejavu-extra-2.37-1; 0-01-01T00:00:00
|   fonts-droid-fallback-1:6.0.1r16-1.1; 0-01-01T00:00:00
|   fonts-liberation-1:1.07.4-2; 0-01-01T00:00:00
|   fonts-linuxlibertine-5.3.0-2; 0-01-01T00:00:00
|   fonts-noto-mono-20161116-1; 0-01-01T00:00:00
|   fonts-sil-gentium-20081126:1.03-1; 0-01-01T00:00:00
|   fonts-sil-gentium-basic-1.1-7; 0-01-01T00:00:00
|   foomatic-filters-4.0.17-9; 0-01-01T00:00:00
|   freepats-20060219-1; 0-01-01T00:00:00
|   fuse-2.9.7-1+deb9u2; 0-01-01T00:00:00
|   g++-4:6.3.0-4; 0-01-01T00:00:00
|   g++-6-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   galera-3-25.3.19-2; 0-01-01T00:00:00
|   gawk-1:4.1.4+dfsg-1; 0-01-01T00:00:00
|   gcc-4:6.3.0-4; 0-01-01T00:00:00
|   gcc-6-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   gcc-6-base-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   gcr-3.20.0-5.1; 0-01-01T00:00:00
|   gdisk-1.0.1-1+deb9u1; 0-01-01T00:00:00
|   gdm3-3.22.3-3+deb9u3; 0-01-01T00:00:00
|   gedit-3.22.0-2; 0-01-01T00:00:00
|   gedit-common-3.22.0-2; 0-01-01T00:00:00
|   gedit-plugins-3.22.0-1; 0-01-01T00:00:00
|   geoclue-2.0-2.4.5-1; 0-01-01T00:00:00
|   geoip-database-20170512-1; 0-01-01T00:00:00
|   gettext-base-0.19.8.1-2; 0-01-01T00:00:00
|   ghostscript-9.26a~dfsg-0+deb9u9; 0-01-01T00:00:00
|   gimp-2.8.18-1+deb9u1; 0-01-01T00:00:00
|   gimp-data-2.8.18-1+deb9u1; 0-01-01T00:00:00
|   gir1.2-accountsservice-1.0-0.6.43-1; 0-01-01T00:00:00
|   gir1.2-atk-1.0-2.22.0-1; 0-01-01T00:00:00
|   gir1.2-atspi-2.0-2.22.0-6+deb9u1; 0-01-01T00:00:00
|   gir1.2-caribou-1.0-0.4.21-1+deb9u1; 0-01-01T00:00:00
|   gir1.2-clutter-1.0-1.26.0+dfsg-3; 0-01-01T00:00:00
|   gir1.2-clutter-gst-3.0-3.0.24-1; 0-01-01T00:00:00
|   gir1.2-cogl-1.0-1.22.2-2; 0-01-01T00:00:00
|   gir1.2-coglpango-1.0-1.22.2-2; 0-01-01T00:00:00
|   gir1.2-evince-3.0-3.22.1-3+deb9u2; 0-01-01T00:00:00
|   gir1.2-freedesktop-1.50.0-1+b1; 0-01-01T00:00:00
|   gir1.2-gck-1-3.20.0-5.1; 0-01-01T00:00:00
|   gir1.2-gcr-3-3.20.0-5.1; 0-01-01T00:00:00
|   gir1.2-gdesktopenums-3.0-3.22.0-1; 0-01-01T00:00:00
|   gir1.2-gdkpixbuf-2.0-2.36.5-2+deb9u2; 0-01-01T00:00:00
|   gir1.2-gdm-1.0-3.22.3-3+deb9u3; 0-01-01T00:00:00
|   gir1.2-git2-glib-1.0-0.24.4-1; 0-01-01T00:00:00
|   gir1.2-glib-2.0-1.50.0-1+b1; 0-01-01T00:00:00
|   gir1.2-gmenu-3.0-3.13.3-9; 0-01-01T00:00:00
|   gir1.2-gnomebluetooth-1.0-3.20.1-1; 0-01-01T00:00:00
|   gir1.2-gnomedesktop-3.0-3.22.2-1; 0-01-01T00:00:00
|   gir1.2-gnomekeyring-1.0-3.12.0-1+b2; 0-01-01T00:00:00
|   gir1.2-gst-plugins-base-1.0-1.10.4-1+deb9u2; 0-01-01T00:00:00
|   gir1.2-gstreamer-1.0-1.10.4-1; 0-01-01T00:00:00
|   gir1.2-gtk-3.0-3.22.11-1; 0-01-01T00:00:00
|   gir1.2-gtkclutter-1.0-1.8.2-2; 0-01-01T00:00:00
|   gir1.2-gtksource-3.0-3.22.2-1; 0-01-01T00:00:00
|   gir1.2-gucharmap-2.90-1:9.0.2-1; 0-01-01T00:00:00
|   gir1.2-gweather-3.0-3.20.4-1; 0-01-01T00:00:00
|   gir1.2-ibus-1.0-1.5.14-3+deb9u2; 0-01-01T00:00:00
|   gir1.2-javascriptcoregtk-4.0-2.18.6-1~deb9u1; 0-01-01T00:00:00
|   gir1.2-json-1.0-1.2.6-1; 0-01-01T00:00:00
|   gir1.2-mutter-3.0-3.22.3-2; 0-01-01T00:00:00
|   gir1.2-networkmanager-1.0-1.6.2-3+deb9u2; 0-01-01T00:00:00
|   gir1.2-nmgtk-1.0-1.4.4-1+deb9u1; 0-01-01T00:00:00
|   gir1.2-notify-0.7-0.7.7-2; 0-01-01T00:00:00
|   gir1.2-packagekitglib-1.0-1.1.5-2+deb9u2; 0-01-01T00:00:00
|   gir1.2-pango-1.0-1.40.5-1; 0-01-01T00:00:00
|   gir1.2-peas-1.0-1.20.0-1+b1; 0-01-01T00:00:00
|   gir1.2-polkit-1.0-0.105-18+deb9u2; 0-01-01T00:00:00
|   gir1.2-secret-1-0.18.5-3.1; 0-01-01T00:00:00
|   gir1.2-soup-2.4-2.56.0-2+deb9u2; 0-01-01T00:00:00
|   gir1.2-telepathyglib-0.12-0.24.1-1.1; 0-01-01T00:00:00
|   gir1.2-telepathylogger-0.2-0.8.2-2; 0-01-01T00:00:00
|   gir1.2-totem-1.0-3.22.1-1; 0-01-01T00:00:00
|   gir1.2-totem-plparser-1.0-3.10.7-1+b1; 0-01-01T00:00:00
|   gir1.2-upowerglib-1.0-0.99.4-4+b1; 0-01-01T00:00:00
|   gir1.2-vte-2.91-0.46.1-1; 0-01-01T00:00:00
|   gir1.2-webkit2-4.0-2.18.6-1~deb9u1; 0-01-01T00:00:00
|   gir1.2-wnck-3.0-3.20.1-3; 0-01-01T00:00:00
|   gir1.2-zeitgeist-2.0-0.9.16-0.2+b1; 0-01-01T00:00:00
|   gjs-1.46.0-1+b2; 0-01-01T00:00:00
|   gkbd-capplet-3.22.0.1-1+b1; 0-01-01T00:00:00
|   glib-networking-2.50.0-1+b1; 0-01-01T00:00:00
|   glib-networking-common-2.50.0-1; 0-01-01T00:00:00
|   glib-networking-services-2.50.0-1+b1; 0-01-01T00:00:00
|   gnome-accessibility-themes-3.22.2-2; 0-01-01T00:00:00
|   gnome-backgrounds-3.22.1-1; 0-01-01T00:00:00
|   gnome-bluetooth-3.20.1-1; 0-01-01T00:00:00
|   gnome-calculator-3.22.3-1; 0-01-01T00:00:00
|   gnome-characters-3.22.0-1; 0-01-01T00:00:00
|   gnome-chess-1:3.22.2-1+b1; 0-01-01T00:00:00
|   gnome-clocks-3.22.1-1; 0-01-01T00:00:00
|   gnome-color-manager-3.22.2-1; 0-01-01T00:00:00
|   gnome-control-center-1:3.22.2-3; 0-01-01T00:00:00
|   gnome-control-center-data-1:3.22.2-3; 0-01-01T00:00:00
|   gnome-desktop3-data-3.22.2-1; 0-01-01T00:00:00
|   gnome-disk-utility-3.22.1-1; 0-01-01T00:00:00
|   gnome-font-viewer-3.22.0-1+b1; 0-01-01T00:00:00
|   gnome-getting-started-docs-3.22.0-1; 0-01-01T00:00:00
|   gnome-keyring-3.20.0-3; 0-01-01T00:00:00
|   gnome-logs-3.22.1-2; 0-01-01T00:00:00
|   gnome-menus-3.13.3-9; 0-01-01T00:00:00
|   gnome-online-accounts-3.22.5-1; 0-01-01T00:00:00
|   gnome-online-miners-3.22.0-1; 0-01-01T00:00:00
|   gnome-orca-3.22.2-3; 0-01-01T00:00:00
|   gnome-screenshot-3.22.0-1+b1; 0-01-01T00:00:00
|   gnome-session-3.22.3-1; 0-01-01T00:00:00
|   gnome-session-bin-3.22.3-1; 0-01-01T00:00:00
|   gnome-session-common-3.22.3-1; 0-01-01T00:00:00
|   gnome-settings-daemon-3.22.2-2+deb9u2; 0-01-01T00:00:00
|   gnome-shell-3.22.3-3+deb9u1; 0-01-01T00:00:00
|   gnome-shell-common-3.22.3-3+deb9u1; 0-01-01T00:00:00
|   gnome-shell-extensions-3.22.2-1; 0-01-01T00:00:00
|   gnome-software-3.22.5-1; 0-01-01T00:00:00
|   gnome-software-common-3.22.5-1; 0-01-01T00:00:00
|   gnome-sushi-3.21.91-2; 0-01-01T00:00:00
|   gnome-system-monitor-3.22.2-1; 0-01-01T00:00:00
|   gnome-terminal-3.22.2-1; 0-01-01T00:00:00
|   gnome-terminal-data-3.22.2-1; 0-01-01T00:00:00
|   gnome-themes-standard-3.22.2-2; 0-01-01T00:00:00
|   gnome-themes-standard-data-3.22.2-2; 0-01-01T00:00:00
|   gnome-tweak-tool-3.22.0-1; 0-01-01T00:00:00
|   gnome-user-guide-3.22.0-1; 0-01-01T00:00:00
|   gnome-user-share-3.18.3-1+b1; 0-01-01T00:00:00
|   gnupg-2.1.18-8~deb9u3; 0-01-01T00:00:00
|   gnupg-agent-2.1.18-8~deb9u3; 0-01-01T00:00:00
|   gpgv-2.1.18-8~deb9u3; 0-01-01T00:00:00
|   grep-2.27-2; 0-01-01T00:00:00
|   grilo-plugins-0.3-0.3.3-1+deb9u1; 0-01-01T00:00:00
|   groff-base-1.22.3-9; 0-01-01T00:00:00
|   grub-common-2.02~beta3-5+deb9u1; 0-01-01T00:00:00
|   grub-pc-2.02~beta3-5+deb9u1; 0-01-01T00:00:00
|   grub-pc-bin-2.02~beta3-5+deb9u1; 0-01-01T00:00:00
|   grub2-common-2.02~beta3-5+deb9u1; 0-01-01T00:00:00
|   gsettings-desktop-schemas-3.22.0-1; 0-01-01T00:00:00
|   gsfonts-1:8.11+urwcyr1.0.7~pre44-4.3; 0-01-01T00:00:00
|   gstreamer1.0-clutter-3.0-3.0.24-1; 0-01-01T00:00:00
|   gstreamer1.0-libav-1.10.4-1+deb9u1; 0-01-01T00:00:00
|   gstreamer1.0-plugins-bad-1.10.4-1+deb9u2; 0-01-01T00:00:00
|   gstreamer1.0-plugins-base-1.10.4-1+deb9u2; 0-01-01T00:00:00
|   gstreamer1.0-plugins-good-1.10.4-1+deb9u1; 0-01-01T00:00:00
|   gstreamer1.0-plugins-ugly-1.10.4-1+deb9u1; 0-01-01T00:00:00
|   gstreamer1.0-pulseaudio-1.10.4-1+deb9u1; 0-01-01T00:00:00
|   gstreamer1.0-x-1.10.4-1+deb9u2; 0-01-01T00:00:00
|   gtk-update-icon-cache-3.22.11-1; 0-01-01T00:00:00
|   gtk2-engines-pixbuf-2.24.31-2; 0-01-01T00:00:00
|   gvfs-1.30.4-1; 0-01-01T00:00:00
|   gvfs-backends-1.30.4-1; 0-01-01T00:00:00
|   gvfs-bin-1.30.4-1; 0-01-01T00:00:00
|   gvfs-common-1.30.4-1; 0-01-01T00:00:00
|   gvfs-daemons-1.30.4-1; 0-01-01T00:00:00
|   gvfs-fuse-1.30.4-1; 0-01-01T00:00:00
|   gvfs-libs-1.30.4-1; 0-01-01T00:00:00
|   gzip-1.6-5+deb9u1; 0-01-01T00:00:00
|   hdparm-9.51+ds-1+deb9u1; 0-01-01T00:00:00
|   hicolor-icon-theme-0.15-1; 0-01-01T00:00:00
|   hitori-3.22.0-1; 0-01-01T00:00:00
|   hoichess-0.19.0-5+b1; 0-01-01T00:00:00
|   hostname-3.18+b1; 0-01-01T00:00:00
|   hplip-3.16.11+repack0-3; 0-01-01T00:00:00
|   hunspell-en-us-20070829-7; 0-01-01T00:00:00
|   i965-va-driver-1.7.3-1; 0-01-01T00:00:00
|   iamerican-3.4.00-5; 0-01-01T00:00:00
|   ibritish-3.4.00-5; 0-01-01T00:00:00
|   icedtea-netx-common-1.6.2-3.1; 0-01-01T00:00:00
|   ienglish-common-3.4.00-5; 0-01-01T00:00:00
|   ifupdown-0.8.19; 0-01-01T00:00:00
|   iio-sensor-proxy-2.0-4; 0-01-01T00:00:00
|   imagemagick-6-common-8:6.9.7.4+dfsg-11+deb9u14; 0-01-01T00:00:00
|   imagemagick-8:6.9.7.4+dfsg-11+deb9u6; 0-01-01T00:00:00
|   init-1.48; 0-01-01T00:00:00
|   init-system-helpers-1.48; 0-01-01T00:00:00
|   initramfs-tools-0.130; 0-01-01T00:00:00
|   initramfs-tools-core-0.130; 0-01-01T00:00:00
|   inkscape-0.92.1-1; 0-01-01T00:00:00
|   inotify-tools-3.14-2; 0-01-01T00:00:00
|   installation-report-2.62; 0-01-01T00:00:00
|   iproute2-4.9.0-1+deb9u1; 0-01-01T00:00:00
|   iptables-1.6.0+snapshot20161117-6; 0-01-01T00:00:00
|   iptables-persistent-1.0.4+nmu2; 0-01-01T00:00:00
|   iputils-arping-3:20161105-1; 0-01-01T00:00:00
|   iputils-ping-3:20161105-1; 0-01-01T00:00:00
|   isc-dhcp-client-4.3.5-3+deb9u2; 0-01-01T00:00:00
|   isc-dhcp-common-4.3.5-3+deb9u2; 0-01-01T00:00:00
|   iso-codes-3.75-1; 0-01-01T00:00:00
|   ispell-3.4.00-5; 0-01-01T00:00:00
|   iw-4.9-0.1; 0-01-01T00:00:00
|   java-common-0.58; 0-01-01T00:00:00
|   kbd-2.0.3-2+b1; 0-01-01T00:00:00
|   keyboard-configuration-1.164; 0-01-01T00:00:00
|   klibc-utils-2.0.4-9+deb9u1; 0-01-01T00:00:00
|   kmod-23-2; 0-01-01T00:00:00
|   krb5-locales-1.15-1+deb9u3; 0-01-01T00:00:00
|   laptop-detect-0.13.8; 0-01-01T00:00:00
|   less-481-2.1; 0-01-01T00:00:00
|   liba52-0.7.4-0.7.4-19; 0-01-01T00:00:00
|   libaa1-1.4p5-44+b1; 0-01-01T00:00:00
|   libaacs0-0.8.1-2; 0-01-01T00:00:00
|   libaccountsservice0-0.6.43-1; 0-01-01T00:00:00
|   libacl1-2.2.52-3+b1; 0-01-01T00:00:00
|   libaio1-0.3.110-3; 0-01-01T00:00:00
|   libalgorithm-diff-perl-1.19.03-1; 0-01-01T00:00:00
|   libalgorithm-diff-xs-perl-0.04-4+b2; 0-01-01T00:00:00
|   libalgorithm-merge-perl-0.08-3; 0-01-01T00:00:00
|   libao-common-1.2.2-1; 0-01-01T00:00:00
|   libao4-1.2.2-1; 0-01-01T00:00:00
|   libapache2-mod-dnssd-0.6-3.1; 0-01-01T00:00:00
|   libapache2-mod-php7.0-7.0.33-0+deb9u12; 0-01-01T00:00:00
|   libapparmor1-2.11.0-3+deb9u2; 0-01-01T00:00:00
|   libappstream-glib8-0.6.8-1; 0-01-01T00:00:00
|   libappstream4-0.10.6-2; 0-01-01T00:00:00
|   libapr1-1.5.2-5+deb9u1; 0-01-01T00:00:00
|   libaprutil1-1.5.4-3; 0-01-01T00:00:00
|   libaprutil1-dbd-sqlite3-1.5.4-3; 0-01-01T00:00:00
|   libaprutil1-ldap-1.5.4-3; 0-01-01T00:00:00
|   libapt-inst2.0-1.4.11; 0-01-01T00:00:00
|   libapt-pkg5.0-1.4.11; 0-01-01T00:00:00
|   libarchive13-3.2.2-2+deb9u3; 0-01-01T00:00:00
|   libart-2.0-2-2.3.21-2; 0-01-01T00:00:00
|   libasan3-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libasound2-1.1.3-5; 0-01-01T00:00:00
|   libasound2-data-1.1.3-5; 0-01-01T00:00:00
|   libasound2-plugins-1.1.1-1; 0-01-01T00:00:00
|   libaspell15-0.60.7~20110707-3+deb9u1; 0-01-01T00:00:00
|   libass5-1:0.13.4-2; 0-01-01T00:00:00
|   libassuan0-2.4.3-2; 0-01-01T00:00:00
|   libasyncns0-0.8-6; 0-01-01T00:00:00
|   libatasmart4-0.19-4+b1; 0-01-01T00:00:00
|   libatk-adaptor-2.22.0-2; 0-01-01T00:00:00
|   libatk-bridge2.0-0-2.22.0-2; 0-01-01T00:00:00
|   libatk1.0-0-2.22.0-1; 0-01-01T00:00:00
|   libatk1.0-data-2.22.0-1; 0-01-01T00:00:00
|   libatkmm-1.6-1v5-2.24.2-2; 0-01-01T00:00:00
|   libatomic1-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libatspi2.0-0-2.22.0-6+deb9u1; 0-01-01T00:00:00
|   libattr1-1:2.4.47-2+b2; 0-01-01T00:00:00
|   libaudio2-1.9.4-5+b1; 0-01-01T00:00:00
|   libaudit-common-1:2.6.7-2; 0-01-01T00:00:00
|   libaudit1-1:2.6.7-2; 0-01-01T00:00:00
|   libauthen-sasl-perl-2.1600-1; 0-01-01T00:00:00
|   libavahi-client3-0.6.32-2+deb9u1; 0-01-01T00:00:00
|   libavahi-common-data-0.6.32-2+deb9u1; 0-01-01T00:00:00
|   libavahi-common3-0.6.32-2+deb9u1; 0-01-01T00:00:00
|   libavahi-core7-0.6.32-2+deb9u1; 0-01-01T00:00:00
|   libavahi-glib1-0.6.32-2+deb9u1; 0-01-01T00:00:00
|   libavahi-gobject0-0.6.32-2+deb9u1; 0-01-01T00:00:00
|   libavahi-ui-gtk3-0-0.6.32-2+deb9u1; 0-01-01T00:00:00
|   libavc1394-0-0.5.4-4+b1; 0-01-01T00:00:00
|   libavcodec57-7:3.2.18-0+deb9u1; 0-01-01T00:00:00
|   libavfilter6-7:3.2.18-0+deb9u1; 0-01-01T00:00:00
|   libavformat57-7:3.2.18-0+deb9u1; 0-01-01T00:00:00
|   libavresample3-7:3.2.18-0+deb9u1; 0-01-01T00:00:00
|   libavutil55-7:3.2.18-0+deb9u1; 0-01-01T00:00:00
|   libbcprov-java-1.56-1+deb9u3; 0-01-01T00:00:00
|   libbdplus0-0.1.2-2; 0-01-01T00:00:00
|   libbind9-140-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   libblas-common-3.7.0-2; 0-01-01T00:00:00
|   libblas3-3.7.0-2; 0-01-01T00:00:00
|   libblkid1-2.29.2-1+deb9u1; 0-01-01T00:00:00
|   libbluetooth3-5.43-2+deb9u5; 0-01-01T00:00:00
|   libbluray1-1:0.9.3-3; 0-01-01T00:00:00
|   libboost-date-time1.62.0-1.62.0+dfsg-4; 0-01-01T00:00:00
|   libboost-filesystem1.62.0-1.62.0+dfsg-4; 0-01-01T00:00:00
|   libboost-system1.62.0-1.62.0+dfsg-4; 0-01-01T00:00:00
|   libboost-thread1.62.0-1.62.0+dfsg-4; 0-01-01T00:00:00
|   libbrlapi0.6-5.4-7+deb9u1; 0-01-01T00:00:00
|   libbs2b0-3.1.0+dfsg-2.2; 0-01-01T00:00:00
|   libbsd0-0.8.3-1+deb9u1; 0-01-01T00:00:00
|   libbz2-1.0-1.0.6-8.1; 0-01-01T00:00:00
|   libc-bin-2.24-11+deb9u3; 0-01-01T00:00:00
|   libc-dev-bin-2.24-11+deb9u3; 0-01-01T00:00:00
|   libc-l10n-2.24-11+deb9u3; 0-01-01T00:00:00
|   libc6-2.24-11+deb9u3; 0-01-01T00:00:00
|   libc6-dev-2.24-11+deb9u3; 0-01-01T00:00:00
|   libcaca0-0.99.beta19-2.1~deb9u2; 0-01-01T00:00:00
|   libcacard0-1:2.5.0-3; 0-01-01T00:00:00
|   libcairo-gobject2-1.14.8-1+deb9u1; 0-01-01T00:00:00
|   libcairo-perl-1.106-1+b2; 0-01-01T00:00:00
|   libcairo2-1.14.8-1+deb9u1; 0-01-01T00:00:00
|   libcairomm-1.0-1v5-1.12.0-1+b1; 0-01-01T00:00:00
|   libcamel-1.2-59-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libcanberra-gtk3-0-0.30-3; 0-01-01T00:00:00
|   libcanberra-gtk3-module-0.30-3; 0-01-01T00:00:00
|   libcanberra-pulse-0.30-3; 0-01-01T00:00:00
|   libcanberra0-0.30-3; 0-01-01T00:00:00
|   libcap-ng0-0.7.7-3+b1; 0-01-01T00:00:00
|   libcap2-1:2.25-1; 0-01-01T00:00:00
|   libcap2-bin-1:2.25-1; 0-01-01T00:00:00
|   libcaribou-common-0.4.21-1+deb9u1; 0-01-01T00:00:00
|   libcaribou-gtk-module-0.4.21-1+deb9u1; 0-01-01T00:00:00
|   libcaribou-gtk3-module-0.4.21-1+deb9u1; 0-01-01T00:00:00
|   libcaribou0-0.4.21-1+deb9u1; 0-01-01T00:00:00
|   libcc1-0-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libcdio-cdda1-0.83-4.3+b1; 0-01-01T00:00:00
|   libcdio-paranoia1-0.83-4.3+b1; 0-01-01T00:00:00
|   libcdio13-0.83-4.3+b1; 0-01-01T00:00:00
|   libcdparanoia0-3.10.2+debian-11; 0-01-01T00:00:00
|   libcdr-0.1-1-0.1.3-3+b1; 0-01-01T00:00:00
|   libcgi-fast-perl-1:2.12-1; 0-01-01T00:00:00
|   libcgi-pm-perl-4.35-1; 0-01-01T00:00:00
|   libchamplain-0.12-0-0.12.15-1; 0-01-01T00:00:00
|   libchamplain-gtk-0.12-0-0.12.15-1; 0-01-01T00:00:00
|   libcheese-gtk25-3.22.1-1+b1; 0-01-01T00:00:00
|   libcheese8-3.22.1-1+b1; 0-01-01T00:00:00
|   libchromaprint1-1.4.2-1; 0-01-01T00:00:00
|   libcilkrts5-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libclass-isa-perl-0.36-5; 0-01-01T00:00:00
|   libclutter-1.0-0-1.26.0+dfsg-3; 0-01-01T00:00:00
|   libclutter-1.0-common-1.26.0+dfsg-3; 0-01-01T00:00:00
|   libclutter-gst-3.0-0-3.0.24-1; 0-01-01T00:00:00
|   libclutter-gtk-1.0-0-1.8.2-2; 0-01-01T00:00:00
|   libcogl-common-1.22.2-2; 0-01-01T00:00:00
|   libcogl-pango20-1.22.2-2; 0-01-01T00:00:00
|   libcogl-path20-1.22.2-2; 0-01-01T00:00:00
|   libcogl20-1.22.2-2; 0-01-01T00:00:00
|   libcolord-gtk1-0.1.26-1.1; 0-01-01T00:00:00
|   libcolord2-1.3.3-2; 0-01-01T00:00:00
|   libcolorhug2-1.3.3-2; 0-01-01T00:00:00
|   libcomerr2-1.43.4-2+deb9u2; 0-01-01T00:00:00
|   libconfig-inifiles-perl-2.94-1; 0-01-01T00:00:00
|   libcrack2-2.9.2-5; 0-01-01T00:00:00
|   libcroco3-0.6.11-3; 0-01-01T00:00:00
|   libcryptsetup4-2:1.7.3-4; 0-01-01T00:00:00
|   libcryptui0a-3.12.2-3; 0-01-01T00:00:00
|   libcrystalhd3-1:0.0~git20110715.fdd2f19-12; 0-01-01T00:00:00
|   libcue1-1.4.0-1; 0-01-01T00:00:00
|   libcups2-2.2.1-8+deb9u8; 0-01-01T00:00:00
|   libcupsfilters1-1.11.6-3+deb9u1; 0-01-01T00:00:00
|   libcupsimage2-2.2.1-8+deb9u8; 0-01-01T00:00:00
|   libcurl3-gnutls-7.52.1-5+deb9u16; 0-01-01T00:00:00
|   libdaemon0-0.14-6; 0-01-01T00:00:00
|   libdatrie1-0.2.10-4+b1; 0-01-01T00:00:00
|   libdb5.3-5.3.28-12+deb9u1; 0-01-01T00:00:00
|   libdbd-mysql-perl-4.041-2; 0-01-01T00:00:00
|   libdbi-perl-1.636-1+b1; 0-01-01T00:00:00
|   libdbus-1-3-1.10.28-0+deb9u1; 0-01-01T00:00:00
|   libdbus-glib-1-2-0.108-2; 0-01-01T00:00:00
|   libdc1394-22-2.2.5-1; 0-01-01T00:00:00
|   libdca0-0.0.5-10; 0-01-01T00:00:00
|   libdconf1-0.26.0-2+b1; 0-01-01T00:00:00
|   libde265-0-1.0.2-2+b2; 0-01-01T00:00:00
|   libdebconfclient0-0.227; 0-01-01T00:00:00
|   libdee-1.0-4-1.0.10-3.1+b2; 0-01-01T00:00:00
|   libdevmapper1.02.1-2:1.02.137-2; 0-01-01T00:00:00
|   libdiscover2-2.1.2-7.1+deb9u1; 0-01-01T00:00:00
|   libdjvulibre-text-3.5.27.1-7+deb9u2; 0-01-01T00:00:00
|   libdjvulibre21-3.5.27.1-7+deb9u2; 0-01-01T00:00:00
|   libdleyna-connector-dbus-1.0-1-0.2.0-1; 0-01-01T00:00:00
|   libdleyna-core-1.0-3-0.4.0-1; 0-01-01T00:00:00
|   libdmapsharing-3.0-2-2.9.37-1; 0-01-01T00:00:00
|   libdns-export162-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   libdns162-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   libdotconf0-1.3-0.2; 0-01-01T00:00:00
|   libdpkg-perl-1.18.26; 0-01-01T00:00:00
|   libdrm-amdgpu1-2.4.74-1; 0-01-01T00:00:00
|   libdrm-intel1-2.4.74-1; 0-01-01T00:00:00
|   libdrm-nouveau2-2.4.74-1; 0-01-01T00:00:00
|   libdrm-radeon1-2.4.74-1; 0-01-01T00:00:00
|   libdrm2-2.4.74-1; 0-01-01T00:00:00
|   libdv4-1.0.0-11; 0-01-01T00:00:00
|   libdvdnav4-5.0.3-3; 0-01-01T00:00:00
|   libdvdread4-5.0.3-2; 0-01-01T00:00:00
|   libebackend-1.2-10-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libebook-1.2-16-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libebook-contacts-1.2-2-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libebur128-1-1.2.2-2; 0-01-01T00:00:00
|   libecal-1.2-19-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libedata-book-1.2-25-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libedata-cal-1.2-28-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libedataserver-1.2-22-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libedataserverui-1.2-1-3.22.7-1+deb9u2; 0-01-01T00:00:00
|   libedit2-3.1-20160903-3; 0-01-01T00:00:00
|   libegl1-mesa-13.0.6-1+b2; 0-01-01T00:00:00
|   libelf1-0.168-1+deb9u1; 0-01-01T00:00:00
|   libenca0-1.19-1+b1; 0-01-01T00:00:00
|   libenchant1c2a-1.6.0-11+b1; 0-01-01T00:00:00
|   libencode-locale-perl-1.05-1; 0-01-01T00:00:00
|   libepoxy0-1.3.1-2; 0-01-01T00:00:00
|   libept1.5.0-1.1+nmu3+b1; 0-01-01T00:00:00
|   libespeak-ng1-1.49.0+dfsg-11; 0-01-01T00:00:00
|   libestr0-0.1.10-2; 0-01-01T00:00:00
|   libevdev2-1.5.6+dfsg-1; 0-01-01T00:00:00
|   libevdocument3-4-3.22.1-3+deb9u2; 0-01-01T00:00:00
|   libevolution-3.22.6-1+deb9u2; 0-01-01T00:00:00
|   libevview3-3-3.22.1-3+deb9u2; 0-01-01T00:00:00
|   libexempi3-2.4.1-1; 0-01-01T00:00:00
|   libexif12-0.6.21-2+deb9u5; 0-01-01T00:00:00
|   libexiv2-14-0.25-3.1+deb9u3; 0-01-01T00:00:00
|   libexpat1-2.2.0-2+deb9u5; 0-01-01T00:00:00
|   libexttextcat-2.0-0-3.4.4-2+b1; 0-01-01T00:00:00
|   libexttextcat-data-3.4.4-2; 0-01-01T00:00:00
|   libfaad2-2.8.0~cvs20161113-1+deb9u3; 0-01-01T00:00:00
|   libfakeroot-1.21-3.1; 0-01-01T00:00:00
|   libfastjson4-0.99.4-1; 0-01-01T00:00:00
|   libfcgi-perl-0.78-2; 0-01-01T00:00:00
|   libfdisk1-2.29.2-1+deb9u1; 0-01-01T00:00:00
|   libffi6-3.2.1-6; 0-01-01T00:00:00
|   libfftw3-double3-3.3.5-3; 0-01-01T00:00:00
|   libfftw3-single3-3.3.5-3; 0-01-01T00:00:00
|   libfile-basedir-perl-0.07-1; 0-01-01T00:00:00
|   libfile-copy-recursive-perl-0.38-1; 0-01-01T00:00:00
|   libfile-desktopentry-perl-0.22-1; 0-01-01T00:00:00
|   libfile-fcntllock-perl-0.22-3+b2; 0-01-01T00:00:00
|   libfile-listing-perl-6.04-1; 0-01-01T00:00:00
|   libfile-mimeinfo-perl-0.27-1; 0-01-01T00:00:00
|   libflac8-1.3.2-2+deb9u2; 0-01-01T00:00:00
|   libflite1-2.0.0-release-3+b1; 0-01-01T00:00:00
|   libfluidsynth1-1.1.6-4+deb9u1; 0-01-01T00:00:00
|   libfont-afm-perl-1.20-2; 0-01-01T00:00:00
|   libfontconfig1-2.11.0-6.7+b1; 0-01-01T00:00:00
|   libfontenc1-1:1.1.3-1+b2; 0-01-01T00:00:00
|   libfreerdp-cache1.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libfreerdp-codec1.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libfreerdp-common1.1.0-1.1.0~git20140921.1.440916e+dfsg1-13+deb; 0-01-01T00:00:00
|   libfreerdp-core1.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libfreerdp-crypto1.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u; 0-01-01T00:00:00
|   libfreerdp-gdi1.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libfreerdp-locale1.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u; 0-01-01T00:00:00
|   libfreerdp-primitives1.1-1.1.0~git20140921.1.440916e+dfsg1-13+d; 0-01-01T00:00:00
|   libfreerdp-utils1.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libfreetype6-2.6.3-3.2+deb9u2; 0-01-01T00:00:00
|   libfribidi0-0.19.7-1+deb9u2; 0-01-01T00:00:00
|   libfuse2-2.9.7-1+deb9u2; 0-01-01T00:00:00
|   libfwupd1-0.7.4-2+deb9u1; 0-01-01T00:00:00
|   libgail-3-0-3.22.11-1; 0-01-01T00:00:00
|   libgail-common-2.24.31-2; 0-01-01T00:00:00
|   libgail18-2.24.31-2; 0-01-01T00:00:00
|   libgbm1-13.0.6-1+b2; 0-01-01T00:00:00
|   libgc1c2-1:7.4.2-8+deb9u1; 0-01-01T00:00:00
|   libgcab-1.0-0-0.7-2+deb9u1; 0-01-01T00:00:00
|   libgcc-6-dev-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libgcc1-1:6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libgcj-common-1:6.3-4; 0-01-01T00:00:00
|   libgcj17-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libgck-1-0-3.20.0-5.1; 0-01-01T00:00:00
|   libgcr-3-common-3.20.0-5.1; 0-01-01T00:00:00
|   libgcr-base-3-1-3.20.0-5.1; 0-01-01T00:00:00
|   libgcr-ui-3-1-3.20.0-5.1; 0-01-01T00:00:00
|   libgcrypt20-1.7.6-2+deb9u4; 0-01-01T00:00:00
|   libgd3-2.2.4-2+deb9u4; 0-01-01T00:00:00
|   libgdata-common-0.17.6-2; 0-01-01T00:00:00
|   libgdata22-0.17.6-2; 0-01-01T00:00:00
|   libgdbm3-1.8.3-14; 0-01-01T00:00:00
|   libgdk-pixbuf2.0-0-2.36.5-2+deb9u2; 0-01-01T00:00:00
|   libgdk-pixbuf2.0-common-2.36.5-2+deb9u2; 0-01-01T00:00:00
|   libgdm1-3.22.3-3+deb9u3; 0-01-01T00:00:00
|   libgee-0.8-2-0.18.1-1; 0-01-01T00:00:00
|   libgeoclue-2-0-2.4.5-1; 0-01-01T00:00:00
|   libgeocode-glib0-3.20.1-2; 0-01-01T00:00:00
|   libgeoip1-1.6.9-4; 0-01-01T00:00:00
|   libgfbgraph-0.2-0-0.2.3-1+b2; 0-01-01T00:00:00
|   libgfortran3-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libgif7-5.1.4-0.4; 0-01-01T00:00:00
|   libgirepository-1.0-1-1.50.0-1+b1; 0-01-01T00:00:00
|   libgit2-24-0.25.1+really0.24.6-1+deb9u1; 0-01-01T00:00:00
|   libgit2-glib-1.0-0-0.24.4-1; 0-01-01T00:00:00
|   libgjs0e-1.46.0-1+b2; 0-01-01T00:00:00
|   libgl1-mesa-dri-13.0.6-1+b2; 0-01-01T00:00:00
|   libgl1-mesa-glx-13.0.6-1+b2; 0-01-01T00:00:00
|   libglapi-mesa-13.0.6-1+b2; 0-01-01T00:00:00
|   libgles2-mesa-13.0.6-1+b2; 0-01-01T00:00:00
|   libglib-perl-3:1.324-1; 0-01-01T00:00:00
|   libglib2.0-0-2.50.3-2+deb9u3; 0-01-01T00:00:00
|   libglib2.0-bin-2.50.3-2+deb9u3; 0-01-01T00:00:00
|   libglib2.0-data-2.50.3-2+deb9u3; 0-01-01T00:00:00
|   libglibmm-2.4-1v5-2.50.0-1; 0-01-01T00:00:00
|   libglu1-mesa-9.0.0-2.1; 0-01-01T00:00:00
|   libgme0-0.6.0-4; 0-01-01T00:00:00
|   libgmime-2.6-0-2.6.22+dfsg2-1; 0-01-01T00:00:00
|   libgmp10-2:6.1.2+dfsg-1+deb9u1; 0-01-01T00:00:00
|   libgnome-autoar-0-0-0.1.1-4+b1; 0-01-01T00:00:00
|   libgnome-autoar-common-0.1.1-4; 0-01-01T00:00:00
|   libgnome-autoar-gtk-0-0-0.1.1-4+b1; 0-01-01T00:00:00
|   libgnome-bluetooth13-3.20.1-1; 0-01-01T00:00:00
|   libgnome-desktop-3-12-3.22.2-1; 0-01-01T00:00:00
|   libgnome-keyring-common-3.12.0-1; 0-01-01T00:00:00
|   libgnome-keyring0-3.12.0-1+b2; 0-01-01T00:00:00
|   libgnome-menu-3-0-3.13.3-9; 0-01-01T00:00:00
|   libgnomekbd-common-3.22.0.1-1; 0-01-01T00:00:00
|   libgnomekbd8-3.22.0.1-1+b1; 0-01-01T00:00:00
|   libgnutls30-3.5.8-5+deb9u6; 0-01-01T00:00:00
|   libgoa-1.0-0b-3.22.5-1; 0-01-01T00:00:00
|   libgoa-1.0-common-3.22.5-1; 0-01-01T00:00:00
|   libgoa-backend-1.0-1-3.22.5-1; 0-01-01T00:00:00
|   libgom-1.0-0-0.3.2-2; 0-01-01T00:00:00
|   libgom-1.0-common-0.3.2-2; 0-01-01T00:00:00
|   libgomp1-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libgpg-error0-1.26-2; 0-01-01T00:00:00
|   libgpgme11-1.8.0-3+b2; 0-01-01T00:00:00
|   libgphoto2-6-2.5.12-1; 0-01-01T00:00:00
|   libgphoto2-l10n-2.5.12-1; 0-01-01T00:00:00
|   libgphoto2-port12-2.5.12-1; 0-01-01T00:00:00
|   libgpm2-1.20.4-6.2+b1; 0-01-01T00:00:00
|   libgraphite2-3-1.3.10-1; 0-01-01T00:00:00
|   libgrilo-0.3-0-0.3.2-2+deb9u1; 0-01-01T00:00:00
|   libgs9-9.26a~dfsg-0+deb9u9; 0-01-01T00:00:00
|   libgs9-common-9.26a~dfsg-0+deb9u9; 0-01-01T00:00:00
|   libgsasl7-1.8.0-8+b2; 0-01-01T00:00:00
|   libgsf-1-114-1.14.41-1; 0-01-01T00:00:00
|   libgsf-1-common-1.14.41-1; 0-01-01T00:00:00
|   libgsf-bin-1.14.41-1; 0-01-01T00:00:00
|   libgsl2-2.3+dfsg-1; 0-01-01T00:00:00
|   libgsm1-1.0.13-4+b2; 0-01-01T00:00:00
|   libgsound0-1.0.2-1+b1; 0-01-01T00:00:00
|   libgspell-1-1-1.2.2-1; 0-01-01T00:00:00
|   libgspell-1-common-1.2.2-1; 0-01-01T00:00:00
|   libgssapi-krb5-2-1.15-1+deb9u3; 0-01-01T00:00:00
|   libgssdp-1.0-3-1.0.1-1+deb9u1; 0-01-01T00:00:00
|   libgstreamer-plugins-bad1.0-0-1.10.4-1+deb9u2; 0-01-01T00:00:00
|   libgstreamer-plugins-base1.0-0-1.10.4-1+deb9u2; 0-01-01T00:00:00
|   libgstreamer1.0-0-1.10.4-1; 0-01-01T00:00:00
|   libgtk-3-0-3.22.11-1; 0-01-01T00:00:00
|   libgtk-3-bin-3.22.11-1; 0-01-01T00:00:00
|   libgtk-3-common-3.22.11-1; 0-01-01T00:00:00
|   libgtk-vnc-2.0-0-0.6.0-3; 0-01-01T00:00:00
|   libgtk2-perl-2:1.2499-1; 0-01-01T00:00:00
|   libgtk2.0-0-2.24.31-2; 0-01-01T00:00:00
|   libgtk2.0-bin-2.24.31-2; 0-01-01T00:00:00
|   libgtk2.0-common-2.24.31-2; 0-01-01T00:00:00
|   libgtkmm-2.4-1v5-1:2.24.5-1; 0-01-01T00:00:00
|   libgtkmm-3.0-1v5-3.22.0-1; 0-01-01T00:00:00
|   libgtksourceview-3.0-1-3.22.2-1; 0-01-01T00:00:00
|   libgtksourceview-3.0-common-3.22.2-1; 0-01-01T00:00:00
|   libgtkspell0-2.0.16-1.1; 0-01-01T00:00:00
|   libgtkspell3-3-0-3.0.9-1; 0-01-01T00:00:00
|   libgtop-2.0-10-2.34.2-1; 0-01-01T00:00:00
|   libgtop2-common-2.34.2-1; 0-01-01T00:00:00
|   libgucharmap-2-90-7-1:9.0.2-1; 0-01-01T00:00:00
|   libgudev-1.0-0-230-3; 0-01-01T00:00:00
|   libgupnp-1.0-4-1.0.1-1+deb9u1; 0-01-01T00:00:00
|   libgupnp-av-1.0-2-0.12.10-1; 0-01-01T00:00:00
|   libgupnp-dlna-2.0-3-0.10.5-3+b1; 0-01-01T00:00:00
|   libgusb2-0.2.9-1+b1; 0-01-01T00:00:00
|   libgvnc-1.0-0-0.6.0-3; 0-01-01T00:00:00
|   libgweather-3-6-3.20.4-1; 0-01-01T00:00:00
|   libgweather-common-3.20.4-1; 0-01-01T00:00:00
|   libgxps2-0.2.4-1+b1; 0-01-01T00:00:00
|   libharfbuzz-icu0-1.4.2-1; 0-01-01T00:00:00
|   libharfbuzz0b-1.4.2-1; 0-01-01T00:00:00
|   libhogweed4-3.3-1+deb9u1; 0-01-01T00:00:00
|   libhtml-form-perl-6.03-1; 0-01-01T00:00:00
|   libhtml-format-perl-2.12-1; 0-01-01T00:00:00
|   libhtml-parser-perl-3.72-3; 0-01-01T00:00:00
|   libhtml-tagset-perl-3.20-3; 0-01-01T00:00:00
|   libhtml-template-perl-2.95-2; 0-01-01T00:00:00
|   libhtml-tree-perl-5.03-2; 0-01-01T00:00:00
|   libhttp-cookies-perl-6.01-1; 0-01-01T00:00:00
|   libhttp-daemon-perl-6.01-1; 0-01-01T00:00:00
|   libhttp-date-perl-6.02-1; 0-01-01T00:00:00
|   libhttp-message-perl-6.11-1; 0-01-01T00:00:00
|   libhttp-negotiate-perl-6.00-2; 0-01-01T00:00:00
|   libhttp-parser2.1-2.1-2; 0-01-01T00:00:00
|   libhunspell-1.4-0-1.4.1-2+b2; 0-01-01T00:00:00
|   libhyphen0-2.8.8-5; 0-01-01T00:00:00
|   libibus-1.0-5-1.5.14-3+deb9u2; 0-01-01T00:00:00
|   libical2-2.0.0-0.5+b1; 0-01-01T00:00:00
|   libice6-2:1.0.9-2; 0-01-01T00:00:00
|   libicu57-57.1-6+deb9u5; 0-01-01T00:00:00
|   libidn11-1.33-1; 0-01-01T00:00:00
|   libidn2-0-0.16-1+deb9u1; 0-01-01T00:00:00
|   libiec61883-0-1.2.0-2; 0-01-01T00:00:00
|   libieee1284-3-0.2.11-13; 0-01-01T00:00:00
|   libijs-0.35-0.35-12; 0-01-01T00:00:00
|   libilmbase12-2.2.0-12; 0-01-01T00:00:00
|   libimage-magick-perl-8:6.9.7.4+dfsg-11+deb9u14; 0-01-01T00:00:00
|   libimage-magick-q16-perl-8:6.9.7.4+dfsg-11+deb9u6; 0-01-01T00:00:00
|   libimobiledevice6-1.2.0+dfsg-3.1; 0-01-01T00:00:00
|   libinotifytools0-3.14-2; 0-01-01T00:00:00
|   libinput-bin-1.6.3-1; 0-01-01T00:00:00
|   libinput10-1.6.3-1; 0-01-01T00:00:00
|   libio-html-perl-1.001-1; 0-01-01T00:00:00
|   libio-socket-ssl-perl-2.044-1; 0-01-01T00:00:00
|   libip4tc0-1.6.0+snapshot20161117-6; 0-01-01T00:00:00
|   libip6tc0-1.6.0+snapshot20161117-6; 0-01-01T00:00:00
|   libipc-system-simple-perl-1.25-3; 0-01-01T00:00:00
|   libiptc0-1.6.0+snapshot20161117-6; 0-01-01T00:00:00
|   libiptcdata0-1.0.4-6+b1; 0-01-01T00:00:00
|   libisc-export160-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   libisc160-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   libisccc140-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   libisccfg140-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   libisl15-0.18-1; 0-01-01T00:00:00
|   libitm1-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libjack-jackd2-0-1.9.10+20150825git1ed50c92~dfsg-5; 0-01-01T00:00:00
|   libjansson4-2.9-1; 0-01-01T00:00:00
|   libjavascriptcoregtk-4.0-18-2.18.6-1~deb9u1; 0-01-01T00:00:00
|   libjbig0-2.1-3.1+b2; 0-01-01T00:00:00
|   libjbig2dec0-0.13-4.1+deb9u1; 0-01-01T00:00:00
|   libjemalloc1-3.6.0-9.1; 0-01-01T00:00:00
|   libjim0.76-0.76-2+b1; 0-01-01T00:00:00
|   libjpeg62-turbo-1:1.5.1-2+deb9u2; 0-01-01T00:00:00
|   libjson-glib-1.0-0-1.2.6-1; 0-01-01T00:00:00
|   libjson-glib-1.0-common-1.2.6-1; 0-01-01T00:00:00
|   libjxr-tools-1.1-6+b1; 0-01-01T00:00:00
|   libjxr0-1.1-6+b1; 0-01-01T00:00:00
|   libk5crypto3-1.15-1+deb9u3; 0-01-01T00:00:00
|   libkate1-0.4.1-7+b1; 0-01-01T00:00:00
|   libkeyutils1-1.5.9-9; 0-01-01T00:00:00
|   libklibc-2.0.4-9+deb9u1; 0-01-01T00:00:00
|   libkmod2-23-2; 0-01-01T00:00:00
|   libkpathsea6-2016.20160513.41080.dfsg-2+deb9u1; 0-01-01T00:00:00
|   libkrb5-3-1.15-1+deb9u3; 0-01-01T00:00:00
|   libkrb5support0-1.15-1+deb9u3; 0-01-01T00:00:00
|   libksba8-1.3.5-2; 0-01-01T00:00:00
|   liblapack3-3.7.0-2; 0-01-01T00:00:00
|   liblcms2-2-2.8-4+deb9u1; 0-01-01T00:00:00
|   liblcms2-utils-2.8-4+deb9u1; 0-01-01T00:00:00
|   libldap-2.4-2-2.4.44+dfsg-5+deb9u9; 0-01-01T00:00:00
|   libldap-common-2.4.44+dfsg-5+deb9u9; 0-01-01T00:00:00
|   libldb1-2:1.1.27-1+deb9u2; 0-01-01T00:00:00
|   liblilv-0-0-0.24.2~dfsg0-1; 0-01-01T00:00:00
|   liblirc-client0-0.9.4c-9; 0-01-01T00:00:00
|   libllvm3.9-1:3.9.1-9; 0-01-01T00:00:00
|   liblocale-gettext-perl-1.07-3+b1; 0-01-01T00:00:00
|   liblockfile-bin-1.14-1+b1; 0-01-01T00:00:00
|   liblogging-stdlog0-1.0.5-2+b2; 0-01-01T00:00:00
|   liblognorm5-2.0.1-1.1+b1; 0-01-01T00:00:00
|   liblouis-data-3.0.0-3+deb9u4; 0-01-01T00:00:00
|   liblouis12-3.0.0-3+deb9u4; 0-01-01T00:00:00
|   liblqr-1-0-0.4.2-2+b2; 0-01-01T00:00:00
|   liblsan0-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libltdl7-2.4.6-2; 0-01-01T00:00:00
|   liblua5.2-0-5.2.4-1.1+b2; 0-01-01T00:00:00
|   liblua5.3-0-5.3.3-1+deb9u1; 0-01-01T00:00:00
|   liblwp-mediatypes-perl-6.02-1; 0-01-01T00:00:00
|   liblwp-protocol-https-perl-6.06-2; 0-01-01T00:00:00
|   liblwres141-1:9.10.3.dfsg.P4-12.3+deb9u12; 0-01-01T00:00:00
|   liblz4-1-0.0~r131-2+deb9u1; 0-01-01T00:00:00
|   liblzma5-5.2.2-1.2+deb9u1; 0-01-01T00:00:00
|   liblzo2-2-2.08-1.2+b2; 0-01-01T00:00:00
|   libmad0-0.15.1b-8+deb9u1; 0-01-01T00:00:00
|   libmagic-mgc-1:5.30-1+deb9u3; 0-01-01T00:00:00
|   libmagic1-1:5.30-1+deb9u3; 0-01-01T00:00:00
|   libmagick++-6.q16-7-8:6.9.7.4+dfsg-11+deb9u14; 0-01-01T00:00:00
|   libmagickcore-6.q16-3-8:6.9.7.4+dfsg-11+deb9u14; 0-01-01T00:00:00
|   libmagickcore-6.q16-3-extra-8:6.9.7.4+dfsg-11+deb9u14; 0-01-01T00:00:00
|   libmagickwand-6.q16-3-8:6.9.7.4+dfsg-11+deb9u14; 0-01-01T00:00:00
|   libmailtools-perl-2.18-1; 0-01-01T00:00:00
|   libmariadb2-2.3.2-2; 0-01-01T00:00:00
|   libmariadbclient18-10.1.48-0+deb9u2; 0-01-01T00:00:00
|   libmbim-glib4-1.14.0-1+b1; 0-01-01T00:00:00
|   libmbim-proxy-1.14.0-1+b1; 0-01-01T00:00:00
|   libmediaart-2.0-0-1.9.0-2; 0-01-01T00:00:00
|   libmhash2-0.9.9.9-7; 0-01-01T00:00:00
|   libmission-control-plugins0-1:5.16.3-2.1; 0-01-01T00:00:00
|   libmjpegutils-2.1-0-1:2.1.0+debian-5; 0-01-01T00:00:00
|   libmm-glib0-1.6.4-1; 0-01-01T00:00:00
|   libmms0-0.6.4-2; 0-01-01T00:00:00
|   libmnl0-1.0.4-2; 0-01-01T00:00:00
|   libmodplug1-1:0.8.8.5-3; 0-01-01T00:00:00
|   libmount1-2.29.2-1+deb9u1; 0-01-01T00:00:00
|   libmozjs-24-0-24.2.0-5.1+b2; 0-01-01T00:00:00
|   libmp3lame0-3.99.5+repack1-9+b2; 0-01-01T00:00:00
|   libmpc3-1.0.3-1+b2; 0-01-01T00:00:00
|   libmpcdec6-2:0.1~r495-1+b1; 0-01-01T00:00:00
|   libmpdec2-2.4.2-1; 0-01-01T00:00:00
|   libmpeg2-4-0.5.1-7+b2; 0-01-01T00:00:00
|   libmpeg2encpp-2.1-0-1:2.1.0+debian-5; 0-01-01T00:00:00
|   libmpfr4-3.1.5-1; 0-01-01T00:00:00
|   libmpg123-0-1.23.8-1+b1; 0-01-01T00:00:00
|   libmplex2-2.1-0-1:2.1.0+debian-5; 0-01-01T00:00:00
|   libmpx2-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libmtdev1-1.1.5-1+b1; 0-01-01T00:00:00
|   libmtp-common-1.1.13-1; 0-01-01T00:00:00
|   libmtp-runtime-1.1.13-1; 0-01-01T00:00:00
|   libmtp9-1.1.13-1; 0-01-01T00:00:00
|   libmusicbrainz5-2-5.1.0+git20150707-6; 0-01-01T00:00:00
|   libmusicbrainz5cc2v5-5.1.0+git20150707-6; 0-01-01T00:00:00
|   libmutter0i-3.22.3-2; 0-01-01T00:00:00
|   libnautilus-extension1a-3.22.3-1+deb9u1; 0-01-01T00:00:00
|   libncurses5-6.0+20161126-1+deb9u2; 0-01-01T00:00:00
|   libncursesw5-6.0+20161126-1+deb9u2; 0-01-01T00:00:00
|   libndp0-1.6-1+b1; 0-01-01T00:00:00
|   libneon27-gnutls-0.30.2-2; 0-01-01T00:00:00
|   libnet-dbus-perl-1.1.0-4+b1; 0-01-01T00:00:00
|   libnet-http-perl-6.12-1; 0-01-01T00:00:00
|   libnet-smtp-ssl-perl-1.04-1; 0-01-01T00:00:00
|   libnet-ssleay-perl-1.80-1; 0-01-01T00:00:00
|   libnetfilter-conntrack3-1.0.6-2; 0-01-01T00:00:00
|   libnetpbm10-2:10.0-15.3+b2; 0-01-01T00:00:00
|   libnettle6-3.3-1+deb9u1; 0-01-01T00:00:00
|   libnewt0.52-0.52.19-1+b1; 0-01-01T00:00:00
|   libnfnetlink0-1.0.1-3; 0-01-01T00:00:00
|   libnfs8-1.11.0-2; 0-01-01T00:00:00
|   libnghttp2-14-1.18.1-1+deb9u2; 0-01-01T00:00:00
|   libnl-3-200-3.2.27-2; 0-01-01T00:00:00
|   libnl-genl-3-200-3.2.27-2; 0-01-01T00:00:00
|   libnm-glib4-1.6.2-3+deb9u2; 0-01-01T00:00:00
|   libnm-gtk0-1.4.4-1+deb9u1; 0-01-01T00:00:00
|   libnm-util2-1.6.2-3+deb9u2; 0-01-01T00:00:00
|   libnm0-1.6.2-3+deb9u2; 0-01-01T00:00:00
|   libnma0-1.4.4-1+deb9u1; 0-01-01T00:00:00
|   libnotify4-0.7.7-2; 0-01-01T00:00:00
|   libnpth0-1.3-1; 0-01-01T00:00:00
|   libnspr4-2:4.12-6; 0-01-01T00:00:00
|   libnss-mdns-0.10-8; 0-01-01T00:00:00
|   libnss-myhostname-232-25+deb9u14; 0-01-01T00:00:00
|   libnss3-2:3.26.2-1.1+deb9u5; 0-01-01T00:00:00
|   libntfs-3g871-1:2016.2.22AR.1+dfsg-1+deb9u3; 0-01-01T00:00:00
|   libntlm0-1.4-8+deb9u1; 0-01-01T00:00:00
|   libnuma1-2.0.11-2.1; 0-01-01T00:00:00
|   liboauth0-1.0.1-1; 0-01-01T00:00:00
|   libofa0-0.9.3-15; 0-01-01T00:00:00
|   libogg0-1.3.2-1; 0-01-01T00:00:00
|   libopenal-data-1:1.17.2-4; 0-01-01T00:00:00
|   libopenal1-1:1.17.2-4+b2; 0-01-01T00:00:00
|   libopencore-amrnb0-0.1.3-2.1+b2; 0-01-01T00:00:00
|   libopencore-amrwb0-0.1.3-2.1+b2; 0-01-01T00:00:00
|   libopencv-calib3d2.4v5-2.4.9.1+dfsg1-2+deb9u1; 0-01-01T00:00:00
|   libopencv-core2.4v5-2.4.9.1+dfsg1-2+deb9u1; 0-01-01T00:00:00
|   libopencv-features2d2.4v5-2.4.9.1+dfsg1-2+deb9u1; 0-01-01T00:00:00
|   libopencv-flann2.4v5-2.4.9.1+dfsg1-2+deb9u1; 0-01-01T00:00:00
|   libopencv-highgui2.4-deb0-2.4.9.1+dfsg1-2+deb9u1; 0-01-01T00:00:00
|   libopencv-imgproc2.4v5-2.4.9.1+dfsg1-2+deb9u1; 0-01-01T00:00:00
|   libopencv-objdetect2.4v5-2.4.9.1+dfsg1-2+deb9u1; 0-01-01T00:00:00
|   libopencv-video2.4v5-2.4.9.1+dfsg1-2+deb9u1; 0-01-01T00:00:00
|   libopenexr22-2.2.0-11+deb9u4; 0-01-01T00:00:00
|   libopenjp2-7-2.1.2-1.1+deb9u7; 0-01-01T00:00:00
|   libopenmpt0-0.2.7386~beta20.3-3+deb9u4; 0-01-01T00:00:00
|   libopts25-1:5.18.12-3; 0-01-01T00:00:00
|   libopus0-1.2~alpha2-1; 0-01-01T00:00:00
|   liborc-0.4-0-1:0.4.26-2; 0-01-01T00:00:00
|   libosinfo-1.0-0-1.0.0-2; 0-01-01T00:00:00
|   libp11-kit0-0.23.3-2+deb9u1; 0-01-01T00:00:00
|   libpackagekit-glib2-18-1.1.5-2+deb9u2; 0-01-01T00:00:00
|   libpam-cap-1:2.25-1; 0-01-01T00:00:00
|   libpam-gnome-keyring-3.20.0-3; 0-01-01T00:00:00
|   libpam-modules-1.1.8-3.6; 0-01-01T00:00:00
|   libpam-modules-bin-1.1.8-3.6; 0-01-01T00:00:00
|   libpam-runtime-1.1.8-3.6; 0-01-01T00:00:00
|   libpam-systemd-232-25+deb9u14; 0-01-01T00:00:00
|   libpam0g-1.1.8-3.6; 0-01-01T00:00:00
|   libpango-1.0-0-1.40.5-1; 0-01-01T00:00:00
|   libpango-perl-1.227-1+b1; 0-01-01T00:00:00
|   libpangocairo-1.0-0-1.40.5-1; 0-01-01T00:00:00
|   libpangoft2-1.0-0-1.40.5-1; 0-01-01T00:00:00
|   libpangomm-1.4-1v5-2.40.1-3; 0-01-01T00:00:00
|   libpangoxft-1.0-0-1.40.5-1; 0-01-01T00:00:00
|   libpaper-utils-1.1.24+nmu5; 0-01-01T00:00:00
|   libpaper1-1.1.24+nmu5; 0-01-01T00:00:00
|   libparted2-3.2-17; 0-01-01T00:00:00
|   libpcap0.8-1.8.1-3+deb9u1; 0-01-01T00:00:00
|   libpcaudio0-1.0-1; 0-01-01T00:00:00
|   libpci3-1:3.5.2-1; 0-01-01T00:00:00
|   libpciaccess0-0.13.4-1+b2; 0-01-01T00:00:00
|   libpcre2-8-0-10.22-3; 0-01-01T00:00:00
|   libpcre3-2:8.39-3; 0-01-01T00:00:00
|   libpcsclite1-1.8.20-1; 0-01-01T00:00:00
|   libpeas-1.0-0-1.20.0-1+b1; 0-01-01T00:00:00
|   libpeas-common-1.20.0-1; 0-01-01T00:00:00
|   libperl5.24-5.24.1-3+deb9u5; 0-01-01T00:00:00
|   libpgm-5.2-0-5.2.122~dfsg-2; 0-01-01T00:00:00
|   libphodav-2.0-0-2.1-1; 0-01-01T00:00:00
|   libphodav-2.0-common-2.1-1; 0-01-01T00:00:00
|   libphonenumber7-7.1.0-5+b1; 0-01-01T00:00:00
|   libpipeline1-1.4.1-2; 0-01-01T00:00:00
|   libpixman-1-0-0.34.0-1; 0-01-01T00:00:00
|   libplist3-1.12+git+1+e37ca00-0.3; 0-01-01T00:00:00
|   libpng16-16-1.6.28-1+deb9u1; 0-01-01T00:00:00
|   libpolkit-agent-1-0-0.105-18+deb9u2; 0-01-01T00:00:00
|   libpolkit-backend-1-0-0.105-18+deb9u2; 0-01-01T00:00:00
|   libpolkit-gobject-1-0-0.105-18+deb9u2; 0-01-01T00:00:00
|   libpoppler-glib8-0.48.0-2+deb9u4; 0-01-01T00:00:00
|   libpoppler64-0.48.0-2+deb9u4; 0-01-01T00:00:00
|   libpopt0-1.16-10+b2; 0-01-01T00:00:00
|   libpostproc54-7:3.2.18-0+deb9u1; 0-01-01T00:00:00
|   libpotrace0-1.13-3; 0-01-01T00:00:00
|   libprocps6-2:3.3.12-3+deb9u1; 0-01-01T00:00:00
|   libprotobuf10-3.0.0-9; 0-01-01T00:00:00
|   libproxy1-plugin-gsettings-0.4.14-2+deb9u2; 0-01-01T00:00:00
|   libproxy1-plugin-networkmanager-0.4.14-2+deb9u2; 0-01-01T00:00:00
|   libproxy1v5-0.4.14-2+deb9u2; 0-01-01T00:00:00
|   libpsl5-0.17.0-3; 0-01-01T00:00:00
|   libpst4-0.6.59-1+b1; 0-01-01T00:00:00
|   libpulse-mainloop-glib0-10.0-1+deb9u1; 0-01-01T00:00:00
|   libpulse0-10.0-1+deb9u1; 0-01-01T00:00:00
|   libpulsedsp-10.0-1+deb9u1; 0-01-01T00:00:00
|   libpwquality-common-1.3.0-1; 0-01-01T00:00:00
|   libpwquality1-1.3.0-1+b1; 0-01-01T00:00:00
|   libpython-stdlib-2.7.13-2; 0-01-01T00:00:00
|   libpython2.7-2.7.13-2+deb9u6; 0-01-01T00:00:00
|   libpython2.7-minimal-2.7.13-2+deb9u6; 0-01-01T00:00:00
|   libpython2.7-stdlib-2.7.13-2+deb9u6; 0-01-01T00:00:00
|   libpython3-stdlib-3.5.3-1; 0-01-01T00:00:00
|   libpython3.5-3.5.3-1+deb9u5; 0-01-01T00:00:00
|   libpython3.5-minimal-3.5.3-1+deb9u5; 0-01-01T00:00:00
|   libpython3.5-stdlib-3.5.3-1+deb9u5; 0-01-01T00:00:00
|   libqmi-glib5-1.16.2-1; 0-01-01T00:00:00
|   libqmi-proxy-1.16.2-1; 0-01-01T00:00:00
|   libquadmath0-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libquvi-0.9-0.9.3-0.9.3-1.2; 0-01-01T00:00:00
|   libquvi-scripts-0.9-0.9.20131130-1.1; 0-01-01T00:00:00
|   librarian0-0.8.1-6+b1; 0-01-01T00:00:00
|   libraw1394-11-2.1.2-1+b1; 0-01-01T00:00:00
|   libreadline5-5.2+dfsg-3+b1; 0-01-01T00:00:00
|   libreadline7-7.0-3; 0-01-01T00:00:00
|   libreoffice-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-avmedia-backend-gstreamer-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-base-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-base-core-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-base-drivers-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-calc-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-common-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-core-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-draw-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-evolution-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-gnome-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-gtk3-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-help-en-us-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-impress-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-java-common-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-math-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-ogltrans-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-pdfimport-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-report-builder-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-report-builder-bin-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-script-provider-bsh-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-script-provider-js-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-script-provider-python-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-sdbc-hsqldb-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-sdbc-postgresql-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-style-galaxy-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-style-tango-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   libreoffice-writer-1:5.2.7-1+deb9u4; 0-01-01T00:00:00
|   librest-0.7-0-0.8.0-2; 0-01-01T00:00:00
|   librevenge-0.0-0-0.0.4-6; 0-01-01T00:00:00
|   librsvg2-2-2.40.21-0+deb9u1; 0-01-01T00:00:00
|   librsvg2-common-2.40.21-0+deb9u1; 0-01-01T00:00:00
|   librtmp1-2.4+20151223.gitfa8646d.1-1+b1; 0-01-01T00:00:00
|   librubberband2-1.8.1-7; 0-01-01T00:00:00
|   librygel-core-2.6-2-0.32.1-3; 0-01-01T00:00:00
|   librygel-db-2.6-2-0.32.1-3; 0-01-01T00:00:00
|   librygel-renderer-2.6-2-0.32.1-3; 0-01-01T00:00:00
|   librygel-renderer-gst-2.6-2-0.32.1-3; 0-01-01T00:00:00
|   librygel-server-2.6-2-0.32.1-3; 0-01-01T00:00:00
|   libsac-java-gcj-1.3+dfsg-2; 0-01-01T00:00:00
|   libsamplerate0-0.1.8-8+deb9u1; 0-01-01T00:00:00
|   libsane-1.0.25-4.1+deb9u2; 0-01-01T00:00:00
|   libsane-common-1.0.25-4.1+deb9u2; 0-01-01T00:00:00
|   libsane-extras-1.0.22.4; 0-01-01T00:00:00
|   libsane-extras-common-1.0.22.4; 0-01-01T00:00:00
|   libsane-hpaio-3.16.11+repack0-3; 0-01-01T00:00:00
|   libsasl2-2-2.1.27~101-g0780600+dfsg-3+deb9u2; 0-01-01T00:00:00
|   libsasl2-modules-2.1.27~101-g0780600+dfsg-3+deb9u2; 0-01-01T00:00:00
|   libsasl2-modules-db-2.1.27~101-g0780600+dfsg-3+deb9u2; 0-01-01T00:00:00
|   libsbc1-1.3-2; 0-01-01T00:00:00
|   libseccomp2-2.3.1-2.1+deb9u1; 0-01-01T00:00:00
|   libsecret-1-0-0.18.5-3.1; 0-01-01T00:00:00
|   libsecret-common-0.18.5-3.1; 0-01-01T00:00:00
|   libselinux1-2.6-3+b3; 0-01-01T00:00:00
|   libsemanage-common-2.6-2; 0-01-01T00:00:00
|   libsemanage1-2.6-2; 0-01-01T00:00:00
|   libsensors4-1:3.4.0-4; 0-01-01T00:00:00
|   libsepol1-2.6-2; 0-01-01T00:00:00
|   libserd-0-0-0.26.0~dfsg0-1; 0-01-01T00:00:00
|   libshine3-3.1.0-5; 0-01-01T00:00:00
|   libshout3-2.3.1-3; 0-01-01T00:00:00
|   libsidplay1v5-1.36.59-10; 0-01-01T00:00:00
|   libsigc++-2.0-0v5-2.10.0-1; 0-01-01T00:00:00
|   libsigsegv2-2.10-5; 0-01-01T00:00:00
|   libslang2-2.3.1-5; 0-01-01T00:00:00
|   libsm6-2:1.2.2-1+b3; 0-01-01T00:00:00
|   libsmartcols1-2.29.2-1+deb9u1; 0-01-01T00:00:00
|   libsmbclient-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   libsnappy1v5-1.1.3-3; 0-01-01T00:00:00
|   libsndfile1-1.0.27-3+deb9u3; 0-01-01T00:00:00
|   libsndio6.1-1.1.0-3; 0-01-01T00:00:00
|   libsnmp-base-5.7.3+dfsg-1.7+deb9u3; 0-01-01T00:00:00
|   libsnmp30-5.7.3+dfsg-1.7+deb9u3; 0-01-01T00:00:00
|   libsodium18-1.0.11-2; 0-01-01T00:00:00
|   libsonic0-0.2.0-4+b1; 0-01-01T00:00:00
|   libsord-0-0-0.16.0~dfsg0-1+b1; 0-01-01T00:00:00
|   libsoundtouch1-1.9.2-2+deb9u1; 0-01-01T00:00:00
|   libsoup-gnome2.4-1-2.56.0-2+deb9u2; 0-01-01T00:00:00
|   libsoup2.4-1-2.56.0-2+deb9u2; 0-01-01T00:00:00
|   libsoxr0-0.1.2-2; 0-01-01T00:00:00
|   libspandsp2-0.0.6+dfsg-0.1; 0-01-01T00:00:00
|   libspectre1-0.2.8-1; 0-01-01T00:00:00
|   libspeechd2-0.8.6-4+deb9u1; 0-01-01T00:00:00
|   libspeex1-1.2~rc1.2-1+b2; 0-01-01T00:00:00
|   libspeexdsp1-1.2~rc1.2-1+b2; 0-01-01T00:00:00
|   libspice-client-glib-2.0-8-0.33-3.3+deb9u2; 0-01-01T00:00:00
|   libspice-client-gtk-3.0-5-0.33-3.3+deb9u2; 0-01-01T00:00:00
|   libsqlite3-0-3.16.2-5+deb9u3; 0-01-01T00:00:00
|   libsratom-0-0-0.6.0~dfsg0-1; 0-01-01T00:00:00
|   libsrtp0-1.4.5~20130609~dfsg-2; 0-01-01T00:00:00
|   libss2-1.43.4-2+deb9u2; 0-01-01T00:00:00
|   libssh-gcrypt-4-0.7.3-2+deb9u3; 0-01-01T00:00:00
|   libssh2-1-1.7.0-1+deb9u2; 0-01-01T00:00:00
|   libssl1.0.2-1.0.2u-1~deb9u7; 0-01-01T00:00:00
|   libssl1.1-1.1.0l-1~deb9u6; 0-01-01T00:00:00
|   libstartup-notification0-0.12-4+b2; 0-01-01T00:00:00
|   libstdc++-6-dev-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libstdc++6-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libstemmer0d-0+svn585-1+b2; 0-01-01T00:00:00
|   libswitch-perl-2.17-2; 0-01-01T00:00:00
|   libswresample2-7:3.2.18-0+deb9u1; 0-01-01T00:00:00
|   libswscale4-7:3.2.18-0+deb9u1; 0-01-01T00:00:00
|   libsystemd0-232-25+deb9u14; 0-01-01T00:00:00
|   libtag1v5-1.11.1+dfsg.1-0.3+deb9u1; 0-01-01T00:00:00
|   libtag1v5-vanilla-1.11.1+dfsg.1-0.3+deb9u1; 0-01-01T00:00:00
|   libtagc0-1.11.1+dfsg.1-0.3+deb9u1; 0-01-01T00:00:00
|   libtalloc2-2.1.8-1; 0-01-01T00:00:00
|   libtasn1-6-4.10-1.1+deb9u1; 0-01-01T00:00:00
|   libtbb2-4.3~20150611-2; 0-01-01T00:00:00
|   libtcl8.6-8.6.6+dfsg-1+b1; 0-01-01T00:00:00
|   libtdb1-1.3.11-2; 0-01-01T00:00:00
|   libteamdctl0-1.26-1+b1; 0-01-01T00:00:00
|   libtelepathy-glib0-0.24.1-1.1; 0-01-01T00:00:00
|   libtelepathy-logger3-0.8.2-2; 0-01-01T00:00:00
|   libterm-readkey-perl-2.37-1; 0-01-01T00:00:00
|   libtevent0-0.9.31-1; 0-01-01T00:00:00
|   libtext-charwidth-perl-0.04-7+b5; 0-01-01T00:00:00
|   libtext-iconv-perl-1.7-5+b4; 0-01-01T00:00:00
|   libtext-wrapi18n-perl-0.06-7.1; 0-01-01T00:00:00
|   libthai-data-0.1.26-1; 0-01-01T00:00:00
|   libthai0-0.1.26-1; 0-01-01T00:00:00
|   libtheora0-1.1.1+dfsg.1-14+b1; 0-01-01T00:00:00
|   libtie-ixhash-perl-1.23-2; 0-01-01T00:00:00
|   libtiff5-4.0.8-2+deb9u8; 0-01-01T00:00:00
|   libtimedate-perl-2.3000-2; 0-01-01T00:00:00
|   libtinfo5-6.0+20161126-1+deb9u2; 0-01-01T00:00:00
|   libtk8.6-8.6.6-1+b1; 0-01-01T00:00:00
|   libtotem-plparser-common-3.10.7-1; 0-01-01T00:00:00
|   libtotem-plparser18-3.10.7-1+b1; 0-01-01T00:00:00
|   libtotem0-3.22.1-1; 0-01-01T00:00:00
|   libtracker-control-1.0-0-1.10.5-1; 0-01-01T00:00:00
|   libtracker-miner-1.0-0-1.10.5-1; 0-01-01T00:00:00
|   libtracker-sparql-1.0-0-1.10.5-1; 0-01-01T00:00:00
|   libtsan0-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libtwolame0-0.3.13-2; 0-01-01T00:00:00
|   libtxc-dxtn-s2tc-1.0+git20151227-2; 0-01-01T00:00:00
|   libubsan0-6.3.0-18+deb9u1; 0-01-01T00:00:00
|   libudev1-232-25+deb9u14; 0-01-01T00:00:00
|   libudisks2-0-2.1.8-1+deb9u1; 0-01-01T00:00:00
|   libunistring0-0.9.6+really0.9.3-0.1; 0-01-01T00:00:00
|   libupower-glib3-0.99.4-4+b1; 0-01-01T00:00:00
|   liburi-perl-1.71-1; 0-01-01T00:00:00
|   libusb-0.1-4-2:0.1.12-30; 0-01-01T00:00:00
|   libusb-1.0-0-2:1.0.21-1; 0-01-01T00:00:00
|   libusbmuxd4-1.0.10-3+b1; 0-01-01T00:00:00
|   libusbredirhost1-0.7.1-1+deb9u1; 0-01-01T00:00:00
|   libusbredirparser1-0.7.1-1+deb9u1; 0-01-01T00:00:00
|   libustr-1.0-1-1.0.4-6; 0-01-01T00:00:00
|   libutempter0-1.1.6-3; 0-01-01T00:00:00
|   libuuid1-2.29.2-1+deb9u1; 0-01-01T00:00:00
|   libv4l-0-1.12.3-1; 0-01-01T00:00:00
|   libv4lconvert0-1.12.3-1; 0-01-01T00:00:00
|   libva-drm1-1.7.3-2; 0-01-01T00:00:00
|   libva-x11-1-1.7.3-2; 0-01-01T00:00:00
|   libva1-1.7.3-2; 0-01-01T00:00:00
|   libvdpau-va-gl1-0.4.2-1; 0-01-01T00:00:00
|   libvdpau1-1.1.1-6; 0-01-01T00:00:00
|   libvisio-0.1-1-0.1.5-4+b1; 0-01-01T00:00:00
|   libvisual-0.4-0-0.4.0-10; 0-01-01T00:00:00
|   libvo-aacenc0-0.1.3-1; 0-01-01T00:00:00
|   libvo-amrwbenc0-0.1.3-1; 0-01-01T00:00:00
|   libvorbis0a-1.3.5-4+deb9u3; 0-01-01T00:00:00
|   libvorbisenc2-1.3.5-4+deb9u3; 0-01-01T00:00:00
|   libvorbisfile3-1.3.5-4+deb9u3; 0-01-01T00:00:00
|   libvpx4-1.6.1-3+deb9u3; 0-01-01T00:00:00
|   libvte-2.91-0-0.46.1-1; 0-01-01T00:00:00
|   libvte-2.91-common-0.46.1-1; 0-01-01T00:00:00
|   libwacom-bin-0.22-1+b1; 0-01-01T00:00:00
|   libwacom-common-0.22-1; 0-01-01T00:00:00
|   libwacom2-0.22-1+b1; 0-01-01T00:00:00
|   libwavpack1-5.0.0-2+deb9u3; 0-01-01T00:00:00
|   libwayland-client0-1.12.0-1; 0-01-01T00:00:00
|   libwayland-cursor0-1.12.0-1; 0-01-01T00:00:00
|   libwayland-egl1-mesa-13.0.6-1+b2; 0-01-01T00:00:00
|   libwayland-server0-1.12.0-1; 0-01-01T00:00:00
|   libwbclient0-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   libwebkit2gtk-4.0-37-2.18.6-1~deb9u1; 0-01-01T00:00:00
|   libwebp6-0.5.2-1+deb9u1; 0-01-01T00:00:00
|   libwebpmux2-0.5.2-1+deb9u1; 0-01-01T00:00:00
|   libwebrtc-audio-processing1-0.3-1; 0-01-01T00:00:00
|   libwildmidi-config-0.4.0-2; 0-01-01T00:00:00
|   libwildmidi2-0.4.0-2+b2; 0-01-01T00:00:00
|   libwinpr-crt0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-crypto0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-dsparse0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-environment0.1-1.1.0~git20140921.1.440916e+dfsg1-13+de; 0-01-01T00:00:00
|   libwinpr-error0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-file0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-handle0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-heap0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-input0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-interlocked0.1-1.1.0~git20140921.1.440916e+dfsg1-13+de; 0-01-01T00:00:00
|   libwinpr-library0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-path0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-pool0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-registry0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u; 0-01-01T00:00:00
|   libwinpr-rpc0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-sspi0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-synch0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-sysinfo0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-thread0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwinpr-utils0.1-1.1.0~git20140921.1.440916e+dfsg1-13+deb9u4; 0-01-01T00:00:00
|   libwmf-bin-0.2.8.4-10.6; 0-01-01T00:00:00
|   libwmf0.2-7-0.2.8.4-10.6; 0-01-01T00:00:00
|   libwnck-3-0-3.20.1-3; 0-01-01T00:00:00
|   libwnck-3-common-3.20.1-3; 0-01-01T00:00:00
|   libwpd-0.10-10-0.10.1-5+deb9u1; 0-01-01T00:00:00
|   libwpg-0.3-3-0.3.1-3; 0-01-01T00:00:00
|   libwrap0-7.6.q-26; 0-01-01T00:00:00
|   libwww-perl-6.15-1; 0-01-01T00:00:00
|   libwww-robotrules-perl-6.01-1; 0-01-01T00:00:00
|   libx11-6-2:1.6.4-3+deb9u4; 0-01-01T00:00:00
|   libx11-data-2:1.6.4-3+deb9u4; 0-01-01T00:00:00
|   libx11-protocol-perl-0.56-7; 0-01-01T00:00:00
|   libx11-xcb1-2:1.6.4-3+deb9u4; 0-01-01T00:00:00
|   libx264-148-2:0.148.2748+git97eaef2-1; 0-01-01T00:00:00
|   libx265-95-2.1-2+b2; 0-01-01T00:00:00
|   libxapian30-1.4.3-2+deb9u2; 0-01-01T00:00:00
|   libxatracker2-13.0.6-1+b2; 0-01-01T00:00:00
|   libxau6-1:1.0.8-1; 0-01-01T00:00:00
|   libxaw7-2:1.0.13-1+b2; 0-01-01T00:00:00
|   libxcb-dri2-0-1.12-1; 0-01-01T00:00:00
|   libxcb-dri3-0-1.12-1; 0-01-01T00:00:00
|   libxcb-glx0-1.12-1; 0-01-01T00:00:00
|   libxcb-icccm4-0.4.1-1; 0-01-01T00:00:00
|   libxcb-image0-0.4.0-1+b2; 0-01-01T00:00:00
|   libxcb-keysyms1-0.4.0-1+b2; 0-01-01T00:00:00
|   libxcb-present0-1.12-1; 0-01-01T00:00:00
|   libxcb-randr0-1.12-1; 0-01-01T00:00:00
|   libxcb-render-util0-0.3.9-1; 0-01-01T00:00:00
|   libxcb-render0-1.12-1; 0-01-01T00:00:00
|   libxcb-res0-1.12-1; 0-01-01T00:00:00
|   libxcb-shape0-1.12-1; 0-01-01T00:00:00
|   libxcb-shm0-1.12-1; 0-01-01T00:00:00
|   libxcb-sync1-1.12-1; 0-01-01T00:00:00
|   libxcb-util0-0.3.8-3+b2; 0-01-01T00:00:00
|   libxcb-xf86dri0-1.12-1; 0-01-01T00:00:00
|   libxcb-xfixes0-1.12-1; 0-01-01T00:00:00
|   libxcb-xkb1-1.12-1; 0-01-01T00:00:00
|   libxcb-xv0-1.12-1; 0-01-01T00:00:00
|   libxcb1-1.12-1; 0-01-01T00:00:00
|   libxcomposite1-1:0.4.4-2; 0-01-01T00:00:00
|   libxcursor1-1:1.1.14-1+deb9u2; 0-01-01T00:00:00
|   libxdamage1-1:1.1.4-2+b3; 0-01-01T00:00:00
|   libxdmcp6-1:1.1.2-3; 0-01-01T00:00:00
|   libxext6-2:1.3.3-1+b2; 0-01-01T00:00:00
|   libxfixes3-1:5.0.3-1; 0-01-01T00:00:00
|   libxfont1-1:1.5.2-4; 0-01-01T00:00:00
|   libxfont2-1:2.0.1-3+deb9u2; 0-01-01T00:00:00
|   libxft2-2.3.2-1+b2; 0-01-01T00:00:00
|   libxi6-2:1.7.9-1; 0-01-01T00:00:00
|   libxinerama1-2:1.1.3-1+b3; 0-01-01T00:00:00
|   libxkbcommon-x11-0-0.7.1-2~deb9u1; 0-01-01T00:00:00
|   libxkbcommon0-0.7.1-2~deb9u1; 0-01-01T00:00:00
|   libxkbfile1-1:1.0.9-2; 0-01-01T00:00:00
|   libxklavier16-5.4-2; 0-01-01T00:00:00
|   libxml-commons-resolver1.1-java-1.2-7; 0-01-01T00:00:00
|   libxml-parser-perl-2.44-2+b1; 0-01-01T00:00:00
|   libxml-twig-perl-1:3.50-1; 0-01-01T00:00:00
|   libxml-xpathengine-perl-0.13-1; 0-01-01T00:00:00
|   libxml2-2.9.4+dfsg1-2.2+deb9u7; 0-01-01T00:00:00
|   libxmu6-2:1.1.2-2; 0-01-01T00:00:00
|   libxmuu1-2:1.1.2-2; 0-01-01T00:00:00
|   libxpm4-1:3.5.12-1; 0-01-01T00:00:00
|   libxrandr2-2:1.5.1-1; 0-01-01T00:00:00
|   libxrender1-1:0.9.10-1; 0-01-01T00:00:00
|   libxres1-2:1.0.7-1+b3; 0-01-01T00:00:00
|   libxshmfence1-1.2-1+b2; 0-01-01T00:00:00
|   libxslt1.1-1.1.29-2.1; 0-01-01T00:00:00
|   libxss1-1:1.2.2-1; 0-01-01T00:00:00
|   libxt6-1:1.1.5-1; 0-01-01T00:00:00
|   libxtables12-1.6.0+snapshot20161117-6; 0-01-01T00:00:00
|   libxtst6-2:1.2.3-1; 0-01-01T00:00:00
|   libxv1-2:1.0.11-1; 0-01-01T00:00:00
|   libxvidcore4-2:1.3.4-1+b2; 0-01-01T00:00:00
|   libxvmc1-2:1.0.10-1; 0-01-01T00:00:00
|   libxxf86dga1-2:1.1.4-1+b3; 0-01-01T00:00:00
|   libxxf86vm1-1:1.1.4-1+b2; 0-01-01T00:00:00
|   libyaml-0-2-0.1.7-2; 0-01-01T00:00:00
|   libyelp0-3.22.0-1; 0-01-01T00:00:00
|   libytnef0-1.9.2-2; 0-01-01T00:00:00
|   libzapojit-0.0-0-0.0.3-3; 0-01-01T00:00:00
|   libzbar0-0.10+doc-10.1; 0-01-01T00:00:00
|   libzeitgeist-2.0-0-0.9.16-0.2+b1; 0-01-01T00:00:00
|   libzmq5-4.2.1-4+deb9u4; 0-01-01T00:00:00
|   libzvbi-common-0.2.35-13; 0-01-01T00:00:00
|   libzvbi0-0.2.35-13; 0-01-01T00:00:00
|   linux-base-4.5; 0-01-01T00:00:00
|   linux-image-4.9.0-8-amd64-4.9.130-2; 0-01-01T00:00:00
|   linux-image-amd64-4.9+80+deb9u6; 0-01-01T00:00:00
|   linux-libc-dev-4.9.320-2; 0-01-01T00:00:00
|   locales-2.24-11+deb9u3; 0-01-01T00:00:00
|   login-1:4.4-4.1+deb9u1; 0-01-01T00:00:00
|   logrotate-3.11.0-0.1; 0-01-01T00:00:00
|   lsb-base-9.20161125; 0-01-01T00:00:00
|   lsb-release-9.20161125; 0-01-01T00:00:00
|   lsof-4.89+dfsg-0.1; 0-01-01T00:00:00
|   lua-bitop-1.0.2-4; 0-01-01T00:00:00
|   lua-expat-1.3.0-4; 0-01-01T00:00:00
|   lua-json-1.3.3-2; 0-01-01T00:00:00
|   lua-lpeg-1.0.0-1; 0-01-01T00:00:00
|   lua-socket-3.0~rc1+git+ac3201d-3; 0-01-01T00:00:00
|   make-4.1-9.1; 0-01-01T00:00:00
|   man-db-2.7.6.1-2; 0-01-01T00:00:00
|   manpages-4.10-2; 0-01-01T00:00:00
|   manpages-dev-4.10-2; 0-01-01T00:00:00
|   mariadb-client-10.1-10.1.48-0+deb9u2; 0-01-01T00:00:00
|   mariadb-client-10.1.48-0+deb9u2; 0-01-01T00:00:00
|   mariadb-client-core-10.1-10.1.48-0+deb9u2; 0-01-01T00:00:00
|   mariadb-common-10.1.48-0+deb9u2; 0-01-01T00:00:00
|   mariadb-server-10.1-10.1.48-0+deb9u2; 0-01-01T00:00:00
|   mariadb-server-10.1.48-0+deb9u2; 0-01-01T00:00:00
|   mariadb-server-core-10.1-10.1.48-0+deb9u2; 0-01-01T00:00:00
|   mawk-1.3.3-17+b3; 0-01-01T00:00:00
|   mesa-va-drivers-13.0.6-1+b2; 0-01-01T00:00:00
|   mesa-vdpau-drivers-13.0.6-1+b2; 0-01-01T00:00:00
|   mime-support-3.60; 0-01-01T00:00:00
|   minissdpd-1.2.20130907-4.1; 0-01-01T00:00:00
|   mobile-broadband-provider-info-20161204-1; 0-01-01T00:00:00
|   modemmanager-1.6.4-1; 0-01-01T00:00:00
|   mount-2.29.2-1+deb9u1; 0-01-01T00:00:00
|   mousetweaks-3.12.0-1+b1; 0-01-01T00:00:00
|   msmtp-1.6.6-1; 0-01-01T00:00:00
|   multiarch-support-2.24-11+deb9u3; 0-01-01T00:00:00
|   mutter-3.22.3-2; 0-01-01T00:00:00
|   mutter-common-3.22.3-2; 0-01-01T00:00:00
|   mysql-common-5.8+1.0.2; 0-01-01T00:00:00
|   nano-2.7.4-1; 0-01-01T00:00:00
|   nautilus-3.22.3-1+deb9u1; 0-01-01T00:00:00
|   nautilus-data-3.22.3-1+deb9u1; 0-01-01T00:00:00
|   nautilus-sendto-3.8.4-2+b1; 0-01-01T00:00:00
|   ncurses-base-6.0+20161126-1+deb9u2; 0-01-01T00:00:00
|   ncurses-bin-6.0+20161126-1+deb9u2; 0-01-01T00:00:00
|   ncurses-term-6.0+20161126-1+deb9u2; 0-01-01T00:00:00
|   net-tools-1.60+git20161116.90da8a0-1; 0-01-01T00:00:00
|   netbase-5.4; 0-01-01T00:00:00
|   netcat-traditional-1.10-41+b1; 0-01-01T00:00:00
|   netfilter-persistent-1.0.4+nmu2; 0-01-01T00:00:00
|   netpbm-2:10.0-15.3+b2; 0-01-01T00:00:00
|   network-manager-1.6.2-3+deb9u2; 0-01-01T00:00:00
|   network-manager-gnome-1.4.4-1+deb9u1; 0-01-01T00:00:00
|   notification-daemon-3.20.0-1+b1; 0-01-01T00:00:00
|   ntfs-3g-1:2016.2.22AR.1+dfsg-1+deb9u3; 0-01-01T00:00:00
|   ntp-1:4.2.8p10+dfsg-3+deb9u2; 0-01-01T00:00:00
|   openjdk-8-jre-headless-8u332-ga-1~deb9u1; 0-01-01T00:00:00
|   openssh-client-1:7.4p1-10+deb9u6; 0-01-01T00:00:00
|   openssl-1.1.0l-1~deb9u6; 0-01-01T00:00:00
|   os-prober-1.76~deb9u1; 0-01-01T00:00:00
|   osinfo-db-0.20180226-1~deb9u1; 0-01-01T00:00:00
|   ossec-hids-server-3.1.0.5732stretch; 0-01-01T00:00:00
|   p11-kit-0.23.3-2+deb9u1; 0-01-01T00:00:00
|   p11-kit-modules-0.23.3-2+deb9u1; 0-01-01T00:00:00
|   p7zip-16.02+dfsg-3+deb9u1; 0-01-01T00:00:00
|   p7zip-full-16.02+dfsg-3+deb9u1; 0-01-01T00:00:00
|   packagekit-1.1.5-2+deb9u2; 0-01-01T00:00:00
|   packagekit-tools-1.1.5-2+deb9u2; 0-01-01T00:00:00
|   parted-3.2-17; 0-01-01T00:00:00
|   passwd-1:4.4-4.1+deb9u1; 0-01-01T00:00:00
|   patch-2.7.5-1+deb9u2; 0-01-01T00:00:00
|   pciutils-1:3.5.2-1; 0-01-01T00:00:00
|   perl-5.24.1-3+deb9u5; 0-01-01T00:00:00
|   perl-base-5.24.1-3+deb9u5; 0-01-01T00:00:00
|   perl-modules-5.24-5.24.1-3+deb9u5; 0-01-01T00:00:00
|   perl-openssl-defaults-3; 0-01-01T00:00:00
|   php-common-1:49; 0-01-01T00:00:00
|   php7.0-7.0.33-0+deb9u12; 0-01-01T00:00:00
|   php7.0-cli-7.0.33-0+deb9u12; 0-01-01T00:00:00
|   php7.0-common-7.0.33-0+deb9u12; 0-01-01T00:00:00
|   php7.0-json-7.0.33-0+deb9u12; 0-01-01T00:00:00
|   php7.0-mysql-7.0.33-0+deb9u12; 0-01-01T00:00:00
|   php7.0-opcache-7.0.33-0+deb9u12; 0-01-01T00:00:00
|   php7.0-readline-7.0.33-0+deb9u12; 0-01-01T00:00:00
|   pinentry-curses-1.0.0-2; 0-01-01T00:00:00
|   pinentry-gnome3-1.0.0-2; 0-01-01T00:00:00
|   polari-3.22.2-1; 0-01-01T00:00:00
|   policykit-1-0.105-18+deb9u2; 0-01-01T00:00:00
|   poppler-data-0.4.7-8; 0-01-01T00:00:00
|   poppler-utils-0.48.0-2+deb9u4; 0-01-01T00:00:00
|   postfix-3.1.8-0+deb9u1; 0-01-01T00:00:00
|   postfix-sqlite-3.1.8-0+deb9u1; 0-01-01T00:00:00
|   powermgmt-base-1.31+nmu1; 0-01-01T00:00:00
|   ppp-2.4.7-1+4+deb9u1; 0-01-01T00:00:00
|   printer-driver-gutenprint-5.2.11-1+b2; 0-01-01T00:00:00
|   printer-driver-pnm2ppa-1.13-8; 0-01-01T00:00:00
|   procps-2:3.3.12-3+deb9u1; 0-01-01T00:00:00
|   psmisc-22.21-2.1+b2; 0-01-01T00:00:00
|   pulseaudio-10.0-1+deb9u1; 0-01-01T00:00:00
|   pulseaudio-module-bluetooth-10.0-1+deb9u1; 0-01-01T00:00:00
|   pulseaudio-utils-10.0-1+deb9u1; 0-01-01T00:00:00
|   python-2.7.13-2; 0-01-01T00:00:00
|   python-apt-common-1.4.3; 0-01-01T00:00:00
|   python-bs4-4.5.3-1; 0-01-01T00:00:00
|   python-cairo-1.8.8-2.2; 0-01-01T00:00:00
|   python-cffi-backend-1.9.1-2; 0-01-01T00:00:00
|   python-chardet-2.3.0-2; 0-01-01T00:00:00
|   python-crypto-2.6.1-7; 0-01-01T00:00:00
|   python-cryptography-1.7.1-3; 0-01-01T00:00:00
|   python-dnspython-1.15.0-1; 0-01-01T00:00:00
|   python-enum34-1.1.6-1; 0-01-01T00:00:00
|   python-gi-3.22.0-2; 0-01-01T00:00:00
|   python-html5lib-0.999999999-1; 0-01-01T00:00:00
|   python-idna-2.2-1; 0-01-01T00:00:00
|   python-ipaddress-1.0.17-1; 0-01-01T00:00:00
|   python-ldb-2:1.1.27-1+deb9u2; 0-01-01T00:00:00
|   python-lxml-3.7.1-1+deb9u5; 0-01-01T00:00:00
|   python-minimal-2.7.13-2; 0-01-01T00:00:00
|   python-numpy-1:1.12.1-3; 0-01-01T00:00:00
|   python-openssl-16.2.0-1; 0-01-01T00:00:00
|   python-pkg-resources-33.1.1-1; 0-01-01T00:00:00
|   python-pyasn1-0.1.9-2; 0-01-01T00:00:00
|   python-requests-2.12.4-1; 0-01-01T00:00:00
|   python-samba-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   python-scour-0.32-2; 0-01-01T00:00:00
|   python-setuptools-33.1.1-1; 0-01-01T00:00:00
|   python-six-1.10.0-3; 0-01-01T00:00:00
|   python-talloc-2.1.8-1; 0-01-01T00:00:00
|   python-tdb-1.3.11-2; 0-01-01T00:00:00
|   python-urllib3-1.19.1-1+deb9u1; 0-01-01T00:00:00
|   python-webencodings-0.5-2; 0-01-01T00:00:00
|   python2.7-2.7.13-2+deb9u6; 0-01-01T00:00:00
|   python2.7-minimal-2.7.13-2+deb9u6; 0-01-01T00:00:00
|   python3-3.5.3-1; 0-01-01T00:00:00
|   python3-apt-1.4.3; 0-01-01T00:00:00
|   python3-brlapi-5.4-7+deb9u1; 0-01-01T00:00:00
|   python3-cairo-1.10.0+dfsg-5+b1; 0-01-01T00:00:00
|   python3-chardet-2.3.0-2; 0-01-01T00:00:00
|   python3-cups-1.9.73-1; 0-01-01T00:00:00
|   python3-cupshelpers-1.5.7-3; 0-01-01T00:00:00
|   python3-dbus-1.2.4-1+b1; 0-01-01T00:00:00
|   python3-debian-0.1.30; 0-01-01T00:00:00
|   python3-debianbts-2.6.1; 0-01-01T00:00:00
|   python3-gi-3.22.0-2; 0-01-01T00:00:00
|   python3-gi-cairo-3.22.0-2; 0-01-01T00:00:00
|   python3-httplib2-0.9.2+dfsg-1; 0-01-01T00:00:00
|   python3-louis-3.0.0-3+deb9u4; 0-01-01T00:00:00
|   python3-minimal-3.5.3-1; 0-01-01T00:00:00
|   python3-pkg-resources-33.1.1-1; 0-01-01T00:00:00
|   python3-pyatspi-2.20.3+dfsg-1; 0-01-01T00:00:00
|   python3-pycurl-7.43.0-2; 0-01-01T00:00:00
|   python3-pysimplesoap-1.16-2; 0-01-01T00:00:00
|   python3-reportbug-7.1.7+deb9u2; 0-01-01T00:00:00
|   python3-requests-2.12.4-1; 0-01-01T00:00:00
|   python3-six-1.10.0-3; 0-01-01T00:00:00
|   python3-smbc-1.0.15.6-1; 0-01-01T00:00:00
|   python3-software-properties-0.96.20.2-1+deb9u1; 0-01-01T00:00:00
|   python3-speechd-0.8.6-4+deb9u1; 0-01-01T00:00:00
|   python3-urllib3-1.19.1-1+deb9u1; 0-01-01T00:00:00
|   python3-xdg-0.25-4+deb9u1; 0-01-01T00:00:00
|   python3.5-3.5.3-1+deb9u5; 0-01-01T00:00:00
|   python3.5-minimal-3.5.3-1+deb9u5; 0-01-01T00:00:00
|   rarian-compat-0.8.1-6+b1; 0-01-01T00:00:00
|   readline-common-7.0-3; 0-01-01T00:00:00
|   realmd-0.16.3-1; 0-01-01T00:00:00
|   rename-0.20-4; 0-01-01T00:00:00
|   reportbug-7.1.7+deb9u2; 0-01-01T00:00:00
|   rsync-3.1.2-1+deb9u3; 0-01-01T00:00:00
|   rsyslog-8.24.0-1+deb9u3; 0-01-01T00:00:00
|   rtkit-0.11-4+b1; 0-01-01T00:00:00
|   rygel-0.32.1-3; 0-01-01T00:00:00
|   rygel-playbin-0.32.1-3; 0-01-01T00:00:00
|   rygel-tracker-0.32.1-3; 0-01-01T00:00:00
|   samba-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   samba-common-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   samba-common-bin-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   samba-dsdb-modules-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   samba-libs-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   samba-vfs-modules-2:4.5.16+dfsg-1+deb9u4; 0-01-01T00:00:00
|   sane-utils-1.0.25-4.1+deb9u2; 0-01-01T00:00:00
|   seahorse-3.20.0-3.1; 0-01-01T00:00:00
|   seahorse-daemon-3.12.2-3; 0-01-01T00:00:00
|   sed-4.4-1; 0-01-01T00:00:00
|   sensible-utils-0.0.9+deb9u1; 0-01-01T00:00:00
|   sgml-base-1.29; 0-01-01T00:00:00
|   sgml-data-2.0.10; 0-01-01T00:00:00
|   shared-mime-info-1.8-1+deb9u1; 0-01-01T00:00:00
|   shotwell-0.25.4+really0.24.5-0.1; 0-01-01T00:00:00
|   shotwell-common-0.25.4+really0.24.5-0.1; 0-01-01T00:00:00
|   snmpd-5.7.3+dfsg-1.7+deb9u3; 0-01-01T00:00:00
|   socat-1.7.3.1-2+deb9u1; 0-01-01T00:00:00
|   software-properties-common-0.96.20.2-1+deb9u1; 0-01-01T00:00:00
|   software-properties-gtk-0.96.20.2-1+deb9u1; 0-01-01T00:00:00
|   sound-theme-freedesktop-0.8-1; 0-01-01T00:00:00
|   speech-dispatcher-0.8.6-4+deb9u1; 0-01-01T00:00:00
|   speech-dispatcher-audio-plugins-0.8.6-4+deb9u1; 0-01-01T00:00:00
|   speech-dispatcher-espeak-ng-0.8.6-4+deb9u1; 0-01-01T00:00:00
|   spice-client-glib-usb-acl-helper-0.33-3.3+deb9u2; 0-01-01T00:00:00
|   ssl-cert-1.0.39; 0-01-01T00:00:00
|   sudo-1.8.19p1-2.1+deb9u3; 0-01-01T00:00:00
|   synaptic-0.84.2; 0-01-01T00:00:00
|   system-config-printer-common-1.5.7-3; 0-01-01T00:00:00
|   system-config-printer-udev-1.5.7-3+b1; 0-01-01T00:00:00
|   systemd-232-25+deb9u14; 0-01-01T00:00:00
|   systemd-sysv-232-25+deb9u14; 0-01-01T00:00:00
|   sysvinit-utils-2.88dsf-59.9; 0-01-01T00:00:00
|   tar-1.29b-1.1+deb9u1; 0-01-01T00:00:00
|   task-desktop-3.39; 0-01-01T00:00:00
|   task-english-3.39; 0-01-01T00:00:00
|   tasksel-3.39; 0-01-01T00:00:00
|   tasksel-data-3.39; 0-01-01T00:00:00
|   tcl-expect-5.45-7+deb9u1; 0-01-01T00:00:00
|   tcl8.6-8.6.6+dfsg-1+b1; 0-01-01T00:00:00
|   tcpd-7.6.q-26; 0-01-01T00:00:00
|   tdb-tools-1.3.11-2; 0-01-01T00:00:00
|   telepathy-idle-0.2.0-2+b1; 0-01-01T00:00:00
|   telepathy-logger-0.8.2-2; 0-01-01T00:00:00
|   telepathy-mission-control-5-1:5.16.3-2.1; 0-01-01T00:00:00
|   telnet-0.17-41; 0-01-01T00:00:00
|   tftpd-hpa-5.2+20150808-1+b1; 0-01-01T00:00:00
|   tk8.6-8.6.6-1+b1; 0-01-01T00:00:00
|   totem-3.22.1-1; 0-01-01T00:00:00
|   totem-common-3.22.1-1; 0-01-01T00:00:00
|   totem-plugins-3.22.1-1; 0-01-01T00:00:00
|   traceroute-1:2.1.0-2; 0-01-01T00:00:00
|   tracker-1.10.5-1; 0-01-01T00:00:00
|   tracker-extract-1.10.5-1; 0-01-01T00:00:00
|   tracker-gui-1.10.5-1; 0-01-01T00:00:00
|   tracker-miner-fs-1.10.5-1; 0-01-01T00:00:00
|   transfig-1:3.2.6a-2+deb9u4; 0-01-01T00:00:00
|   transmission-gtk-2.92-2+deb9u1; 0-01-01T00:00:00
|   tzdata-2021a-0+deb9u4; 0-01-01T00:00:00
|   ucf-3.0036; 0-01-01T00:00:00
|   udev-232-25+deb9u14; 0-01-01T00:00:00
|   udisks2-2.1.8-1+deb9u1; 0-01-01T00:00:00
|   unattended-upgrades-0.93.1+nmu1; 0-01-01T00:00:00
|   unzip-6.0-21; 0-01-01T00:00:00
|   update-inetd-4.44; 0-01-01T00:00:00
|   upower-0.99.4-4+b1; 0-01-01T00:00:00
|   usb-modeswitch-2.5.0+repack0-1; 0-01-01T00:00:00
|   usb-modeswitch-data-20170120-1; 0-01-01T00:00:00
|   usbmuxd-1.1.0-2+b2; 0-01-01T00:00:00
|   usbutils-1:007-4+b1; 0-01-01T00:00:00
|   util-linux-2.29.2-1+deb9u1; 0-01-01T00:00:00
|   util-linux-locales-2.29.2-1+deb9u1; 0-01-01T00:00:00
|   va-driver-all-1.7.3-2; 0-01-01T00:00:00
|   vdpau-driver-all-1.1.1-6; 0-01-01T00:00:00
|   vim-common-2:8.0.0197-4+deb9u7; 0-01-01T00:00:00
|   vim-tiny-2:8.0.0197-4+deb9u7; 0-01-01T00:00:00
|   vinagre-3.22.0-1+b1; 0-01-01T00:00:00
|   vino-3.22.0-1; 0-01-01T00:00:00
|   wamerican-7.1-1; 0-01-01T00:00:00
|   wget-1.18-5+deb9u3; 0-01-01T00:00:00
|   whiptail-0.52.19-1+b1; 0-01-01T00:00:00
|   wireless-regdb-2016.06.10-1; 0-01-01T00:00:00
|   wodim-9:1.1.11-3+b2; 0-01-01T00:00:00
|   wpasupplicant-2:2.4-1+deb9u9; 0-01-01T00:00:00
|   x11-apps-7.7+6+b1; 0-01-01T00:00:00
|   x11-common-1:7.7+19; 0-01-01T00:00:00
|   x11-session-utils-7.7+2+b1; 0-01-01T00:00:00
|   x11-utils-7.7+3+b1; 0-01-01T00:00:00
|   x11-xkb-utils-7.7+3+b1; 0-01-01T00:00:00
|   x11-xserver-utils-7.7+7+b1; 0-01-01T00:00:00
|   xauth-1:1.0.9-1+b2; 0-01-01T00:00:00
|   xbitmaps-1.1.1-2; 0-01-01T00:00:00
|   xbrlapi-5.4-7+deb9u1; 0-01-01T00:00:00
|   xdg-user-dirs-0.15-2+b1; 0-01-01T00:00:00
|   xdg-user-dirs-gtk-0.10-1+b1; 0-01-01T00:00:00
|   xdg-utils-1.1.1-1+deb9u1; 0-01-01T00:00:00
|   xfonts-100dpi-1:1.0.4+nmu1; 0-01-01T00:00:00
|   xfonts-75dpi-1:1.0.4+nmu1; 0-01-01T00:00:00
|   xfonts-base-1:1.0.4+nmu1; 0-01-01T00:00:00
|   xfonts-encodings-1:1.0.4-2; 0-01-01T00:00:00
|   xfonts-scalable-1:1.0.3-1.1; 0-01-01T00:00:00
|   xfonts-utils-1:7.7+4; 0-01-01T00:00:00
|   xinit-1.3.4-3+b1; 0-01-01T00:00:00
|   xkb-data-2.19-1+deb9u1; 0-01-01T00:00:00
|   xml-core-0.17; 0-01-01T00:00:00
|   xorg-1:7.7+19; 0-01-01T00:00:00
|   xorg-docs-core-1:1.7.1-1; 0-01-01T00:00:00
|   xserver-common-2:1.19.2-1+deb9u9; 0-01-01T00:00:00
|   xserver-xephyr-2:1.19.2-1+deb9u9; 0-01-01T00:00:00
|   xserver-xorg-1:7.7+19; 0-01-01T00:00:00
|   xserver-xorg-core-2:1.19.2-1+deb9u9; 0-01-01T00:00:00
|   xserver-xorg-input-all-1:7.7+19; 0-01-01T00:00:00
|   xserver-xorg-input-libinput-0.23.0-2; 0-01-01T00:00:00
|   xserver-xorg-input-wacom-0.34.0-1; 0-01-01T00:00:00
|   xserver-xorg-legacy-2:1.19.2-1+deb9u9; 0-01-01T00:00:00
|   xserver-xorg-video-all-1:7.7+19; 0-01-01T00:00:00
|   xserver-xorg-video-amdgpu-1.2.0-1+b1; 0-01-01T00:00:00
|   xserver-xorg-video-ati-1:7.8.0-1+b1; 0-01-01T00:00:00
|   xserver-xorg-video-fbdev-1:0.4.4-1+b5; 0-01-01T00:00:00
|   xserver-xorg-video-intel-2:2.99.917+git20161206-1; 0-01-01T00:00:00
|   xserver-xorg-video-nouveau-1:1.0.13-3; 0-01-01T00:00:00
|   xserver-xorg-video-qxl-0.1.4+20161126git4d7160c-1; 0-01-01T00:00:00
|   xserver-xorg-video-radeon-1:7.8.0-1+b1; 0-01-01T00:00:00
|   xserver-xorg-video-vesa-1:2.3.4-1+b2; 0-01-01T00:00:00
|   xserver-xorg-video-vmware-1:13.2.1-1+b1; 0-01-01T00:00:00
|   xterm-327-2+deb9u3; 0-01-01T00:00:00
|   xwayland-2:1.19.2-1+deb9u9; 0-01-01T00:00:00
|   xxd-2:8.0.0197-4+deb9u7; 0-01-01T00:00:00
|   xz-utils-5.2.2-1.2+deb9u1; 0-01-01T00:00:00
|   yelp-3.22.0-1; 0-01-01T00:00:00
|   yelp-xsl-3.20.1-2; 0-01-01T00:00:00
|   zeitgeist-core-0.9.16-0.2+b1; 0-01-01T00:00:00
|   zenity-3.22.0-1+b1; 0-01-01T00:00:00
|   zenity-common-3.22.0-1; 0-01-01T00:00:00
|   zlib1g-1:1.2.8.dfsg-5+deb9u1; 0-01-01T00:00:00
|_  zlibc-0.9k-4.3; 0-01-01T00:00:00
5353/udp open|filtered zeroconf
Too many fingerprints match this host to give specific OS details
Network Distance: 2 hops
Service Info: Host: JOY

Host script results:
|_clock-skew: 2s
|_nbstat: NetBIOS name: JOY, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)

TRACEROUTE (using port 1023/udp)
HOP RTT     ADDRESS
1   0.43 ms 192.168.17.1
2   0.97 ms 10.0.88.34

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 275.61 seconds
```

</details>

# 4. TFTP

The public SNMP results from nmap revealed that there is a TFTP service on `36969` that is mapped to `/home/patrick`

```console
|   751:
|     Name: in.tftpd
|     Path: /usr/sbin/in.tftpd
|     Params: --listen --user tftp --address 0.0.0.0:36969 --secure /home/patrick
```

Unlike FTP, we cannot list directory with TFTP, but we conveniently found a directory listing of Patrick's home directory previously; let's retrieve the `version_control` file

```console
┌──(root㉿kali)-[~]
└─# tftp 10.0.88.34 36969
tftp> get version_control
┌──(root㉿kali)-[~]
└─# cat version_control
Version Control of External-Facing Services:

Apache: 2.4.25
Dropbear SSH: 0.34
ProFTPd: 1.3.5
Samba: 4.5.12

We should switch to OpenSSH and upgrade ProFTPd.

Note that we have some other configurations in this machine.
1. The webroot is no longer /var/www/html. We have changed it to /var/www/tryingharderisjoy.
2. I am trying to perform some simple bash scripting tutorials. Let me see how it turns out.
```
