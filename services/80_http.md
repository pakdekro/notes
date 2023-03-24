# 80\_HTTP

## Enumeration

Informations sur le serveur utilisé, les technologies (php, js...), le CMS...:

```bash
whatweb -a3 https://$domain -v
```

Récupérer le HEADER HTTP:

```bash
curl -I "http://$domain"
```

Savoir si il y a un firewall applicatif web (WAF):

```bash
wafw00f -v https://www.tesla.com
```

## Brute force

## Attaque

## Metasploit

## Configuration potentiellement dangereuse
