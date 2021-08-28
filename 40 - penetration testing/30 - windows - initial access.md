# windows - initial access

## checklist

* Vulnerable, external facing services?
* Any credentials leaked somewhere?
* etc.

## evil-winrm

```bash
sudo gem install evil-winrm
```

```bash
evil-winrm -u USERNAME -p PASSWORD -i TARGET_IP
```

```
evil-winrm -u USERNAME -H HASH -i TARGET_IP
```

