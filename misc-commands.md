# Network Discovery using `netdiscover`
Ref: <https://manpages.debian.org/bullseye/netdiscover/netdiscover.8.en.html>
```console
sudo netdiscover -r <range>
```
# Port Scan using `nmap`
Ref: <https://manpages.debian.org/bullseye/nmap/nmap.1.en.html>
```console
nmap -p- -sV <ip-address>
```
```console
nmap -A <ip-address>
```
```console
nmap -sN <ip-address>
```
# Running a web server to host files
```console
cp /<path>/<to>/<file> .
sudo python3 -m http.server 80 &> /dev/null &
```
# Downloading files
## Using `curl`
Ref: <https://manpages.debian.org/bullseye/curl/curl.1.en.html>
```console
curl -kLO https://<url>
```
## Using `wget`
Ref: <https://manpages.debian.org/bullseye/wget/wget.1.en.html>
```console
wget -O- https://<url>
```
