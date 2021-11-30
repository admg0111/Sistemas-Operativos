# Configuración automatizada desde powershell del directorio activo de un Instituto
- Guardar en ficheros los usuarios para crearlos
- Crear las unidades organizativas correspondientes para cada clase, curso, ciclo, etc.
- Crear los usuarios con una contraseña global 
- Filtrar a los profesores para meterlos en un grupo diferente
- Añadir los usuarios a sus OU correspondientes
- Crear un grupo para cada clase 
- Añadir a los alumnos y profesores a sus grupos de clase correpondientes
- Crear una carpeta compartida para cada clase y determinar sus permisos
- Redireccionar el escritorio de los usuarios a la carpeta del servidor
- Denegar mediante GPO el uso del cmd a los alumnos de la ESO
-------------------------------------------------------------------

```powershell
### Crear los ficheros correspondientes de cada clase, con los nombres de los usuarios que deberemos crear ###
## Nombrar a los profesores de "Don" para diferenciarlos ## 
## Agilizar la creación de usuarios introduciendo el contenido legible de los ficheros en variables ##


"Don Manuel,Pedro García,Juan Cuesta,Luis Guerra,Sonia Montalvo,Alejandro Carrasco,Paco Peña,Manuel Santos,Andreu Gisbert,Jorge Casanovas" | Out-file C:\Users\Administrador\Desktop\Logs\Herramientas\UsuariosPrimero.txt
"Don Antonio,Guillermo Díaz,Miguel-Ángel Ríos,Sandra Sánchez,Nicolás Perez,Victor Salamanca,Ignacio Pérez,Adrián Medina,Clara Lagunas,Cristina Torres" | Out-File C:\Users\Administrador\Desktop\Logs\Herramientas\UsuariosSegundo.txt
"Don Miguel,Santiago Franco,Alfonso Gutierrez,Marina Carmona,Pedro-Luis Moreno,Jesús Niño,Pedro Sánchez,Angélica Guzmán,Federico García,Álvaro Riveiro" | Out-File C:\Users\Administrador\Desktop\Logs\Herramientas\UsuariosTercero.txt
"Don Sancho,Federico Martínez,Francisco Navarro,Laura Ortiz,Juan-Bautista Criado,Helena Gonzalez,Georgi Anatoliev,Senna Fredich,Viktor Sazir" | Out-File C:\Users\Administrador\Desktop\Logs\Herramientas\UsuariosCuarto.txt
"Don Luis,Sandor Martín,Diego Sierra,Omar Navarrete,Aitana Lacumza,Layla Peña,Andrés Flores" | Out-File C:\Users\Administrador\Desktop\Logs\Herramientas\Usuarios1ºBach.txt
"Don Nicolás,Pedro Torres,Ángela Medina,Héctor Salazar,Marta Pérez,Pedro Duque,Hugo León" | Out-File C:\Users\Administrador\Desktop\Logs\Herramientas\Usuarios2ºBach.txt
$UsuariosPrimero= (gc C:\Users\Administrador\Desktop\Logs\Herramientas\UsuariosPrimero.txt).Split(",")
$UsuariosSegundo= (gc C:\Users\Administrador\Desktop\Logs\Herramientas\UsuariosSegundo.txt).Split(",")
$UsuariosTercero= (gc C:\Users\Administrador\Desktop\Logs\Herramientas\UsuariosTercero.txt).Split(",")
$UsuariosCuarto = (gc C:\Users\Administrador\Desktop\Logs\Herramientas\UsuariosCuarto.txt).Split(",")
$Usuarios1ºBach = (gc C:\Users\Administrador\Desktop\Logs\Herramientas\Usuarios1ºBach.txt).Split(",")
$Usuarios2ºBach = (gc C:\Users\Administrador\Desktop\Logs\Herramientas\Usuarios2ºBach.txt).Split(",")


### Crear la Unidad Organizativa del INSTITUTO y que contenga las OU de cada curso ###
 
New-ADOrganizationalUnit -Name "INSTITUTO" -Path "dc=admg,dc=local"

New-ADOrganizationalUnit -Name "ESO" -Path "ou=INSTITUTO,dc=admg,dc=local"
New-ADOrganizationalUnit -Name "1ºESO" -Path "ou=ESO,ou=INSTITUTO,dc=admg,dc=local"
New-ADOrganizationalUnit -Name "2ºESO" -Path "ou=ESO,ou=INSTITUTO,dc=admg,dc=local"
New-ADOrganizationalUnit -Name "3ºESO" -Path "ou=ESO,ou=INSTITUTO,dc=admg,dc=local"
New-ADOrganizationalUnit -Name "4ºESO" -Path "ou=ESO,ou=INSTITUTO,dc=admg,dc=local"

New-ADOrganizationalUnit -Name "BACH" -Path "ou=INSTITUTO,dc=admg,dc=local"
New-ADOrganizationalUnit -Name "1ºBACH" -Path "ou=BACH,ou=INSTITUTO,dc=admg,dc=local"
New-ADOrganizationalUnit -Name "2ºBACH" -Path "ou=BACH,ou=INSTITUTO,dc=admg,dc=local"


### Crear una cuenta de usuario para cada uno de los nombres guardados en las variables ###
## Determinar la contraseña del usuario y que no tenga que cambiarla al iniciar sesión ##

foreach ($usuario in $UsuariosPrimero) 
    {
    $password = (ConvertTo-SecureString "AndelAndel33" -AsPlainText -Force)
    New-ADUser -Name $usuario -Sam $usuario -Path "ou=1ºESO,ou=ESO,ou=INSTITUTO,dc=admg,dc=local" -AccountPassword $password -Enabled $true -ChangePasswordAtLogon $false
    }
foreach ($usuario in $UsuariosSegundo) 
    {
    $password = (ConvertTo-SecureString "AndelAndel33" -AsPlainText -Force)
    New-ADUser -Name $usuario -Sam $usuario -Path "ou=2ºESO,ou=ESO,ou=INSTITUTO,dc=admg,dc=local" -AccountPassword $password -Enabled $true -ChangePasswordAtLogon $false
    }
foreach ($usuario in $UsuariosTercero) 
    {
    $password = (ConvertTo-SecureString "AndelAndel33" -AsPlainText -Force)
    New-ADUser -Name $usuario -Sam $usuario -Path "ou=3ºESO,ou=ESO,ou=INSTITUTO,dc=admg,dc=local" -AccountPassword $password -Enabled $true -ChangePasswordAtLogon $false
    }
foreach ($usuario in $UsuariosCuarto) 
    {
    $password = (ConvertTo-SecureString "AndelAndel33" -AsPlainText -Force)
    New-ADUser -Name $usuario -Sam $usuario -Path "ou=4ºESO,ou=ESO,ou=INSTITUTO,dc=admg,dc=local" -AccountPassword $password -Enabled $true -ChangePasswordAtLogon $false
    }
foreach ($usuario in $Usuarios1ºBach) 
    {
    $password = (ConvertTo-SecureString "AndelAndel33" -AsPlainText -Force)
    New-ADUser -Name $usuario -SamAccountName $usuario -Path "ou=1ºBACH,ou=BACH,ou=INSTITUTO,dc=admg,dc=local" -AccountPassword $password -Enabled $true -ChangePasswordAtLogon $false
    }
foreach ($usuario in $Usuarios2ºBach) 
    {
    $password = (ConvertTo-SecureString "AndelAndel33" -AsPlainText -Force)
    New-ADUser -Name $usuario -SamAccountName $usuario -Path "ou=2ºBACH,ou=BACH,ou=INSTITUTO,dc=admg,dc=local" -AccountPassword $password -Enabled $true -ChangePasswordAtLogon $false 
    }


### Filtrar a los profesores y añadirlos a un nuevo grupo de "Profesores" ###

    $TodoElMundo = (Get-ADUser -Filter * | select Name)
    $Profesores = foreach ($Profesor in $TodoElMundo)
    {
        if ($Profesor -match "Don")
            {
                $Profesor.name
            }
    }


New-ADGroup -Name Profesores -GroupScope DomainLocal

foreach ($Profe in $Profesores)
{
Set-LocalUser -name $Profe -Description Profesor 
Add-ADGroupMember  -Identity "CN=Profesores,CN=Users,dc=admg,dc=local" $Profe
}


### Crear un grupo para cada clase, con el nombre de la clase y su descripción ###

$Cursos = (Get-ADOrganizationalUnit -Filter * | select name).name -match "º"

foreach ($Clase in $Cursos)
{
 New-ADGroup -Name $Clase -GroupScope DomainLocal -Description "Grupo de alumnos de $Clase"
}


### Añadir a los alumnos y profesores, al grupo específico de su clase ###

$Grupos = (Get-ADGroup -Filter * | select name).name -match "º"

$Users = (Get-ADUser -Filter * | select name,DistinguishedName).DistinguishedName -match "º"

foreach ($Grupo in $Grupos )
{
  
    $Seleccionado = $Users -Match $Grupo
    Add-ADGroupMember  -Identity "CN=$Grupo,CN=Users,dc=admg,dc=local" $Seleccionado
    
}


### Crear una carpeta compartida para cada curso ###
## Solo podrán acceder a esta los alumnos de ese curso y los profesores ##


foreach ($Curso in $Cursos)
{
    mkdir -Name $Curso -Path \\192.168.1.100\INSTITUTO\Cursos\
}

$Carpetas = (Get-ChildItem \\192.168.1.100\INSTITUTO\Cursos\).name

foreach ($Carpeta in $Carpetas)
{
   $Permiso = $Grupos -EQ $Carpeta

   $Identidad = "ADMG\$Permiso"
   $Permisos = "Read,Write,CreateFiles,Modify"
   $Herencia = "ContainerInherit, ObjectInherit"
   $Propagación = "None"
   $Type = "Allow"
        
            $ACE = New-Object System.Security.AccessControl.FileSystemAccessRule($Identidad,$Permisos,$Herencia,$Propagación,$Type)  
            $ACL = get-acl -Path "\\192.168.1.100\INSTITUTO\Cursos\$Carpeta"
            $ACL.SetAccessRule($ACE)
            $ACL | set-acl "\\192.168.1.100\INSTITUTO\Cursos\$Carpeta"
}


### Redireccionar mediante GPO la ubicación del escritorio del usuario a la carpeta compartida del dominio ###
## Determinar los permisos ## 

New-GPLink -Name Redirección -Target "ou=INSTITUTO,dc=admg,dc=local" -Enforced Yes 

### Los alumnos de la ESO no tienen permiso para usar el cmd, por lo que vamos a denegar su acceso por GPO ###

New-GPLink -Name "NO cmd" -Target "ou=ESO,ou=INSTITUTO,dc=admg,dc=local" 
```
