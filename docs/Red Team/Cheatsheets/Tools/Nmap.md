https://github.com/nmap/nmap

### 🛠 Base du scan Nmap

#### Scan par défaut

```cmd
nmap [adresse_ip_ou_plage]
```

Effectue un scan simple pour découvrir les ports ouverts et les services basiques.

#### Scan rapide (-T4 pour accélérer, -F pour scan rapide)

```cmd
nmap -T4 -F [adresse_ip_ou_plage]
```

Effectue un scan rapide en ne testant que les ports les plus couramment utilisés.

#### Détecter les systèmes d'exploitation et services actifs

```cmd
nmap -A [adresse_ip_ou_plage]
```

Lance une détection approfondie des services, du système d’exploitation et des versions.

---
### 🧹 Balayage de Ports Spécifiques

#### Scan d’un seul port

```cmd
nmap -p [port] [adresse_ip]
```

Analyse uniquement le **port spécifié**.

#### Scan de plusieurs ports spécifiques

```cmd
nmap -p [port1],[port2],... [adresse_ip]
```

Permet de scanner plusieurs **ports spécifiques**.

#### Scan d’une plage de ports

```cmd
nmap -p [port_debut]-[port_fin] [adresse_ip]
```

Effectue un scan sur une **plage de ports**.

---
### 🧹 Balayages Avancés

#### Scan SYN (Half-Open Scan)

```cmd
nmap -sS [adresse_ip_ou_plage]
```

Effectue un scan **furtif SYN** pour détecter les ports ouverts sans établir de connexion complète.

#### Scan UDP (Détection des services UDP)

```cmd
nmap -sU [adresse_ip_ou_plage]
```

Analyse les services **UDP** qui ne répondent pas aux connexions TCP.

#### Scan de tous les ports TCP

```cmd
nmap -p- [adresse_ip]
```

Scanne **tous les 65 535 ports TCP**.

#### Scan sans résolution DNS

```cmd
nmap -n [adresse_ip_ou_plage]
```

Désactive la résolution DNS pour accélérer le scan.

#### Scan agressif

```cmd
nmap -A -T4 [adresse_ip_ou_plage]
```

Effectue une analyse approfondie avec détection des versions, OS et scripts actifs.

---
### 🔎 Découverte d'hôtes

#### Ping Scan (Ne scanne pas les ports)

```cmd
nmap -sn [adresse_ip_ou_plage]
```

Permet de détecter les hôtes actifs sans scanner les ports.

#### Découverte des hôtes actifs sur un réseau

```cmd
nmap -sP [plage_reseau]
```

Identifie les **hôtes vivants** sur un réseau donné.

---
### 🚒 Techniques de Contournement de Firewall

#### Fragmentation des paquets

```cmd
nmap -f [adresse_ip_ou_plage]
```

Envoie des paquets fragmentés pour contourner certains pare-feu.

#### Scan avec un port source spécifique

```cmd
nmap --source-port [port] [adresse_ip_ou_plage]
```

Simule une requête venant d’un **port spécifique**.

#### Scan avec intensité de détection de version réduite

```cmd
nmap -sV --version-intensity 0 [adresse_ip_ou_plage]
```

Diminue le niveau d'analyse des versions pour éviter la détection par des IDS/IPS.

---
### 🕵️ Scripts Nmap et Détection de Vulnérabilités

#### Utiliser des scripts Nmap (NSE)

```cmd
nmap --script=[nom_script] [adresse_ip_ou_plage]
```

Permet d’exécuter un **script NSE spécifique**.

#### Détection automatique de vulnérabilités

```cmd
nmap --script=vuln [adresse_ip_ou_plage]
```

Lance un scan basé sur **les scripts de vulnérabilités** intégrés.

---
### ⚗️ Options Diverses

#### Spécifier le débit des paquets

```cmd
nmap --max-rate [packets_per_second] [adresse_ip_ou_plage]
```

Limite le nombre de **paquets envoyés par seconde** pour éviter d’être détecté.

#### Définir un délai d’expiration

```cmd
nmap --host-timeout [temps] [adresse_ip_ou_plage]
```

Fixe un **timeout** pour éviter les scans trop longs.

#### Sauvegarder la sortie dans un fichier

```cmd
nmap -oN [nom_fichier] [adresse_ip_ou_plage]
```

Enregistre les résultats du scan dans un fichier texte.

