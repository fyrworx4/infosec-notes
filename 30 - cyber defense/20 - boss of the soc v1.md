# boss of the soc v1

link: https://samsclass.info/50/proj/botsv1.htm

## level 1

**Vulnerability scanner is "Acunetix".**

```
index="botsv1" sourcetype="stream:http" imreallynotbatman.com
```

Scroll down a little to see the Directory Path Traversal attack:

![image-20211003173805909](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003173805909.png)

---

**Attacker's IP address is 40.80.148.42.**

```
index="botsv1" sourcetype="stream:http" imreallynotbatman.com
```

In the same event:

![image-20211003173919905](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003173919905.png)

IP address running the scan is the attacker's IP address.

---

**The webserver's IP address is 192.168.250.70.**

```
index="botsv1" sourcetype="stream:http" imreallynotbatman.com
```

In the same event:

![image-20211003174041414](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003174041414.png)

Destination IP is the webserver's IP.

---

**Defacement file is called "poisonivy-is-coming-for-you-batman.jpeg".**

```
index="botsv1" sourcetype="stream:http" src_ip="192.168.250.70"
```

Turn off event sampling, should see about 8 events. Scroll down until you see an interesting event:

![image-20211003174238240](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003174238240.png)

Alternatively, look at the field details for "request":

![image-20211003174259241](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003174259241.png)

Similarly with the "uri" values:

![image-20211003174328210](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003174328210.png)

---

**The FQDN of the staging server is prankglassinebracket.jumpingcrab.com.**

In the same event as the last question, the "site" field shows the domain name of the destination IP of this HTTP request.

![image-20211003174427050](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003174427050.png)

---

## level 2

**The IP address of the staging server is 23.22.63.114.**

```
index="botsv1" sourcetype="stream:http" prankglassinebracket.jumpingcrab.com http_method="GET"
```

This is the same event as before. The destination IP is the staging server.

---

**The leetspeak domain is PO1S0N1VY.COM.**

Do a google search on "what is 23.22.63.114" and go to this site: https://www.threatcrowd.org/ip.php?ip=23.22.63.114

![image-20211003174756312](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003174756312.png)

---

**The IP of the brute-force attack is 23.22.63.114.**

```
index="botsv1" sourcetype="stream:http" http_method="POST"
```

```
index="botsv1" sourcetype="stream:http" http_method="POST" src_headers NOT "*Acunetix*"
```

```
index="botsv1" sourcetype="stream:http" http_method="POST" src_headers NOT "*Acunetix*" 
| table _time, form_data, src_ip
```

then sort src_ip field ascending:

![image-20211003175104023](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003175104023.png)

The form_data field contains username and passwd fields of all sorts of different combinations, coming from 23.22.63.144. This must be the IP of the brute-force attack.

---

**The executable file is 3971.exe.**

```
index="botsv1" sourcetype="stream:http" http_method="POST" src_headers NOT "*Acunetix*" ".exe"
```

Shows a POST request with some data:

![image-20211003175244699](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003175244699.png)

I did a CTRL+F and typed in .exe and it shows me this:

![image-20211003175313274](C:\Users\fyr\AppData\Roaming\Typora\typora-user-images\image-20211003175313274.png)

This is the only occurrence of .exe so this must be the file.