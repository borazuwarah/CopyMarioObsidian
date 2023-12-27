## (Vulnerabilidad voting system, php malicioso, escalada privilegios winpeas y vulnerabilidad AlwaysInstallElevated con fichero .msi malicioso de msfvenom)

Empezaremos con los reconocimientos de siempre de nmap:
![[Pasted image 20230410102341.png]]
Como tiene el puerto 80 abierto lo analizamos:
![[Pasted image 20230410102349.png]]
Como hay un servidor web, pegamos la IP en el navegador:
![[Pasted image 20230410102357.png]]
Vamos a hacer un fuzzing y nos encuentra varias rutas, aunque la ruta de admin es interesante porque si la pegamos ya no nos pide un voter ID, sino un nombre de usuario:
![[Pasted image 20230410102405.png]]
![[Pasted image 20230410102408.png]]
Podemos buscar en searchsploit vulnerabilidades de voting system; y encontramos algunos, donde probaremos el siguiente:
![[Pasted image 20230410102416.png]]
Y aquí vemos las instrucciones de como se debe tramitar la petición por POST para hacer un bypass del login:
![[Pasted image 20230410102423.png]]
Por tanto vamos a abrir burp suite y vamos a enviar esta data:
![[Pasted image 20230410102430.png]]
Y al enviar esta petición vemos que ha funcionado correctamente:
![[Pasted image 20230410102439.png]]
Cuando estamos dentro de algún tipo de plataforma online, debemos mirar a ver si podemos subir algún archivo malicioso para ejecutar comandos de manera remota; y vemos que lo encontramos:
![[Pasted image 20230410102447.png]]
Por tanto vamos a crear un fichero php malicioso que sea una cmd que ejecute comandos en la máquina; y lo vamos a subir:
![[Pasted image 20230410102455.png]]
Ahora si vamos dentro de esta ruta vemos que existe el fichero php malicioso que subimos:
![[Pasted image 20230410102502.png]]
![[Pasted image 20230410102505.png]]
![[Pasted image 20230410102508.png]]
Vamos a descargar el ejecutable de netcat para windows y así podemos enviarnos una reverse shell, para ello compartimos netcat a través de un servidor Python y después lo descargamos en la máquina windows con un curl:
![[Pasted image 20230410102516.png]]
Ahora en la máquina víctima urlencodeamos con burpsuite el curl para descargarlo:
![[Pasted image 20230410102523.png]]
Lo pegamos en el navegador y ya tendremos el archivo descargado en la máquina víctima, donde después simplemente ejecutamos este otro comando para mandarnos la reverse shell utilizando este fichero de netcat:
![[Pasted image 20230410102530.png]]
Y si nos ponemos a escuchar con netcat y pegamos esto en el navegador, ya habremos obtenido la reverse shell:
![[Pasted image 20230410102537.png]]
![[Pasted image 20230410102541.png]]
Y navegando por los directorios obtenemos la flag de user:
![[Pasted image 20230410102548.png]]
Ahora para escalar los privilegios vamos a utilizar una herramienta llamada winpeas, que nos reportará vulnerabilidades de la máquina, para ello primero nos lo bajamos en la máquina Kali con un wget:
![[Pasted image 20230410102555.png]]
Montamos un servidor web con Python alojando este archivo y lo descargamos de esta manera en la máquina víctima:
![[Pasted image 20230410102605.png]]
Ahora para ejecutarlo escribimos wp.exe y ya se nos habrá ejecutado:
![[Pasted image 20230410102614.png]]
Una vez ejecutada esta herramienta, debemos fijarnos si en el apartado AlwaysInstallElevated está activada como 1, lo cual indica que en ese caso podemos escalar privilegios siguiendo las instrucciones del enlace que se nos muestra, por tanto lo pegamos; donde vemos que se nos dice que tenemos que crear un virus con msfvenom que tenga extensión msi y ejecutarlo en la máquina víctima:
![[Pasted image 20230410102645.png]]
Aunque siguiendo otro tutorial, mejor crear con msfvenom este otro payload que nos envía una reverse shell:
![[Pasted image 20230410102653.png]]
Lo creamos con msfvenom:
![[Pasted image 20230410102701.png]]
Ahora lo compartimos con la máquina víctima a través del servidor con Python:
![[Pasted image 20230410102709.png]]
Y para ejecutar un fichero msi en Windows lo hacemos con este comando:
![[Pasted image 20230410102717.png]]
Ahora habremos recibido una reverse shell como el usuario authority/system con netcat:
![[Pasted image 20230410102725.png]]
Y ya podemos acceder a la flag de root:
![[Pasted image 20230410102733.png]]
