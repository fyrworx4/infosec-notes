# linux - privesc

## privilege escalation vs exploitation

* exploit some vuln to escalate
* exploitation doesn't have to escalate
* to escalate, you need to exploit

### how to escalate

* abuse crontabs
* abuse suid/guid
* edit/abuse misconfigured files
  * `/etc/sudoers`
  * sshd.config file
* guess/crack pw
* steal/abuse ssh keys
* find other running services
* environment variables
* use known shell exploits
* read command history

## persistence

* reverse shells (netcat)
* setup your own services/backdoors
  * webshell (user is www-data)
* get info about system
* https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Persistence.md

