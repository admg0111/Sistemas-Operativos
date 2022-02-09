# Puertos, Procesos y Servicios



```powershell

### ¿El puerto 80 está funcionando con Apache? ¿No? ¿Que está sucediendo en ese puerto? 


#Identificar un proceso
$proceso= (Get-NetTCPConnection -LocalPort 80).OwningProcess 
$id= (ps -id $proceso).Id

#Identificar un servicio
$serv= Get-ciminstance -Class Win32_service 
$servids= foreach ($servid in $serv.processID){$servid}



###¿Proceso o Servicio? Liberar puertos
   if ($servids -eq $id){
   cls
   Write-Host "↓↓↓↓ Este puerto esta escuchando a este servicio ↓↓↓↓"
   Get-CimInstance Win32_Service -Filter "ProcessID = '$id'"
   


    } else {
   cls
   write-host "↓↓↓↓ Este puerto esta escuchando este proceso ↓↓↓↓"
   ps -id $proceso


 
    } 


#Detener proceso 
(ps -id $proceso).kill()

#Detener Servicio
Stop-Service (($serv | where processID -eq $id).name)

```
