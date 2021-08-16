# nmap

## nmap switches

* `-sS` - SYN scan
* `-sU` - UDP scan
* `-O` - OS detection
* `-sV` - Service detection
* `-v` - Verbose

* `-vv` - Extra verbose
* `-oA` - Save nmap output to 3 major formats
* `-oN` - Save nmap output to normal format
* `-oG` - Save nmap output to grepable format
* `-A` - Aggressive mode, or "all" (does service detection, OS detection, traceroute, and scripts)
* `-T5` - Set timing to level 5
* `-p 80` - Scan only port 80
* `-p 1000-1500` - Scan ports 1000 to 1500
* `-p-` - Scan all ports
* `--script` - Activate a script from nmap scripting library
* `--script=vuln` - Activate all scripts from "vuln" category

## scan types

* `-sT` - TCP Connect Scan
* `-sS` - SYN Half-Open Scan
* `-sU` - UDP Scan
* `-sN` - TCP Null Scan
* `-sF` - TCP FIN Scan
* `-sX` - TCP Xmas Scan

## tcp connect scans

a tcp connect scan performs the TCP 3-way handshake with the specified port, then determines whether the service is up/down based on its response.

* closed port - receive a RST packet
* open port - receive a SYN/ACK packet
* filtered - receives nothing

Quick tip: always configure a firewall to respond with RST TCP packet:

```bash
iptables -I INPUT -p tcp --dport <port> -j REJECT --reject-with tcp-reset
```

## syn scans

a syn half-open / stealth scan also preforms the 3-way handshake with the specified port, but instead of completing the handshake, will reset the connection by sending a RST packet in the 3rd step of the handshake.

if you run nmap as sudo, this scan is ran be default. otherwise, a tcp connect scan is ran.

## udp scans

udp doesn't do acknowledgements, so it can be slower and more difficult to scan. when a packet is sent to a UDP port:

* `open|filtered` - no response (after 2 tries)
* `open` - get a response
* `closed` - get a ping response

Quick tip: use the `--top-ports` flag when doing UDP scans

## null, fin, xmas scans

these are other types of scans used to evade firewalls.

### null

sends a TCP request with no flags.

* closed port - receive a RST packet
* open port - no response...?

### fin

sends a TCP request with the FIN flag.

* closed port - receive a RST packet
* open port - get no response

### xmas

sends a malformed TCP packet with the flags PSH, URG, and FIN

* closed port - receive a RST packet
* open port - get no response

why xmas? because in Wireshark it looks like a blinking Christmas tree

## icmp network scanning

used to figure out what's on the network

```bash
nmap -sn 192.168.0.1-254
```

```bash
nmap -sn 192.168.0.0/24
```

## nse scripts

written in Lua

use `--script` to select a script to use. some scripts has arguments, which you define by using `--script-args`

### searching for scripts

scripts are located in `/usr/share/nmap/scripts`

get more info on scripts by looking at `/usr/share/nmap/scripts/script.db`

## firewall evasion

* `-Pn` - disable ping discovery
  * windows blocks all ICMP packets, which is bad for us. however, we can use the `-Pn` flag to disable pinging host before scanning it
* `-f` - fragment packets
* `--mtu <number>` - set maximum transmission unit size (must be multiple of 8)
* `--scan-delay <time>ms` - delays packets being sent
* `--badsum` - generate invalid checksum

* `--data-length` - add random data to end of packet