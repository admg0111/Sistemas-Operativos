```powershell

### Comprimir varios archivos a la vez


foreach ($archivo in Get-ChildItem "C:\ParaComprimir"){

$rutasnc=$archivo.FullName
$nombres=$archivo.name
$nombre=$nombres.split(".")[0]
Compress-Archive -LiteralPath $rutasnc -CompressionLevel Optimal -DestinationPath "C:\Comprimidas\$nombre.zip"

}
```
