# powershell

---

## general commands

| Command name        | Alias                         | Description                                                  |
| :------------------ | :---------------------------- | :----------------------------------------------------------- |
| `Set-Location`      | cd, chdir, sl                 | Sets the current working location to a specified location.   |
| `Get-Content`       | cat, gc, type                 | Gets the content of the item at the specified location.      |
| `Add-Content`       | ac                            | Adds content to the specified items, such as adding words to a file. |
| `Set-Content`       | sc                            | Writes or replaces the content in an item with new content.  |
| `Copy-Item`         | copy, cp, cpi                 | Copies an item from one location to another.                 |
| `Remove-Item`       | del, erase, rd, ri, rm, rmdir | Deletes the specified items.                                 |
| `Move-Item`         | mi, move, mv                  | Moves an item from one location to another.                  |
| `Set-Item`          | si                            | Changes the value of an item to the value specified in the command. |
| `New-Item`          | ni                            | Creates a new item.                                          |
| `Start-Job`         | sajb                          | Starts a Windows PowerShell background job.                  |
| `Compare-Object`    | compare, dif                  | Compares two sets of objects.                                |
| `Group-Object`      | group                         | Groups objects that contain the same value for specified properties. |
| `Invoke-WebRequest` | curl, iwr, wget               | Gets content from a web page on the Internet.                |
| `Measure-Object`    | measure                       | Calculates the numeric properties of objects, and the characters, words, and lines in string objects, such as files … |
| `Resolve-Path`      | rvpa                          | Resolves the wildcard characters in a path, and displays the path contents. |
| `Resume-Job`        | rujb                          | Restarts a suspended job                                     |
| `Set-Variable`      | set, sv                       | Sets the value of a variable. Creates the variable if one with the requested name does not exist. |
| `Show-Command`      | shcm                          | Creates Windows PowerShell commands in a graphical command window. |
| `Sort-Object`       | sort                          | Sorts objects by property values.                            |
| `Start-Service`     | sasv                          | Starts one or more stopped services.                         |
| `Start-Process`     | saps, start                   | Starts one or more processes on the local computer.          |
| `Suspend-Job`       | sujb                          | Temporarily stops workflow jobs.                             |
| `Wait-Job`          | wjb                           | Suppresses the command prompt until one or all of the Windows PowerShell background jobs running in the session are … |
| `Where-Object`      | ?, where                      | Selects objects from a collection based on their property values. |
| `Write-Output`      | echo, write                   | Sends the specified objects to the next command in the pipeline. If the command is the last command in the pipeline,… |

## ad related

`Import-Module ActiveDirectory` - import AD module

`Get-ADDomain` - get basic domain info

`Get-ADDomainController -filter * | select hostname, operatingsystem` - get all domain controllers by hostname and OS

`Get-ADFineGrainedPasswordPolicy -filter *` - get all fine-grained password policies

`Get-ADDefaultDomainPasswordPolicy` - get default password ploicy

`Get-ADUser username -Properties *` - get user and list all properties (replace username)

`Get-ADUser -Filter *` - get all AD users

`Get-ADComputer -filter *` - get all AD computers

`Get-Service` - get all services

`Get-Process` - get all processes

`Enter-PSSession -ComputerName` - start interactive session with remote computer

## smb lmao

Disabling SMBv1

```powershell
Get-WindowsOptionalFeature -Online -FeatureName smb1protocol
```

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName smb1protocol
```

Listing fileshares

```
Get
```

