# Tomcat

Enumération:

```shell-session
curl -s http://app-dev.inlanefreight.local:8080/docs/ | grep Tomcat
```

```shell-session
gobuster dir -u http://web01.inlanefreight.local:8180/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```

Bruteforce:

```shell-session
msf6 auxiliary(scanner/http/tomcat_mgr_login)
```
