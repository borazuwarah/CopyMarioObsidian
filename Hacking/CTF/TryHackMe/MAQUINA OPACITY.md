Hacemos el escaneo de nmap:
![[Pasted image 20231029212628.png]]
Y esto es lo que tenemos en el puerto 80:
![[Pasted image 20231029212659.png]]
Probamos en hacer fuzzing web y nos encontramos con el directorio /cloud:
![[Pasted image 20231029212831.png]]
Volvemos a hacer fuzzing sobre este nuevo directorio, y vemos /images:
![[Pasted image 20231029214341.png]]
Y sobre /images ya no encontramos nada:
![[Pasted image 20231029214433.png]]
Vemos que la web de /cloud hace una petición a una página web en busca de una imagen, y seguramente la guarde en el directorio /images:
![[Pasted image 20231029212844.png]]
Por lo que si alojamos en un servidor http con python algún archivo, podemos probar a ver si hace alguna petición:
![[Pasted image 20231029213355.png]]
Pero vemos que no hace nada:
![[Pasted image 20231029213413.png]]
Por lo que interceptamos con burp suite:
![[Pasted image 20231029213429.png]]
Pero vemos que no recibimos la petición:
![[Pasted image 20231029213442.png]]
Pero si probamos en cambiar la extensión a jpg por ejemplo (simulando que se trate de una imagen), vemos que ahora sí hace la petición:
![[Pasted image 20231029213525.png]]
![[Pasted image 20231029213537.png]]
Una vez comprobado esto, nuestro objetivo será que la web nos permita subir una webshell en php; y para ello tenemos esta url que nos indica cómo hacerlo:
https://book.hacktricks.xyz/pentesting-web/file-upload
![[Pasted image 20231029214856.png]]
Por tanto, en burp suite vamos a probar en inyectar este bypass, utilizando .php%0a.jpg:
![[Pasted image 20231029215453.png]]
Enviamos la petición y vemos que funciona:
![[Pasted image 20231029215021.png]]Por tanto, ahora tenemos que ponernos con netcat a la escucha:
![[Pasted image 20231029215046.png]]
Y en la web vemos que se cambia la estructura y además se nos muestra la ruta donde se guardó el archivo malicioso en php:
```bash
http://10.10.141.241/cloud/images/reverse.php.jpg
```
![[Pasted image 20231029215621.png]]
Por tanto vamos a poner dicha url en el navegador:
![[Pasted image 20231029215603.png]]
Y recibimos la conexión:
![[Pasted image 20231029215633.png]]
Una vez dentro, vemos un archivo llamado dataset.kdbx:
![[Pasted image 20231029220005.png]]
Nos lo pasamos a nuestra máquina atacante para intentar crackearlo con john the ripper:
![[Pasted image 20231029220041.png]]
Y lo crackeamos con john the ripper:
![[Pasted image 20231029220158.png]]
```bash
741852963
```
Por tanto, nos abrimos el keepass y vemos las contraseñas que hay dentro:
![[Pasted image 20231029220304.png]]
```
sysadmin:Cl0udP4ss40p4city#8700
```
Entramos vía ssh y estamos dentro:
![[Pasted image 20231029220505.png]]
Pero dentro de la carpeta /scripts/lib/ nos encontramos con que no podemos editar ninguno de los archivos, pero sin embargo nuestro usuario es el propietario de la carpeta:
![[Pasted image 20231029221057.png]]
Y llama la atención el fichero backup.inc.php porque tiene un tamaño diferente:
![[Pasted image 20231029222850.png]]
Por tanto miramos como inyectar el código de pentestmonkey en este archivo y así escalar privilegios:
![[Pasted image 20231029222043.png]]
Y lo compartimos con la máquina víctima, donde le vamos a poner el mismo nombre que el script php que queremos reemplazar:
![[Pasted image 20231029222147.png]]
Y eliminamos el fichero backup.inc.php y volvemos a copiar el archivo nuevo:
```bash
rm /home/sysadmin/scripts/lib/backup.inc.php
cp backup.inc.php /home/sysadmin/scripts/lib/backup.inc.php
```
![[Pasted image 20231029221546.png]]
Y ahora si nos ponemos en la escucha con netcat, deberíamos recibir la reverse shell como root:
![[Pasted image 20231029222335.png]]
