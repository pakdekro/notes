# Spawn Shell

## Spawn shell

Sh:

<pre class="language-bash"><code class="lang-bash"><strong>/bin/sh -i
</strong></code></pre>

perl:

```bash
perl —e 'exec "/bin/sh";'
```

ruby

```bash
ruby: exec "/bin/sh"
```

lua:

```bash
lua: os.execute('/bin/sh')
```

awk:

```bash
awk 'BEGIN {system("/bin/sh")}'
```

find:

```bash
find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;
```

find:

```bash
find . -exec /bin/sh \; -quit
```

vim:

```bash
vim -c ':!/bin/sh'
```
