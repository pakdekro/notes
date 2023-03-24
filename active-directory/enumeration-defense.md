# Enumération Defense

Voir le statut de Windows Defender:

```powershell
Get-MpComputerStatus
```

Lister les règles AppLocker:

```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

Savoir si powershell est en "Constrained Language Mode" ou en "Full language Mode". Le mode "constrained" va empêcher l'utilisation de certaines features de powershell.

```powershell
$ExecutionContext.SessionState.LanguageMode
```
