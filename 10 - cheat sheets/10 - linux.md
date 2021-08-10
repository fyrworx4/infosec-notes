# linox

### package management

* `apt install` - install package

* `apt remove` - remove package

* `apt autoremove` - remove dependencies

* `apt purge` - remove package + configurations

* `dpkg` - manage debian packages

### insane linux

* `Ctrl + L` - clear terminal

* `Ctrl + R` - search command history

* `Ctrl + A` - go to beginning of line

* `Ctrl + E` - go to end of line

* `Alt + B` - go back one word

* `Alt + F` - go forward one word

* `Ctrl + U` - cut everything before cursor

* `Ctrl + K` - cut everything after cursor

* `Ctrl + Y` - paste

### services

* `systemctl status sshd` or `service ssh status` - view status of service

* `systemctl restart sshd` or `service ssh restart` - restart service

* `systemctl stop sshd` or `service ssh stop` - stop service

* `systemctl start sshd` or `service ssh start` - start service

### nano

##### file handling

* `Ctrl + S` - save
* `Ctrl + O` - save as
* `Ctrl + X` - exit

##### cursor movement

* `Ctrl + A` - start of line

* `Ctrl + E` - end of line

* `Ctrl + Y` - one page up

* `Ctrl + V` - one page down

##### searching

* `Ctrl + Q` - backward search

* `Ctrl + W` - forward search

* `Alt + Q` - find next occurrence backward

* `Alt + W` - find next occurrence forward

##### editing

* `Alt + A` - turn mark on/off

* `Ctrl + K` - cut

* `Alt + 6` - copy

* `Ctrl + U` - paste

* `Alt + U` - undo

* `Alt + E` - redo

* `Ctrl + K` - remove line

##### macros

* `Alt + :` - start / stop macro

* `Alt + ;` - replay macro

##### misc

* `Alt + N` - toggle line numbers

### vim

???

### networking

* `ifconfig` or `ip addr` - displays IP address
* `iwconfig` - displays IP address for wireless interface
* `ping` - ping an address
* `arp -a` - associates MAC to IP
* `netstat` - shows network stats
* `netstat -tulpn` - the bread and butter of connections
* `route` - show routing table
* `dhclient` - show stats of DHCP client



# sysadmin commmands

### find

### hostname

```bash
sudo hostnamectl set-hostname [hostname]
```

### users and groups

add a new user:

```bash
sudo useradd -m -s /b[username]
sudo passwd  [username]
```

add user to sudo group

```bash
usermod -aG sudo [username]
```

add user to sudoers fiile

```bash
sudo visudo
```

add:

```bash
[username] ALL=ALL NOPASSWD:ALL
```

change username of user

```bash
usermod -l new_name old_name
```

disable user

```bash
usermod -L [username]
passwd -l []
```

### VirtualBox Guest Additions

```bash
sudo apt update
sudo apt upgrade
sudo apt install build-essential dkms linux-headers-$(uname -r)
```

# tmux

