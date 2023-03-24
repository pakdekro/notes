# Fuzzing

Fuzzing repertoire web:

```shell-session
ffuf -w /usr/share/wordlist/seclists/Discovery/Web-Content/directory-list-2.3-big.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```

Fuzzing extensions:

```shell-session
ffuf -w /usr/share/wordlist/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ
```

Fuzzing de pages web:

```shell-session
ffuf -w /usr/share/wordlist/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```

Fuzzing récursif (ici récursivité sur un niveau, et tentes de trouver des fichiers .php):

```shell-session
ffuf -w /usr/share/wordlist/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```

Fuzzing LFI:

```shell-session
ffuf -w /usr/share/wordlist/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ' -fs 2287
```

Fuzzing paramètres:

```shell-session
ffuf -w /usr/share/wordlist/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287
```

