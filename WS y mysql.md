# Acceder remotamente a una base de datos a través del WS para comprobar la integridad de algún programa del dominio
-Conexión remota a mysql desde Windows Server
-Inserts remoto a una base de datos
-Ejecutar scripts en equipos del dominio desde el server (Invoke-Command)
---------------------------

## Abrir una conexión entre el server y la base de datos
```powershell
[void][System.Reflection.Assembly]::LoadWithPartialName("MySql.Data")
$Connection = ([MySql.Data.MySqlClient.MySqlConnection]::new("server=" + "localhost" + ";port=3306;uid=" + "usuario" + ";pwd=contraseña" + ";database="+"LosHashes"+";SslMode=none"))
$Connection.Open()
```

## Insertar en la base de datos los HASH que deben tener los programas (Ej. HASHES del WS)
```powershell
$HashModelo = (Get-FileHash C:\Windows\system32\calc.exe).HASH
$Programa = "calc.exe"

$Query = 'INSERT INTO hash (HASH,PROGRAMA) VALUES ("'+$HashModelo+'","'+$Programa+'");'
$Command = New-Object MySql.Data.MySqlClient.MySqlCommand($Query, $Connection)
$DataAdapter = New-Object MySql.Data.MySqlClient.MySqlDataAdapter($Command)
$DataSet = New-Object System.Data.DataSet
$RecordCount = $dataAdapter.Fill($dataSet, "data")
$DataSet.Tables[0]

$Connection.Close()
```
## Comprobar que hemos insertado correctamente los hashes en la base de dates mediante una QUERY
```powershell
$Query = 'select * from LosHashes;'
$Command = [MySql.Data.MySqlClient.MySqlCommand]::new($Query, $Connection)
$DataAdapter = [MySql.Data.MySqlClient.MySqlDataAdapter]::new($Command)
$DataSet = [System.Data.DataSet]::new()
$DataAdapter.Fill($DataSet)
$DataSet.Tables
```
## Obtener los datos de integridad de los programas de un equipo del dominio y comprobar su hash con el almacenado en la base de datos
```powershell
$HashSospechoso = Invoke-Command -ScriptBlock { (Get-FileHash C:\Windows\system32\calc.exe).HASH } -ComputerName "DESKTOP-XXXXXXX"
```
```powershell
if (($DataSet.Tables).hash -eq $HashSospechoso ) 
{
    "El Hash es correcto"
} else
{
    "El Hash no es correcto"
}
```



