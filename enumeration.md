# ❗ENUMERATE HARDER❗

The motto of OffSec is **try harder**, but this practicially means **enumerate harder**

❗Try harder ≠ brute force❗

Try harder means you missed something that was not enumerated, and this can sometimes mean:

1. There a a port in nmap result that is not checked, just `nc` to the port and see what it says
2. Run the same [web scan](#2-web-scanning) with a bigger wordlist - there will never be an empty web server with default web page, there must be something in it

# 1. Network scanning

## 1.1. netdiscover

netdiscover uses ARP to scan the network i.e. the scanner needs to be in the same subnet to work

Ref: <https://manpages.debian.org/bullseye/netdiscover/netdiscover.8.en.html>

```console
sudo netdiscover -r $TARGET_RANGE
```
## 1.2. nmap

Ref: <https://manpages.debian.org/bullseye/nmap/nmap.1.en.html>

- `-sn` : Ping Scan - disable port scan
- `-Pn` : Treat all hosts as online -- skip host discovery
- `-p $PORT_RANGE` : Only scan specified ports; `-p-` scans all ports
- `-sV` : Probe open ports to determine service/version info
- `-sS` / `-sT` / `-sA` / `-sW` / `-sM` : TCP SYN / Connect() / ACK / Window / Maimon scans
- `-sU` : UDP Scan
- `-sN` / `-sF` / `-sX` : TCP Null / FIN / Xmas scans
- `-A` : Enables OS detection `-O`, version scanning `-sV`, script scanning `-sC` and traceroute `--traceroute`
- `-T<0-5>` : Set timing template (higher is faster) <paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), insane (5)>

### Examples

Initial network sweep

```console
nmap -sn $TARGET_RANGE
```

Scan all ports with service detection

```console
nmap -p- -sV $TARGET_IP
```

Scan with more details (note that using `-p-` with `-A` will take a long time)

```console
nmap -A $TARGET_IP
```

# 2. Web scanning

## 2.1. Wordlists

### Default dirb wordlist:

```console
/usr/share/dirb/wordlists/common.txt
```

### SecLists

Web discovery wordlists from [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content): `/usr/share/seclists/Discovery/Web-Content/`

Install [SecLists](https://www.kali.org/tools/seclists/) in Kali with `sudo apt -y install SecList`

#### combined_words.txt

```console
/usr/share/seclists/Discovery/Web-Content/combined_words.txt
```

128k entries, is a combination of the following wordlists:

- big.txt
- common.txt
- raft-large-words-lowercase.txt
- raft-large-words.txt
- raft-medium-words-lowercase.txt
- raft-medium-words.txt
- raft-small-words-lowercase.txt
- raft-small-words.txt

☝️ The description for `combined_words.txt` says it's use for files, but it is commonly useful for directories as well

#### combined_directories.txt

```console
/usr/share/seclists/Discovery/Web-Content/combined_directories.txt
```

1.37m entries, is a combination of the following wordlists:

- apache.txt
- combined_words.txt
- directory-list-1.0.txt
- directory-list-2.3-big.txt
- directory-list-2.3-medium.txt
- directory-list-2.3-small.txt
- raft-large-directories-lowercase.txt
- raft-large-directories.txt
- raft-medium-directories-lowercase.txt
- raft-medium-directories.txt
- raft-small-directories-lowercase.txt
- raft-small-directories.txt

☝️ `combined_directories.txt` is such a huge list that you either end up crashing the web server or having to reduce the scan rate; either ways use caution and keep in mind that it may not be effective to use this list

## 2.2. dirb

Syntax:

```console
dirb <URL> [wordlist]
```

Examples:

- Using default wordlist:

```console
dirb https://10.0.88.39:2390
```

- Using `combined_words.txt` from `SecLists`:

```console
dirb https://10.0.88.39:2390 /usr/share/seclists/Discovery/Web-Content/combined_words.txt
```

## 2.3. gobuster

Syntax:

```console
gobuster <command> -u <URL> -w <wordlist> -t <concurrent-threads> -o <output-file>
```

Example:

```console
gobuster dir -u http://10.0.88.39:2390 -w /usr/share/seclists/Discovery/Web-Content/combined_words.txt
```

☝️ `gobuster` is observed to be significantly faster than `dirb` thanks to the threaded scan (default: 10 threads)
