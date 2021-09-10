# brief walkthrough of explore box

nmap scan on box to get open ports:

- 2222 - Banana Studio SSH
- 5555 - freeciv
- 42135 - es file explorer

used es file explorer python exploit to read the filesystem

* credentials were found in /sdcard

used banana studio exploit from exploit.db to get root

* had to connect to localhost:5555
