# Énumération

## Énumération

Lister les processus actifs:

```bash
ps aux
```

Lister les privilèges sudo:

```bash
sudo -l
```

Lister les utilisateurs présents:

```bash
ls /home
cat /etc/passwd
```

Lister les fichiers crontab:

```bash
ls -R /etc/cron*
```

Informations sur la distribution/kernel:

```bash
uname -a
cat /etc/lsb-release
```

Lister les fichiers avec un setuid:

```bash
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```

Lister les fichiers avec un setgid:

```bash
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
```

Chercher tout les fichiers contenant le terme "config" dans leur nom:

```bash
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null
```
