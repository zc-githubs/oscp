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

|Option|Description|
|---|---|
|`-sn`|Ping Scan - disable port scan|
|`-Pn`|Treat all hosts as online -- skip host discovery|
|`-p $PORT_RANGE`|Only scan specified ports; `-p-` scans all ports<br>[By default](https://nmap.org/book/performance-port-selection.html), Nmap scans the **top 1,000 ports** for each scan protocol requested|
|`-F`|Fast mode - Scan fewer ports than the default scan<br>(scans the **most common 100 ports** for each scanned protocol)|
|`-sV`|Probe open ports to determine service/version info|
|`-sS` / `-sT` / `-sA` / `-sW` / `-sM`|TCP SYN / Connect() / ACK / Window / Maimon scans|
|`-sU`|UDP Scan|
|`-sN` / `-sF` / `-sX`|TCP Null / FIN / Xmas scans|
|`-A`|Enables OS detection `-O`, version scanning `-sV`, script scanning `-sC` and traceroute `--traceroute`|
|`-T<0-5>`|Set timing template (higher is faster) <paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), insane (5)>|

### Examples

|Command|Usage|
|---|---|
|`nmap -sn $TARGET_RANGE`|Initial network sweep|
|`nmap -p- -sV $TARGET_IP`|Scan all ports with service detection|
|`nmap -A $TARGET_IP`|Scan with more details<br>(note that using `-p-` with `-A` can take a long time)|
|`nmap -sU -A -F $TARGET_IP`|Scan most common 100 ports on UDP<br>(UDP scans take a long time because of the wait time to confirm if a port is open,<br>scanning just the top 100 ports with `-F` should balance between speed and coverage)|

# 2. Web scanning

## 2.1. Wordlists

### Default dirb wordlist:

```console
/usr/share/dirb/wordlists/common.txt
```

### SecLists

Web discovery wordlists from [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content): `/usr/share/seclists/Discovery/Web-Content/`

Install [SecLists](https://www.kali.org/tools/seclists/) in Kali with `sudo apt -y install SecList`

|`/usr/share/seclists/Discovery/Web-Content/combined_words.txt`|`/usr/share/seclists/Discovery/Web-Content/combined_directories.txt`|
|---|---|
|128k entries, is a combination of the following wordlists:<ul><li>big.txt</li><li>common.txt</li><li>raft-large-words-lowercase.txt</li><li>raft-large-words.txt</li><li>raft-medium-words-lowercase.txt</li><li>raft-medium-words.txt</li><li>raft-small-words-lowercase.txt</li><li>raft-small-words.txt</li></ul>|1.37m entries, is a combination of the following wordlists:<ul><li>apache.txt</li><li>combined_words.txt</li><li>directory-list-1.0.txt</li><li>directory-list-2.3-big.txt</li><li>directory-list-2.3-medium.txt</li><li>directory-list-2.3-small.txt</li><li>raft-large-directories-lowercase.txt</li><li>raft-large-directories.txt</li><li>raft-medium-directories-lowercase.txt</li><li>raft-medium-directories.txt</li><li>raft-small-directories-lowercase.txt</li><li>raft-small-directories.txt</li></ul>|

☝️ **Note:**
- The description for `combined_words.txt` says it's use for files, but it is commonly useful for directories as well
- `combined_directories.txt` is such a huge list that you either end up crashing the web server or having to reduce the scan rate; either ways use caution and keep in mind that it may not be effective to use this list


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

## ☝️ dirb vs gobuster

`gobuster` is observed to be significantly faster than `dirb` thanks to the threaded scan (default: 10 threads)

However, `dirb` does recursive enumeration (i.e. `\word\word\...`), `gobuster` does not

Recursive + single-thread means that `dirb` can be crawling slow with large wordlists
