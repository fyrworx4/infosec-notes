# misconfigure windows

## autologon misconfig

Set registry key to 1:

```
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
```

Edit / create new keys:

| Name              | Type   | Value        |
| ----------------- | ------ | ------------ |
| DefaultUserName   | String | s.chisholm   |
| DefaultPassword   | String | FallOutBoy1! |
| DefaultDomainName | String | mayorsec     |
| AutoAdminLogon    | String | 1            |

Exploiting:

* Get a Covenant Grunt
* PowerUp.ps1

```
PowershellImport
powershell invoke-allchecks

Seatbelt
```

## alwaysinstallelevated

Registry key:

```
Computer\HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Installer
```

* Name: AlwaysInstallElevated
* Type: REG_DWORD
* Data: 1

