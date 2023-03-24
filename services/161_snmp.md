---
description: >-
  Simple Network Management Protocol, port 161, UDP Monitoring des équipements
  réseaux, peut aussi être utilisé pour faire de la configuration à distance. La
  v2 n'utilise pas d'authentification ni de ch
---

# 161\_SNMP

## Enumeration

SnmpWalk:

```bash
snmpwalk -v2c -c $CommunityString $ip
```

## Brute force

Brute force de la community string:

```bash
onesixtyone -c /opt/useful/SecLists/Discovery/SNMP/snmp.txt $ip
```

## Attaque

## Metasploit

## Configuration potentiellement dangereuse

| Settings      |                                      Description                                      |
| ------------- | :-----------------------------------------------------------------------------------: |
| rwuser noauth |              Provides access to the full OID tree without authentication.             |
| rwcommunity   | Provides access to the full OID tree regardless of where the requests were sent from. |
| rwcommunity6  |           Same access as with rwcommunity with the difference of using IPv6.          |
