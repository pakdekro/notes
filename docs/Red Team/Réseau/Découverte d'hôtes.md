#### Ping Sweep Linux

```bash
for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) ;done
```

#### Ping Sweep Windows

```powershell
for /L %i in (1,1,255) do @ping -n 1 -w 200 192.168.1.%i > nul && echo 192.168.1.%i is up.
```

#### Nmap

```bash
nmap -sn 192.168.1.0/24
```

