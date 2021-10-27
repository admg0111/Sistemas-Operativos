# Acceder remotamente a una base de datos a través del WS para comprobar la integridad de un programa del dominio

## Abrir una conexión entre el server y la base de datos
```powershell
[void][System.Reflection.Assembly]::LoadWithPartialName("MySql.Data")
$Connection = ([MySql.Data.MySqlClient.MySqlConnection]::new("server=" + "localhost" + ";port=3306;uid=" + "usuario" + ";pwd=contraseña" + ";database="+"LosHashes"+";SslMode=none"))
$Connection.Open()
```
