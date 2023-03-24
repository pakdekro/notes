# Joomla

Enumération:

```shell-session
sudo pip3 install droopescan
```

```shell-session
droopescan scan joomla --url http://dev.inlanefreight.local/
```

Bruteforce:

{% embed url="https://github.com/ajnik/joomla-bruteforce" %}

```shell-session
python3 joomla-brute.py -u http://$url -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```
