Lo primero es hacer el reconocimiento de puertos con nmap:
![[Pasted image 20230222131737.png]]
Y ahora como siempre con nmap vamos a inspeccionar estos puertos abiertos:
![[Pasted image 20230222131753.png]]
Y aquí tenemos los detalles de estos puertos:
![[Pasted image 20230222131818.png]]
Ahora vamos a analizar la tecnología implicada en el puerto 80 ya que sabemos que habrá una web, utilizaremos la herramienta whatweb:
![[Pasted image 20230222131830.png]]
![[Pasted image 20230222131845.png]]
Pero vemos cómo la dirección da un error porque no la encuentra, y esto se arregla añadiendo este dominio al fichero /etc/host para que la IP apunte a este dominio:
![[Pasted image 20230222131902.png]]
Y ahora si ejecutamos un whatweb ya no habrá ese error y el dominio ya estará disponible:
![[Pasted image 20230222131914.png]]
![[Pasted image 20230222131917.png]]
Tras inspeccionar esta web vemos que no hay nada interesante, por lo que debemos mirar a ver si existen subdominios donde podamos acceder; para ello usaremos un ataque de fuerza bruta por diccionario con wfuzz de esta manera:

De esta manera se hace un ataque por fuerza bruta para descubrir los subdominios; y nos encuentra uno:

Ahora esta dirección debemos añadirla al /etc/host para que nos funcione si intentamos acceder a ella:
![[Pasted image 20230222131953.png]]
Y ahora con esta dirección y habiéndose añadido al fichero de host ya podremos entrar:
![[Pasted image 20230222132004.png]]
Ahora vamos a comprobar si al escribir un correo estamos ante un server side template injection, y efectivamente lo es, por lo que podremos ejecutar comandos:
![[Pasted image 20230222132014.png]]
Ahora para explotar esta vulnerabilidad, debemos conocer el lenguaje que está hecha la web, el cual es Nose.js:
![[Pasted image 20230222132023.png]]
Ahora buscaremos por internet una inyección de código para insertarlo en webs que tengan esta vulnerabilidad y sean Node.js, donde encontramos esta web que explica el SSTI en webs de node.js:
![[Pasted image 20230222132033.png]]
![[Pasted image 20230222132037.png]]
Copiaremos esa línea y vamos a ejecutarla en la web, pero a través de burpsuite, por tanto escribimos un correo aleatorio para que burpsuite lo intercepte (debemos configurar el certificado SSL para que burpsuite intercepte la petición):
![[Pasted image 20230222132048.png]]
Y esta petición será interceptada por burpsuite:
![[Pasted image 20230222132054.png]]
Y pegaremos todo esto:
![[Pasted image 20230222132101.png]]
Aunque tenemos que escapar las comillas para que no haya problemas con ellas, quedando de la siguiente manera:
![[Pasted image 20230222132112.png]]
Y si enviamos todo esto, vemos cómo recibimos una respuesta donde se ejecuta el código del /etc/passwd:
![[Pasted image 20230222132121.png]]
Llegados a este punto yo puedo ejecutar cualquier código, por ejemplo un whoami en lugar de un /etc/passwd; y vemos como nos da un email:
![[Pasted image 20230222132131.png]]
Ahora que podemos ejecutar comandos, vamos a comprobar si hay conexión entre esta máquina remota y mi equipo; para ello vamos a montar un servidor web desde donde vemos las peticiones que se reciben, para después ejecutar una petición a este servidor desde la máquina víctima con curl:
![[Pasted image 20230222132146.png]]
Y ahora lanzamos un curl a la IP de la interfaz de nuestro PC que está conectada a hackthebox (que es la tun0, podemos verla con ifconfig), para ver si desde la máquina víctima llega la petición al servidor web:
![[Pasted image 20230222132159.png]]
Y vemos cómo llegó la petición a nuestro servidor web, por lo que hay conexión:
![[Pasted image 20230222132215.png]]
Ahora desde este servidor podemos subir un index.html con cierto código que se ejecutará en la máquina víctima:
![[Pasted image 20230222132228.png]]
![[Pasted image 20230222132231.png]]
Ahora montamos el servidor con este fichero html que tiene un código que se encargará de enviarnos una reverse shell:
![[Pasted image 20230222132241.png]]
Ahora nos pondremos en espera con netcat por el puerto 443:
![[Pasted image 20230222132252.png]]
Y por último lanzamos la petición con curl desde la máquina víctima al servidor web para que obtenga ese código que nos enviará una reverse shell a netcat:
![[Pasted image 20230222132303.png]]
Lo ejecutamos y ya tenemos acceso:
![[Pasted image 20230222132311.png]]
Ahora que ya hemos ganado acceso a esta máquina, vamos a ir al directorio home para buscar la flag:
![[Pasted image 20230222132321.png]]
Ahora vamos a escalar privilegios, para ello vamos a ejecutar el comando getcap -r /2>/dev/null, donde debemos fijarnos en la segunda fila porque dice que perl puede ejecutar cosas con plenos poderes:
![[Pasted image 20230222132331.png]]
También podemos comprobar que podemos ejecutar código como root desde perl con el comando ls -la:
![[Pasted image 20230222132340.png]]
Ahora debemos crear un script en perl que nos sirve para elevar los privilegios, el cual será este:
![[Pasted image 20230222132350.png]]
Para ejecutarlo lo hacemos con echo -ne:
![[Pasted image 20230222132400.png]]
Y ahora ya somos root:
![[Pasted image 20230222132408.png]]
VÍDEO DE YOUTUBE:
https://www.youtube.com/watch?v=Rvr4eBTecY8&t=17s
