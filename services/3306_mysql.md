---
description: Base de données SQL, port 3306, TCP
---

# 3306\_MySQL

| Command                                                                                                                                                                                |                                            Description                                            |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-----------------------------------------------------------------------------------------------: |
| mysql -u $user -p$password $ip                                                                                                                                                         | Connect to the MySQL server. There should not be a space between the '-p' flag, and the password. |
| show databases;                                                                                                                                                                        |                                        Show all databases.                                        |
| use $database;                                                                                                                                                                         |                               Select one of the existing databases.                               |
| show tables;                                                                                                                                                                           |                        Show all available tables in the selected database.                        |
| show columns from $table;                                                                                                                                                              |                             Show all columns in the selected database.                            |
| select \* from $table;                                                                                                                                                                 |                               Show everything in the desired table.                               |
| select \* from $table where $columns = "$string";                                                                                                                                      |                           Search for needed string in the desired table.                          |
| # Enumeration                                                                                                                                                                          |                                                                                                   |
| \`\`\`bash                                                                                                                                                                             |                                                                                                   |
| nmap -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 $ip |                                                                                                   |
| \`\`\`                                                                                                                                                                                 |                                                                                                   |

## Brute force

```bash
hydra -l monUser -P /usr/share/seclists/Passwords/xato-net-10-million-passwords-1000000.txt $ip mysql
```

## Attaque

## Metasploit

```
auxiliary/scanner/mysql/mysql_version
auxiliary/scanner/mysql/mysql_authbypass_hashdump
auxiliary/scanner/mysql/mysql_hashdump 
auxiliary/admin/mysql/mysql_enum 
auxiliary/scanner/mysql/mysql_schemadump  
exploit/windows/mysql/mysql_start_up 
```

Brute force user/password:

```
auxiliary/scanner/mysql/mysql_login
```

## Configuration potentiellement dangereuse

| Settings           |                                                  Description                                                 |
| ------------------ | :----------------------------------------------------------------------------------------------------------: |
| user               |                                Sets which user the MySQL service will run as.                                |
| password           |                                     Sets the password for the MySQL user.                                    |
| admin\_address     |       The IP address on which to listen for TCP/IP connections on the administrative network interface.      |
| debug              |                            This variable indicates the current debugging settings                            |
| sql\_warnings      | This variable controls whether single-row INSERT statements produce an information string if warnings occur. |
| secure\_file\_priv |                This variable is used to limit the effect of data import and export operations.               |
