Empezamos con los reconocimientos de siempre, que veremos que estamos ante una máquina windows:
![[Pasted image 20230210030049.png]]
![[Pasted image 20230210030053.png]]
![[Pasted image 20230210030057.png]]
Ahora sabemos que es un windows 7 con service pack 1 y de nombre haris-pc, además de que tiene el puerto de smb abierto, que es el 445. Para saber si un equipo es vulnerable a eternal blue, podemos analizarlo con nmap, ya que cuenta con un script que analiza si lo es o no, el cual es el cualquiera de los siguientes comandos:
![[Pasted image 20230210030115.png]]
Y este es el resultado, donde se nos dice que sí es vulnerable:
![[Pasted image 20230210030125.png]]
Ahora, vamos a descargar de un repositorio de github la herramienta para lanzar el ataque, para ello accederemos al siguiente repositorio:
![[Pasted image 20230210030133.png]]
Y lo clonamos:
![[Pasted image 20230210030142.png]]
Vamos a editar el checker.py para añadir en la parte de username un guest, para evitar errores:
![[Pasted image 20230210030153.png]]
Y ahora lo lanzamos contra la máquina para comprobar si es vulnerable a esta herramienta (lo lanzaremos con python2, porque con el 3 da error):
![[Pasted image 20230210030207.png]]
Y da el resultado:
![[Pasted image 20230210030217.png]]
Ahora debemos elegir una de estas vulnerabilidad, por ejemplo samr, por lo que ejecutamos el exploit zzz, donde se nos indicarán las instrucciones, que es poner la IP y el nombre de la vulnerabilidad que es samr:
![[Pasted image 20230210030226.png]]
Aunque primero editaremos un poco este fichero python para que nos ejecute el comando que queramos dentro de la máquina, para lanzarnos una reverse shell:
![[Pasted image 20230210030235.png]]
Luego editamos el comando, el cual vamos a comentar el comando que viene que no nos intereresa:
![[Pasted image 20230210030243.png]]
De esta parte vamos a comentar todas estas líneas y en la parte de service exec lo dejamos así, que es para dar la orden de enviarnos una reverse shell por netcat a nuestro equipo por el puerto 443:
![[Pasted image 20230210030251.png]]
Ahora vamos a descargar un ejecutable de netcat para enviarlo luego a la máquina víctima:
![[Pasted image 20230210030302.png]]
Y ya tenemos el nc64.exe descargado, que es el que nos interesa:
![[Pasted image 20230210030311.png]]
Y ahora vamos a compartir por smb esta carpeta, para acceder a ella desde la máquina víctima explotando la vulnerabilidad:
![[Pasted image 20230210030318.png]]
Ahora el exploit se va a colar en la máquina víctima, se conectará a nosotros y nos enviará la consola interactiva, por lo que primero nos ponemos a escuchar con netcat:
![[Pasted image 20230210030327.png]]
Y ahora lanzamos el exploit, donde va a enviarnos una consola interactiva por el puerto 443 gracias a que estamos también compartiendo el ejecutable de netcat.exe:
![[Pasted image 20230210030336.png]]
Y se lanzaría la reverse shell:
![[Pasted image 20230210030343.png]]
También podemos hacerlo con metasploit, buscamos por el nombre de eternalblue: [[EternalBlue]]
![[Pasted image 20230210030351.png]]
Elegiremos el exploit:
![[Pasted image 20230210030410.png]]
Ejecutamos el comando options y definimos la IP de la máquina víctima:
![[Pasted image 20230210030417.png]]
Ahora definimos nuestra IP local para recibir la conexión:
![[Pasted image 20230210030428.png]]
Ahora lanzamos el ataque con el comando run:
![[Pasted image 20230210030437.png]]
Y al lanzar este ataque, ya tendremos la conexión remota:
![[Pasted image 20230210030444.png]]
Y tras navegar por los directorios, encontramos la flag:
![[Pasted image 20230210030500.png]]
Ahora vamos a conseguir una reverse shell más interactiva, ya que con esta no podemos ejecutar todos los comandos, por lo que vamos a elegir otro payload, por tanto salimos y ejecutamos el comando show payloads y probamos en seleccionar este:  
Ahora ejecutamos el run y vemos que tenemos una shell de más calidad y que podemos ejecutar comandos del sistema:  
Y vemos que ya somos el usuario administrador y por tanto también podemos acceder a la flag de root: