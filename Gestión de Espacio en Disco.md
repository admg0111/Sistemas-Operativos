# Advertir con un mensaje de cuando quede poco espacio

```powershell
### Advertencia Poco Espacio

Get-WMIObject  -Class Win32_LogicalDisk | where deviceid -eq f: | %{
    if($_.FreeSpace/1MB -lt 20)
    {
        mensaje "Cuidado con el tamaño del disco"
    }
}

function mensaje($mensajito)
{
    $balloon = New-Object System.Windows.Forms.NotifyIcon
    #Configurar notificación
    #Icono
    $balloon.Icon  = [System.Drawing.Icon]::ExtractAssociatedIcon((Get-Process -Name notepad).Path) 
    $balloon.BalloonTipIcon  = [string]$Icon = 'Info'
    #Mensaje
    $balloon.BalloonTipText  = "Cuidado"
    #Título
    $balloon.BalloonTipTitle  = $mensajito
 
    $balloon.Visible  = $true
    $balloon.ShowBalloonTip(5000)
}
```
