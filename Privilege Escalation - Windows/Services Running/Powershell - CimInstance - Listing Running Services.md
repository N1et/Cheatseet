```
Get-CimInstance -ClassName win32_service | Select Name,State,StartMode,PathName | Where-Object {$_.State -like 'Running'}

```