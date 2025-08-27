```
Start-Job -ScriptBlock { powershell -ExecutionPolicy Bypass -File "C:\Path\To\winPEAS.ps1" > C:\Path\To\output.txt 2>&1 }
```