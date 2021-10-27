# Acceder remotamente a una base de datos a través del WS para comprobar la integridad de algún programa del dominio
#


## Abrir una conexión entre el server y la base de datos
```powershell
[void][System.Reflection.Assembly]::LoadWithPartialName("MySql.Data")
$Connection = ([MySql.Data.MySqlClient.MySqlConnection]::new("server=" + "localhost" + ";port=3306;uid=" + "usuario" + ";pwd=contraseña" + ";database="+"LosHashes"+";SslMode=none"))
$Connection.Open()
```

## Insertar en la base de datos los HASH que deben tener los programas
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
