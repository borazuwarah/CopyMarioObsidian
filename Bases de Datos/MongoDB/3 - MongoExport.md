MongoExport sirve para ejecutar instrucciones de mongo desde fuera de la mongosh, de tal forma que podremos extraer el output de las consultas a un fichero externo. Por tanto lo bajamos desde esta web:
![[Pasted image 20230607123639.png]]
Y pasamos todos los archivos a la carpeta de bin de la mongosh:
![[Pasted image 20230607123755.png]]
Y ahora abrimos aquí una ventana de powershell:
![[Pasted image 20230607123916.png]]
Una vez dentro ya podemos ejecutar la instrucción, por ejemplo esta:
```sql
./mongoexport --uri "mongodb://localhost:27017/canal" --collection suscriptores --out archivoDeSalida.json
```
![[Pasted image 20230607124741.png]]
Y ya lo tenemos exportado:
![[Pasted image 20230607124756.png]]