Post Office Protocol, port 110/995, TCP. Internet Message Access Protocol, port 143/993, TCP.

Possible de se connecter directement via telnet. 

#### Commandes disponibles (IMAP):

| Command                       |                                                Description                                                |
| ----------------------------- | :-------------------------------------------------------------------------------------------------------: |
| 1 LOGIN username password     |                                               User's login.                                               |
| 1 LIST "" \*                  |                                           Lists all directories.                                          |
| 1 CREATE "INBOX"              |                                  Creates a mailbox with a specified name.                                 |
| 1 DELETE "INBOX"              |                                             Deletes a mailbox.                                            |
| 1 RENAME "ToRead" "Important" |                                             Renames a mailbox.                                            |
| 1 LSUB "" \*                  | Returns a subset of names from the set of names that the User has declared as being active or subscribed. |
| 1 SELECT INBOX                |                     Selects a mailbox so that messages in the mailbox can be accessed.                    |
| 1 UNSELECT INBOX              |                                        Exits the selected mailbox.                                        |
| 1 FETCH all                   |                          Retrieves data associated with a message in the mailbox.                         |
| 1 CLOSE                       |                              Removes all messages with the Deleted flag set.                              |
| 1 LOGOUT                      |                                Closes the connection with the IMAP server.                                |

#### Commandes disponibles (POP3):

| Command       |                         Description                         |
| ------------- | :---------------------------------------------------------: |
| USER username |                     Identifies the user.                    |
| PASS password |        Authentication of the user using its password.       |
| STAT          |     Requests the number of saved emails from the server.    |
| LIST          | Requests from the server the number and size of all emails. |
| RETR id       |  Requests the server to deliver the requested email by ID.  |
| DELE id       |   Requests the server to delete the requested email by ID.  |
| CAPA          |   Requests the server to display the server capabilities.   |
| RSET          |  Requests the server to reset the transmitted information.  |
| QUIT          |         Closes the connection with the POP3 server.         |

---
### Enumeration

#### Connection via telnet:

```bash
nc -nv $ip 143
```

#### Connction POP3 via openssl:

```bash
openssl s_client -connect $ip:pop3s
```

#### Connection IMAP via openssl:

```bash
openssl s_client -connect $ip:imaps
```

#### Nmap:

```bash
nmap $ip -sV -p110,143,993,995 -sC
```

#### Lister les mailbox d'un user:

```bash
curl -k 'imaps://$ip' --user user:password
```

#### Lister les messages dans une mailbox:

```bash
curl -k 'imaps://$ip/INBOX?ALL' --user user:password
```

#### Voir le contenu d'un message (ici le premier mail dans la mailbox "Drafts"):

```bash
curl -k 'imaps://$ip/Drafts;MAILINDEX=1' --user user:pass
```

---
### Brute force

#### Brute force password pop3:

```bash
hydra -l monUser -P /usr/share/seclists/Passwords/xato-net-10-million-passwords-1000000.txt -f $ip pop3 -V
```

#### Brute force password imap:

```bash
hydra -l monUser -P /usr/share/seclists/Passwords/xato-net-10-million-passwords-1000000.txt -f $ip imap -V
```

---
### Configuration potentiellement dangereuse

| Setting                   |                                        Description                                        |
| ------------------------- | :---------------------------------------------------------------------------------------: |
| auth\_debug               |                         Enables all authentication debug logging.                         |
| auth\_debug\_passwords    |  This setting adjusts log verbosity, the submitted passwords, and the scheme gets logged. |
| auth\_verbose             |                Logs unsuccessful authentication attempts and their reasons.               |
| auth\_verbose\_passwords  |          Passwords used for authentication are logged and can also be truncated.          |
| auth\_anonymous\_username | This specifies the username to be used when logging in with the ANONYMOUS SASL mechanism. |
