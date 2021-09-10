# nat holepunch



```
sudo iptables -P FORWARD DROP
sudo iptables -A FORWARD -i eth0 -o wg0 -p tcp --syn --dport 10527 -m conntrack --ctstate NEW -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o wg0 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A FORWARD -i wg0 -o eth0 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 10527 -j DNAT --to-destination 192.168.4.2
sudo iptables -t nat -A POSTROUTING -o wg0 -p tcp --dport 10527 -d 192.168.4.2 -j SNAT --to-source 192.168.4.1
```



