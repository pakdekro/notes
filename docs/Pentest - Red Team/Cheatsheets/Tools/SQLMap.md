https://github.com/sqlmapproject/sqlmap

#### 🔍 Commande de base (GET)

```bash
sqlmap -u http://domain.com/truc?oups=1
```

Effectue un **test d’injection SQL** sur un **paramètre GET**.

#### 📡 Commande de base (POST)

```bash
sqlmap -u http://domain.com/truc --data "user=oups&pass=oulah"
```

Exécute un **test d’injection SQL** sur une **requête POST**.

#### 📜 Scanner une requête depuis Burp Suite

```bash
sqlmap -r req.txt
```

Lit la requête depuis un **fichier enregistré avec Burp Suite** et teste l’injection SQL.

#### ⚠️ Augmenter le niveau de risque et le nombre de tests

```bash
sqlmap -r req.txt --level 5 --risk 3
```

> [!Astuce]
> - `--level 5` → **Effectue plus de tests** (augmente la couverture)
> - `--risk 3` → **Augmente l'agressivité** (potentiellement plus destructeur)

#### 📌 Définir un suffixe (utile pour contourner les WAFs)

```bash
sqlmap -r req.txt --suffix="-- "
```

Ajoute un **suffixe** à la charge utile (ex: `--` pour commenter la requête SQL).

#### 📌 Définir un préfixe (utile pour échapper un filtre)

```bash
sqlmap -r req.txt --prefix="') "
```

Ajoute un **préfixe** à la charge utile (ex: `')` pour briser une requête existante).

#### 🍪 Définir un cookie spécifique

```bash
sqlmap --cookie="lecookie"
```

Utilise un **cookie d’authentification** spécifique pour le test.

#### 🗑️ Réinitialiser la session SQLMap

```bash
sqlmap --flush-session
```

Supprime les résultats stockés de précédentes sessions SQLMap.
