# Covenant

## Covenant Terms

* Listener
* Grunt

## Covenant Grunt Commands

* `whoami` - who am I
* `Seatbelt -group=all` - get system info about everything
* `GetDomainUser` - get domain users

## How to get a grunt

* Make sure attacker's antivirus is turned off
* Create a Listener
  * Be sure to change ConnectAddresses
* Go into Launchers
  * Click on Powershell
  * Select Listener
  * Go into  Host
  * type in `/rev.ps1` and click host
  * Take note of the EncodedLauncher command
* Get Windows machine to run the command
* Once ran, go into Grunts
* Enjoy!

## Enumeration with Covenant

### Seatbelt

```
Seatbelt -group=all
```

### normal powershell

```
GetDomainUser
```

```
GetNetLoggedOnUser
```

```
GetNetLocalGroup
```

## Importing stuff into covenant

### PowerUp.ps1

```
PowershellImport

* select file * 

powershell i
```

### ???

