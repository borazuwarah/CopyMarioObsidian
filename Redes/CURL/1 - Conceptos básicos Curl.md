# SUBIR ARCHIVOS CON CURL
Por ejemplo, si deseas subir un archivo llamado `imagen.jpg` a la URL `http://ejemplo.com/subir`, ejecutarías el siguiente comando:
```javascript
curl -F "file=@/ruta/completa/a/imagen.jpg" http://ejemplo.com/subir
```
También podemos subir ficheros con curl a través del protocolo FTP de la siguiente manera:
```javascript
curl -T hola.pdf ftp://192.168.0.20/home/mario/ -u mario:123123
```
# ENVIAR CREDENCIALES CON CURL / AUTENTICARSE CON CURL
```bash
curl -X POST 192.63.12.3/login.php -d 'name=mario&password=123123' -v
```