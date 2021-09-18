# brief walkthrough

nmap scan revealed port 22 and 80 open.

website had one of the owasp top 10: xxe payload

used that to get credentials

python script ran as root, so had to inject some code to get root