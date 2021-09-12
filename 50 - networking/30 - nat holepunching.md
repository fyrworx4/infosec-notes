# nat holepunch

## install wireguard

Install wireguard on both machines.

```bash
sudo apt update
sudo apt upgrade
sudo apt install wireguard
sudo reboot
```

## generate public and private key

Copy the entire code block and paste into terminal of both machines.

```bash
(umask 077 && printf "[Interface]\nPrivateKey = " | sudo tee /etc/wireguard/wg0.conf > /dev/null)
wg genkey | sudo tee -a /etc/wireguard/wg0.conf | wg pubkey | sudo tee /etc/wireguard/publickey
```

## edit wireguard configurations

```bash
sudo nano /etc/wireguard/wg0.conf
```

Configure /etc/wireguard/wg0.conf on cloud machine:

```
[Interface]
PrivateKey = qHOQs4...
ListenPort = 55107
Address = 192.168.4.1

[Peer]
PublicKey =  ums9y... <--- public key from the machine at home
AllowedIPs = 192.168.4.2/32
```

on the site machine:

```
[Interface]
PrivateKey = OKNAiUi/u...
Address = 192.168.4.2

[Peer]
PublicKey = GJtb+O7nnT... <---- public key from VPS
AllowedIPs = 192.168.4.1/32
Endpoint = 18.184.64.177:55107
PersistentKeepalive = 25
```

## enable wireguard

```bash
sudo systemctl start wg-quick@wg0
sudo systemctl enable wg-quick@wg0
```

*ensure that ports are open

## NAT traffic forwarding

Copy and paste entire code block, just be sure to change the dport:

```
sudo iptables -P FORWARD DROP
sudo iptables -A FORWARD -i eth0 -o wg0 -p tcp --syn --dport 10527 -m conntrack --ctstate NEW -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o wg0 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A FORWARD -i wg0 -o eth0 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 10527 -j DNAT --to-destination 192.168.4.2
sudo iptables -t nat -A POSTROUTING -o wg0 -p tcp --dport 10527 -d 192.168.4.2 -j SNAT --to-source 192.168.4.1
```



