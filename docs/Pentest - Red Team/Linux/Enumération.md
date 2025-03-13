### 🔎 Situational awareness

#### Lister les processus actifs:

```bash
ps aux
```

#### Lister les utilisateurs présents:

```bash
ls /home
cat /etc/passwd
```

#### Lister les fichiers crontab:

```bash
ls -laR /etc/cron*
crontab -l
```

#### Informations sur la distribution et le kernel:

```bash
uname -a
cat /etc/lsb-release
```

#### Vérifier les connexions réseau:

```bash
netstat -tunlp
```

---
### 🏆 Quickwin

#### Lister les privilèges sudo:

```bash
sudo -l
```

#### Lister les fichiers avec un Setuid:

```bash
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```

#### Lister les fichiers avec un Setgid:

```bash
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
```

#### Lister les capabilities:

```bash
getcap -r / 2>/dev/null
```

---
### 🕵️ Recherches

#### Chercher des fichiers contenant "config":

```bash
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null`
```

#### Rechercher des fichiers modifiés récemment:

```bash
find / -mtime -5`
```

#### Rechercher des mots de passe en Clair:

```bash
grep -Ri pass /etc 2>/dev/null
```

