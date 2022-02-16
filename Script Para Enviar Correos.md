```powershell
$Destinatario = "ElDestinatario@gmail.com"
$Emisor = "TuCorreo@gmail.com"
$Asunto = "Asunto para el Correo"
$CuerpoEnHTML = "Hola <b>AMIGO</b>. ¿No estarás haciendo el mal? ñ? ☺?"
$SMTPServidor = "smtp.gmail.com"
$RutaNombreFicheroAdjunto = "C:\Ruta\Del\Fichero\Adjunto.txt"
$CodificacionCaracteres=[System.Text.Encoding]::UTF8
$creDenciAles= Get-Credential

try
{
  #Definir los objetos del correo
  $SMTPMensaje = New-Object System.Net.Mail.MailMessage($Emisor, $Destinatario, $Asunto, $CuerpoEnHTML)
  $SMTPAdjunto = New-Object System.Net.Mail.Attachment($RutaNombreFicheroAdjunto)
  $SMTPMensaje.Attachments.Add($SMTPAdjunto)
  
  #Entender el body del correo como HTML
  $SMTPMensaje.IsBodyHtml = $true
  
  #Codificar en UTF8 el asunto y el body del correo para que entienda cacracteres "Ñ", tildes, etc.
  $SMTPMensaje.BodyEncoding = $CodificacionCaracteres
  $SMTPMensaje.SubjectEncoding = $CodificacionCaracteres
  
  #Configuracion del cliente (emisor) del servidor de correo (gmail en este caso)
  $SMTPCliente = New-Object Net.Mail.SmtpClient($SMTPServidor, 587)
  $SMTPCliente.EnableSsl = $true
  $SMTPCliente.Credentials = New-Object System.Net.NetworkCredential($creDenciAles.UserName, $creDenciAles.Password);
  $SMTPCliente.Send($SMTPMensaje)
  
  #Confirmación de envío
  Write-Output "Mensaje enviado correctamente"
  #a0d1m0g1
}  
catch
{
  #Captar error
  Write-Error -Message "Error al enviar correo electrónico"
}
```
