# Commandes

## MySQL

Créer une base de données:

```sql
CREATE DATABASE nomDB;
```

Lister les databases:

```sql
SHOW DATABASES;
```

Se déplacer dans un database:

```sql
USE nomDB;
```

Créer une table dans une database:

```sql
CREATE TABLE logins (
    id INT,
    username VARCHAR(100),
    password VARCHAR(100),
    date_of_joining DATETIME
    );
```

Lister les tables d'une database:

```sql
SHOW TABLES;
```

Connaitre la structure d'une table:

```sql
DESCRIBE $nomTable;
```

Créer un nouvel enregistrement dans une table:

```sql
INSERT INTO table_name VALUES (column1_value, column2_value, column3_value, ...);
```

Créer un nouvel enregistrement dans une table, mais uniquement dans certaine colonnes (ce qui laissera la valeur des autres colonnes par défaut):

```sql
INSERT INTO table_name(column2, column3, ...) VALUES (column2_value, column3_value, ...);
```

Créer plusieurs enregistrements dans une table, uniquement dans certaines colonnes:

```sql
INSERT INTO logins(username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!');
```

Lister tout les enregistrements d'une table:

```sql
SELECT * FROM $nomTable;
```

Lister tout les enregistrements d'une table, mais montrer uniquement certaines colonnes:

```sql
SELECT column1, column2 FROM table_name;
```

Insérer une nouvelle colonne (ici de type INT):

```sql
ALTER TABLE $nomTable ADD $nomColonne INT;
```

Renommer une colonne:

```sql
ALTER TABLE $nomTable RENAME COLUMN $nouveauNom TO $ancienNom;
```

Modifier le type d'une colonne (ici convertie en DATE):

```sql
ALTER TABLE $nomTable MODIFY $nomColonne DATE;
```

Effacer une colonne:

```sql
ALTER TABLE $nomTable DROP $nomColonne;
```

Changer la valeur de l'enregistrement d'une colonne. Ici on modifie la valeur "password" de tout les enregistrement de la table dont l'id est supérieur à 1:

```sql
UPDATE logins SET password = 'change_password' WHERE id > 1;
```
