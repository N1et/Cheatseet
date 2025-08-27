```
# Obter o nome do usuário atual
$currentUser = [System.Security.Principal.WindowsIdentity]::GetCurrent().Name

# Enumerar todas as tarefas agendadas no sistema
Get-ScheduledTask | ForEach-Object {
    $task = $_
    # Obter informações detalhadas da tarefa
    $path = $task.Actions | Where-Object { $_.Execute } | Select-Object -ExpandProperty Execute

    # Validar se o caminho não está vazio e é válido
    if (![string]::IsNullOrWhiteSpace($path) -and (Test-Path $path)) {
        # Obter as ACLs do arquivo executável
        $acl = Get-Acl $path
        $permissions = $acl.Access | Where-Object {
            # Normalizar o nome do usuário para garantir a correspondência exata
            $identity = $_.IdentityReference.Value.Trim()
            ($identity -notlike 'NT AUTHORITY\SYSTEM') -and
            ($identity -notlike 'BUILTIN\Administrators') -and
            ($identity -notlike 'NT SERVICE\TrustedInstaller') -and
            (
                ($_.FileSystemRights -match 'Write') -or
                ($_.FileSystemRights -match 'Modify') -or
                ($_.FileSystemRights -match 'FullControl')
            )
        }

        # Exibir as tarefas onde o usuário atual ou outros têm permissões específicas
        if ($permissions) {
            Write-Host "Vulnerable Scheduled Task Found:" -ForegroundColor Yellow
            Write-Host "Task Name: $($task.TaskName)" -ForegroundColor Cyan
            Write-Host "Executable Path: $path" -ForegroundColor Green
            Write-Host "Permissions:" -ForegroundColor Magenta
            $permissions | ForEach-Object {
                Write-Host "  User: $($_.IdentityReference) | Rights: $($_.FileSystemRights)" -ForegroundColor Gray
            }
            Write-Host "`n"
        }
    } else {
        # Relatar tarefas com caminho inválido ou vazio
        Write-Host "Invalid or Nonexistent Path for Task: $($task.TaskName)" -ForegroundColor DarkRed
    }
}

```