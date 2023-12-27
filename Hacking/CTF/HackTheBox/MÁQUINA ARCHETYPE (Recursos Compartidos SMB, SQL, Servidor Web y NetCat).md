Y ya estamos dentro:Lo primero como siempre será ejecutar nmap para ver los servicios y puertos de la máquina, con estas opciones veremos los servicios y versiones de lo que corre en los puertos:
![[Pasted image 20230702211959.png]]
Y esto son los resultados, donde tenemos estos servicios y estos puertos abiertos:
![[Pasted image 20230702212022.png]]
Aunque primero vamos a acceder a través del protocolo samba al puerto 445 que será lo que nos permita acceder a directorios y ficheros:
![[Pasted image 20230702212031.png]]
Vamos a probar a ver si podemos conectarnos a través de SMB sin credenciales de esta manera, donde en la opción -N comprueba si podemos entrar sin credenciales y -L lista el contenido de los archivos:
![[Pasted image 20230702212043.png]]
![[Pasted image 20230702212047.png]]
Ahora vamos a probar en entrar dentro del directorio backups a través de una línea de comandos:
![[Pasted image 20230702212056.png]]
Y podemos ejecutar comandos, por ejemplo listando archivos:
![[Pasted image 20230702212107.png]]
Vamos a descargar ese archivo con el comando get:
![[Pasted image 20230702212115.png]]
Y este es el contenido del fichero que hemos descargado, donde tenemos un usuario y contraseña:
![[Pasted image 20230702212123.png]]
Ahora que ya tenemos esta información, vamos a tratar de conectarnos a la base de datos a través de mssqlclient, sería este comando:
![[Pasted image 20230702212131.png]]
Y aquí tenemos las instrucciones:
![[Pasted image 20230702212314.png]]
Entonces ahora vamos a utilizar el impacket-mssqlclient para conectarnos a la base de datos utilizando las credenciales del fichero que nos hemos descargado antes, utilizando este comando:
![[Pasted image 20230702212323.png]]
Y ya estamos dentro:
![[Pasted image 20230702212338.png]]
Ahora que estamos dentro de la base de datos, podemos activar la línea de comandos y así tenemos acceso al sistema operativo con el comando xp_cmdshell, por tanto vamos a habilitarlo con este comando:
![[Pasted image 20230702212346.png]]
Ahora ya puedo ejecutar comandos con normalidad, por ejemplo un whoami:
![[Pasted image 20230702212353.png]]
