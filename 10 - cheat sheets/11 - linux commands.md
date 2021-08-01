# linox commands

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

