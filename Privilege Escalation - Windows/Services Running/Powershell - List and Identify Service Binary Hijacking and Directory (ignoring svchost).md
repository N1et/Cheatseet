```
Get-CimInstance -ClassName win32_service |
Where-Object {$_.State -like 'Running' -and $_.PathName -notmatch 'svchost.exe'} | # Ignora svchost.exe
ForEach-Object {
    $path = ($_).PathName -replace '"', '' -replace '\s-.*', '' # Remove aspas e argumentos
    $directory = Split-Path $path -Parent # Obter o diretório do binário
    if (Test-Path $path) {
        Write-Host "----------------------------------------" -ForegroundColor Cyan
        Write-Host "Permissions for Binary: ${path}" -ForegroundColor Yellow

        # Verificar permissões no binário
        $permissions = icacls $path
        foreach ($line in $permissions) {
            if ($line -match '\(F\)') {
                Write-Host $line -ForegroundColor Green # Full Control em verde
            } elseif ($line -match '\(M\)') {
                Write-Host $line -ForegroundColor Red # Modify em vermelho
            } elseif ($line -match '\(W\)') {
                Write-Host $line -ForegroundColor Red # Write em vermelho
            } else {
                Write-Host $line -ForegroundColor Gray # Outras permissões
            }
        }

        # Verificar permissões no diretório do binário
        if (Test-Path $directory) {
            Write-Host "Permissions for Directory: ${directory}" -ForegroundColor Yellow
            $dirPermissions = icacls $directory
            foreach ($line in $dirPermissions) {
                if ($line -match '\(F\)') {
                    Write-Host $line -ForegroundColor Green # Full Control em verde
                } elseif ($line -match '\(M\)') {
                    Write-Host $line -ForegroundColor Red # Modify em vermelho
                } elseif ($line -match '\(W\)') {
                    Write-Host $line -ForegroundColor Red # Write em vermelho
                } else {
                    Write-Host $line -ForegroundColor Gray # Outras permissões
                }
            }
        } else {
            Write-Host "Directory ${directory} does not exist or is invalid." -ForegroundColor DarkRed
        }
    } else {
        Write-Host "----------------------------------------" -ForegroundColor Yellow
        Write-Host "Path ${path} does not exist or is invalid." -ForegroundColor DarkRed
    }
}

```