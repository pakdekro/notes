#### Identifier le format de l'image mémoire :

```bash
vol.py -f [fichier_image_mémoire] windows.info
```

#### Lister les plugins disponibles :

```bash
vol.py -f [fichier_image_mémoire] --plugins=[chemin_plugins] windows.info
```

### Analyse des Processus

#### Lister les processus en cours d'exécution :

```bash
vol.py -f [fichier_image_mémoire] windows.pslist
```

#### Voir les processus avec leur arborescence :

```bash
vol.py -f [fichier_image_mémoire] windows.pstree
```

#### Vérifier les processus cachés ou non répertoriés :

```bash
vol.py -f [fichier_image_mémoire] windows.psxview
```

### Analyse de la Mémoire

#### Extraire le contenu de la mémoire d'un processus :

```bash
vol.py -f [fichier_image_mémoire] windows.memdump --pid [pid_processus] --dump-dir=[dossier_destination]
```

#### Analyser la mémoire virtuelle d'un processus :

```bash
vol.py -f [fichier_image_mémoire] windows.vadinfo --pid [pid_processus]
```

### Analyse de Fichiers et DLL

#### Lister les DLL chargées pour chaque processus :

```bash
vol.py -f [fichier_image_mémoire] windows.dlllist
```

#### Extraire les fichiers exécutables/DLL mappés en mémoire :

```bash
vol.py -f [fichier_image_mémoire] windows.moddump --pid [pid_processus] --dump-dir=[dossier_destination]
```

### Analyse de Réseau

#### Voir les connexions réseau actives et en écoute :

```bash
vol.py -f [fichier_image_mémoire] windows.netscan
```

#### Lister les sockets et connexions :

```bash
vol.py -f [fichier_image_mémoire] windows.netstat
```

### Analyse du Registre

#### Extraire les hives du registre :

```bash
vol.py -f [fichier_image_mémoire] windows.registry.hivelist
```

#### Analyser les clés de registre spécifiques :

```bash
vol.py -f [fichier_image_mémoire] windows.registry.printkey --key="Chemin_de_la_clé"
```

### Analyse Avancée

#### Détecter des rootkits ou des anomalies dans le noyau :

```bash
vol.py -f [fichier_image_mémoire] windows.malfind
```

#### Analyser les crash dumps :

```bash
vol.py -f [fichier_image_mémoire] windows.crashinfo
```

### Utilitaires et Divers

#### Extraire les mots de passe et les hashes :

```bash
vol.py -f [fichier_image_mémoire] windows.hashdump
```

#### Rechercher des chaînes ou des motifs dans la mémoire :

```bash
vol.py -f [fichier_image_mémoire] windows.strings -s [chaîne_à_rechercher]
```