# netdiscover
Ref: <https://manpages.debian.org/bullseye/netdiscover/netdiscover.8.en.html>
```console
sudo netdiscover -r $TARGET_RANGE
```
# nmap
Ref: <https://manpages.debian.org/bullseye/nmap/nmap.1.en.html>

- `-sn` : Ping Scan - disable port scan
- `-Pn` : Treat all hosts as online -- skip host discovery
- `-p $PORT_RANGE` : Only scan specified ports; `-p-` scans all ports
- `-sV` : Probe open ports to determine service/version info
- `-sS` / `-sT` / `-sA` / `-sW` / `-sM` : TCP SYN / Connect() / ACK / Window / Maimon scans
- `-sU` : UDP Scan
- `-sN` / `-sF` / `-sX` : TCP Null / FIN / Xmas scans
- `-A` : Enables OS detection `-O`, version scanning `-sV`, script scanning `-sC` and traceroute `--traceroute`
- `-T<0-5>` : Set timing template (higher is faster) <paranoid (0) | sneaky (1) | polite (2) | normal (3) | aggressive (4) | insane (5)>

## Examples
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
