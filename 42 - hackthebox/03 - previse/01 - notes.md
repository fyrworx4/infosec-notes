# notes

* Ran nmap scan, found port 22 and port 80

Found this website:

![image-20210912114849446](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912114849446.png)

Used gobuster to list directories:

```
gobuster dir -w /usr/share/wordlists/dirb/big.txt -o gobuster -x ".php" -u http://10.10.11.104
```

```
/.htaccess            (Status: 403) [Size: 277]
/.htpasswd.php        (Status: 403) [Size: 277]
/.htaccess.php        (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/accounts.php         (Status: 302) [Size: 3994] [--> login.php]
/config.php           (Status: 200) [Size: 0]
/css                  (Status: 301) [Size: 310] [--> http://10.10.11.104/css/]
/download.php         (Status: 302) [Size: 0] [--> login.php]
/favicon.ico          (Status: 200) [Size: 15406]
/files.php            (Status: 302) [Size: 4914] [--> login.php]
/footer.php           (Status: 200) [Size: 217]
/header.php           (Status: 200) [Size: 980]
/index.php            (Status: 302) [Size: 2801] [--> login.php]
/js                   (Status: 301) [Size: 309] [--> http://10.10.11.104/js/]
/login.php            (Status: 200) [Size: 2224]
/logout.php           (Status: 302) [Size: 0] [--> login.php]
/logs.php             (Status: 302) [Size: 0] [--> login.php]
/nav.php              (Status: 200) [Size: 1248]
/server-status        (Status: 403) [Size: 277]
/status.php           (Status: 302) [Size: 2968] [--> login.php]
```

Burped my way through... attempted a logon with user "test" and password "password"

![image-20210912115955583](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912115955583.png)

POST goes to /login.php, but what if we went to /accounts.php? Sent the request to Repeater (Ctrl + R), sent the request, and looked at response

![image-20210912120500252](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912120500252.png)

Looks like the response includes a PHP form with the fields of "username" and "password". If we send a request to /accounts.php, here's part of the response:

![image-20210912120629810](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912120629810.png)

So it looks like some HTML showing an admin panel or something...?  Scrolling down we can see something about making a new account:

![image-20210912120846086](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912120846086.png)

This is another form that contains the fields of "username", "password", and "confirm". Editing the request to reflect the fields on the form:

![image-20210912121228440](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912121228440.png)

Username = PandaExpress; Password = ILovePandaExpress!

Keep note that the values need to be URL encoded.After sending the request, I was able to login with the credentials:

![image-20210912144551619](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912144551619.png)

![image-20210912121352971](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912121352971.png)

Was able to download siteBackup.zip

![image-20210912144719125](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912144719125.png)

Look through the site files, found this interesting Python function:

![image-20210912144643803](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912144643803.png)

Proxied the request to get logs, sent to Repeater:

![image-20210912144924645](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912144924645.png)



Looks like I can edit the delim parameter to be whatever I want...

![image-20210912154220426](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912154220426.png)

I ran this to get a Meterpreter shell. I love Meterpreter!

![image-20210912154137928](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912154137928.png)

![image-20210912154307079](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912154307079.png)

![image-20210912154437382](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912154437382.png)

And we get user: 647f488299618bb15e7abaccb67130da

Used mysql credentials to get into mysql:

![image-20210912155401354](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912155401354.png)

Used hashcat to decrypt the hashes + the salts

m4lwhere : `ilovecody112235!`

Using SSH, I was able to login into a proper shell. Doing `sudo -l`:

![image-20210912172450881](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912172450881.png)

The contents of access_backup.sh

![image-20210912172522811](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912172522811.png)



So it runs the "date" command without any specified path. This means that the Linux machine's going to look through it's $PATH variable to find where "date" is.

I can add a new directory of my own choosing to PATH, then create a malicious `date` binary to make a reverse shell to my own machine:

![image-20210912172749733](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912172749733.png)

Then, by executing the access_backup.sh script again, I was able to get a reverse shell:

![image-20210912172329742](/home/fyr/Notes/infosec-notes/42 - hackthebox/03 - previse/01 - notes.assets/image-20210912172329742.png)



user: 647f488299618bb15e7abaccb67130da

root: 29eaa6aca6339aa6b58c69552e6d8d7e

