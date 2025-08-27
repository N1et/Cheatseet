```
Get-WmiObject win32_service | select Name,PathName,StartMode,StartName | where {$_.StartMode -ne "Disabled" -and $_.StartName -eq "LocalSystem" -and $_.PathName -notmatch "`"" -and $_.PathName -notmatch "C:\\Windows"} | Format-List
```