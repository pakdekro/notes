https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1

### 🔍 Exploration du Domaine

#### Trouver des informations sur le domaine actuel

```powershell
Get-NetDomain
```

Affiche des informations sur le domaine Active Directory auquel la machine appartient.

#### Trouver des informations sur un domaine spécifique

```powershell
Get-NetDomain -Domain [nom_domaine]
```

Permet d'obtenir des détails sur un autre domaine si un **trust** existe.

#### Trouver un contrôleur de domaine

```powershell
Get-NetDomainController
```

Renvoie le **nom du contrôleur de domaine** actuel.

---

### 👥 Utilisateurs et Groupes

#### Trouver des informations sur un utilisateur

```powershell
Get-NetUser -Username [nom_utilisateur]
```

Affiche des informations détaillées sur un utilisateur spécifique, y compris **email, SID et groupes associés**.

#### Lister tous les utilisateurs du domaine

```powershell
Get-NetUser | select cn
```

Affiche uniquement les **noms communs (CN)** des utilisateurs.

#### Trouver des informations sur un groupe

```powershell
Get-NetGroup -GroupName [nom_groupe]
```

Récupère des détails sur un **groupe** AD spécifique.

#### Lister tous les groupes du domaine

```powershell
Get-NetGroup -FullData
```

Affiche la liste complète des groupes avec toutes les informations associées.

#### Trouver les membres d’un groupe

```powershell
Get-NetGroupMember -GroupName [nom_groupe]
```

Affiche les utilisateurs et machines qui font partie du **groupe**.

#### Lister les groupes auxquels appartient un utilisateur

```powershell
Get-NetGroup -Username [nom_utilisateur]
```

Affiche tous les **groupes d’appartenance** d’un utilisateur donné.

---

### 🖥️ Exploration des Ordinateurs

#### Lister tous les ordinateurs du domaine

```powershell
Get-NetComputer
```

Récupère la liste des machines du domaine.

#### Trouver des informations détaillées sur un ordinateur spécifique

```powershell
Get-NetComputer -ComputerName [nom_ordi] -FullData
```

Affiche **l'OS, le type d'ordinateur et d'autres propriétés**.

#### Trouver des ordinateurs avec un OS spécifique

```powershell
Get-NetComputer -OperatingSystem "*Server*"
```

Permet de **filtrer** les ordinateurs ayant un **OS spécifique**, par exemple, les **serveurs Windows**.

---

### 🔄 Sessions et Connexions

#### Lister les sessions actives sur un ordinateur

```powershell
Get-NetSession -ComputerName [nom_ordi]
```

Affiche les **sessions d’utilisateurs actuellement connectés**.

#### Lister les connexions ouvertes sur une machine

```powershell
Get-NetLoggedon -ComputerName [nom_ordi]
```

Renvoie la liste des **utilisateurs loggés sur la machine cible**.

#### Trouver sur quelles machines un utilisateur est connecté

```powershell
Find-LocalAdminAccess -ComputerName [nom_ordi]
```

Recherche si un utilisateur dispose de **droits d’admin local** sur d’autres machines.

---

### 🏛 Gestion des Politiques de Groupe (GPO)

#### Lister les GPO appliquées à un ordinateur

```powershell
Get-NetGPO -ComputerName [nom_ordi]
```

Affiche toutes les **GPO (Group Policy Objects)** appliquées à un ordinateur.

#### Lister les GPO définissant des droits utilisateur/administrateur

```powershell
Get-NetGPOGroup
```

Recherche les **GPO** qui **ajoutent des droits admin** à des utilisateurs ou groupes.

---

### 🔗 Relations de Confiance Active Directory

#### Lister les relations de confiance d’un domaine

```powershell
Get-NetDomainTrust
```

Affiche les **relations de confiance inter-domaines** pour identifier d’éventuelles escalades.

---

### 🎯 Divers et Recherche Active Directory

#### Rechercher des objets spécifiques dans Active Directory

```powershell
Find-ADObject -SearchBase "LDAP://OU=Users,DC=domaine,DC=com"
```

Effectue une **recherche avancée** d’objets **dans une unité organisationnelle (OU)** spécifique.

#### Trouver des objets intéressants (partages, machines vulnérables)

```powershell
Invoke-ShareFinder -CheckShareAccess
```

Recherche des **partages SMB** accessibles et liste ceux qui sont **potentiellement sensibles**.

#### Enumérer les permissions d’un objet (ACLs)

```powershell
Get-ObjectAcl -SamAccountName [nom_objet] -ResolveGUIDs
```

Affiche les **permissions** sur un **utilisateur, groupe, machine ou GPO**, utile pour la **privesc**.
