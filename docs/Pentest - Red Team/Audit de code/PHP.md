### 💉 SQLi

```bash
grep -R --include=*.php "query(" .
grep -R --include=*.php "exec(" .
grep -R --include=*.php "prepare(" .
```

SQLi possible si la requête n'est pas préparée, exemple:

```sql
$sql = "SELECT * FROM users WHERE username = '$user'";
$pdo->query($sql);
```

---
### 🛰️ RCE

```bash
grep -R --include=*.php -E "system\(|exec\(|shell_exec\(|passthru\(|popen\(|proc_open\(" .
```

RCE possible si l'entrée est directement passée, exemple:

```php
system($_GET['cmd']);
```

---
### 🪂 LFI/RFI

```bash
grep -R --include=*.php -E "include\(|require\(" .
```

```php
include($_GET['page']);
```

---
### 🧁 XSS

```bash
grep -R --include=*.php -E "echo |print " .
```

```php
echo $_GET['name'];
```

(si 'htmlspecialchar' est utilisé => pas de xss possible)