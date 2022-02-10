# Advertir con un mensaje cuando quede poco espacio

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

# Ver el espacio del que disponemos en un disco

```powershell
### ¿Cuanto espacio disponile tengo en un disco?

$disco = (Get-WMIObject  -Class Win32_LogicalDisk | where deviceid -eq f:)

$espacio= $disco.FreeSpace/1mb
$letra=$disco.DeviceID

write-host "Quedan $espacio mb en la unidad $letra"
```
