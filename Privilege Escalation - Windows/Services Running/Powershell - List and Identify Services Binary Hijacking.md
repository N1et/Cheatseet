```
Get-CimInstance -ClassName win32_service |
 Where-Object {$_.State -like 'Running' -and $_.PathName} |
 ForEach-Object {
     $path = ($_).PathName -replace '"', '' -replace '\s-.*', '' # Remove aspas e argumentos
     if (Test-Path $path) {
         $permissions = icacls $path
         Write-Host "Permissions for ${path}:" -ForegroundColor Yellow
         foreach ($line in $permissions) {
             if ($line -match '\(F\)') {
                 Write-Host $line -ForegroundColor Green # Full Control em verde
             } elseif ($line -match '\(M\)') {
                 Write-Host $line -ForegroundColor Red # Modify em ciano
             } elseif ($line -match '\(W\)') {
                 Write-Host $line -ForegroundColor Red # Write em vermelho
             } else {
                 Write-Host $line -ForegroundColor Gray # Outras permissões
             }
         }
     } else {
         Write-Host "Path ${path} does not exist or is invalid." -ForegroundColor DarkRed
     }
 }

```