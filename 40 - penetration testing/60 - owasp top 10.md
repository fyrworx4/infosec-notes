# owasp top 10

## 1. injection

user input is interpreted as actual commands

* SQL injection
* Command injection

things that attackers can do:

* access/modify/delete info in a database
* execute system commands

defending:

* using an allow list (input is checked for "safe characters")
* stripping input (dangerous characters are removed)

### OS command injection

* allows attacker to take advantage of system call to execute OS system commands

## 2. broken authentication

* user sends credentials. if they are correct, user gets a cookie
* attack methods:
  * brute force
  * weak creds
  * weak cookies
* defend:
  * strong password policy
  * automatic lockout
  * MFA

## 3. sensitive data exposure

if databases are able to be downloaded, use sqlite to view them

```sqlite
pragma table_info(customers);
select * from customers;
```

## 4. XML external identity

XXE Payloads:

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

## 5. broken access controls

visitors can access unauthorized pages

for example:

```
http://10.10.205.237/note.php?note=0
```

```
http://10.10.205.237/note.php?note=1
```

## 6. security misconfigs

default passwords...?

## 7. XSS

* Stored XSS - malicious string originates from website's database
* Reflected XSS - malicious payload is part of a victim's request
* DOM-Based XSS

Some HTML:

```html
<script>alert("Hello world!")</script>
```

Some JS:

```javascript
window.location.hostname
```

```js
document.cookie
```

```html
<script>document.querySelector('#thm-title').textContent = 'I am a hacker'</script>
```

## 8. insecure deserialization

## 9. components w/ known vulns

## 10. insufficient logging

