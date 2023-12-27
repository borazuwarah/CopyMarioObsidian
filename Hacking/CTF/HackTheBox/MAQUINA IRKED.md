## (Vulnerabilidad UNrealIRCD con Metasploit, extraer información de una imagen con steghide, escalada privilegios comprobando permisos SUID y ejecutar binario que ejecuta comandos como root)


Hacemos los escaneos de siempre con nmap y vemos algo de ircunreal, la cual es es una vulnerabilidad que podemos explotar con metasploit:
![[Pasted image 20230306210833.png]]
Vemos algo de unrealIRCd, por lo que abrimos metasploit y buscamos por unrealirc y vemos que existe un exploit:
![[Pasted image 20230306210843.png]]
Ahora cargamos el payload, el cual usaré el 5, el que pone reverse:
![[Pasted image 20230306210853.png]]
![[Pasted image 20230306210857.png]]
También tenemos que seleccionar el puerto de la máquina remota donde está corriendo el unrealircd:
![[Pasted image 20230306212229.png]]
Una vez con todo seleccionado, vamos a ejecutar el comando run para lanzar el exploit y ya vemos que tenemos acceso remoto:
![[Pasted image 20230306210912.png]]
Y podemos mirar dentro del directorio de djmardov:
![[Pasted image 20230306212509.png]]
Y vemos que dentro del directorio documents hay un directorio oculto que se llama .backup:
![[Pasted image 20230306212618.png]]
Y si miramos dentro nos encontramos con una contraseña:
![[Pasted image 20230306212652.png]]
En cuanto a la flag, vemos que no tenemos permisos para leerla:
![[Pasted image 20230306210922.png]]
Para seguir investigando, vamos a ir al servidor web y copiar la imagen a nuestro ordenador para inspeccionarla:
![[Pasted image 20230306210934.png]]
Una vez la imagen extraida, hay un comando que sirve para extraer información de una imagen, el cual es con la herramienta steghide:[[Esteganografía con Steghide, Foremost y BinWalk]]
![[Pasted image 20230306210944.png]]
Ahora nos lo extrae todo en un documento pass.txt, el cual contiene una contraseña que podemos utilizarla para conectarnos por ssh:
![[Pasted image 20230306211005.png]]
Nos conectamos con el usuario djmardov, ya que es el usuario que vimos que existía cuando nos conectamos con metasploit:
![[Pasted image 20230306211016.png]]
Y ya tenemos la flag:
![[Pasted image 20230306211023.png]]
Una vez dentro, ejecutamos el comando find / -perm -4000 2>/dev/null para comprobar donde tenemos permisos SUID (Se tratan de permisos que permiten a un usuario ejecutar un archivo con los privilegios del propietario del archivo), por tanto ejecutamos este comando:
```bash
find / -perm -u=s -type f 2>/dev/null
```
![[Pasted image 20230306213232.png]]
El secreto aquí es buscar binarios de nombre extraño o binarios que generalmente no tengan el setuid activado; y salta a la vista el binario viewuser, por lo que lo vamos a inspeccionar:
![[Pasted image 20230306213415.png]]
Vamos a entrar dentro y nos encontramos con un error donde nos dice que no encuentra la carpeta /tmp/listusers, además de poder intuir que esta herramienta ejecuta el comando que se encuentre dentro del fichero testusers, por lo que podemos usarlo para ver la flag de root:
![[Pasted image 20230306213525.png]]
Por tanto, para que se pueda ejecutar correctamente, vamos a probar en crear ese mismo directorio para que desaparezca este error:
![[Pasted image 20230306213718.png]]
Proporcionamos los permisos para poder ejecutar y hacer de todo con ese archivo:
![[Pasted image 20230306213829.png]]
Y lo ejecutamos y se nos ejecuta el comando que está dentro del fichero listusers; y ya tenemos acceso a la flag de root:
![[Pasted image 20230306213911.png]]
No obstante, si queremos convertirnos en usuario root directamente, podemos ejecutar el comando su -, y podemos hacerlo dentro del fichero listusers ya que tiene permisos 777:
![[Pasted image 20230306214235.png]]
Lo ejecutamos y ya nos convertimos en usuario root:
![[Pasted image 20230306214257.png]]