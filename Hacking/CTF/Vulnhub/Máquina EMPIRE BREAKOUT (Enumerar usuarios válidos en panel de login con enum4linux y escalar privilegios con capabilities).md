Lo primero será hacer el reconocimiento de los puertos con nmap:
![[Pasted image 20230111163624.png]]
![[Pasted image 20230111163708.png]]
Vemos que entre otras cosas, tiene el puerto 80 abierto, por lo que vamos a acceder desde el navegador poniendo la IP de la máquina, donde vemos que se trata de una plantilla de apache:
![[Pasted image 20230111163809.png]]
Pero si inspeccionamos el código fuente de la web, nos encontramos con un texto encriptado en la parte de abajo del todo:
![[Pasted image 20230111163843.png]]
Vamos a copiar toda esa línea y vamos a pegarla en la web de dcode.fr/brainfuck; y vemos que obtenemos unas credenciales:
![[Pasted image 20230111163953.png]]
 Ahora por otra parte, también vemos que por el puerto 20000 está corriendo otro servicio web que parece un panel de autenticación, por lo que accederemos también desde el navegador y vemos que efectivamente se trata de eso:
 ![[Pasted image 20230111164102.png]]
 En este panel ya tenemos la contraseña que fue la de antes, pero nos falta el usuario, por lo que tendremos que utilizar una herramienta que se llama enum4linux para comprobar la existencia de usuarios válidos, por lo que ejecutamos el siguiente comando:
 ![[Pasted image 20230111164312.png]]
 Y en esta parte vemos que nos encontró un usuario válido, el cual es cyber:
 ![[Pasted image 20230111164347.png]]
Por tanto vamos a probar estas credenciales en el panel de autenticación de antes:
![[Pasted image 20230111164418.png]]
Y vemos que funciona correctamente:
![[Pasted image 20230111164440.png]]
Una vez dentro de este panel de administración, hay un icono de un terminal para poder ejecutar comandos:
![[Pasted image 20230111191843.png]]
![[Pasted image 20230111191904.png]]
Por tanto en este punto vamos a enviarnos una reverse shell ejecutando el comando para enviárnosla directamente, ya que a través de un servidor Python no va a funcionar ya que esta máquina no tiene instalado curl:
![[Pasted image 20230111192243.png]]
![[Pasted image 20230111192254.png]]
Y ya tenemos acceso a la flag de user:
![[Pasted image 20230111192511.png]]
Ahora vamos a ejecutar este comando para ver las capabilities, es decir, qué acciones podremos realizar siendo el usuario que somos; y vemoos que podemos visualizar cualquier documento a través de la herramienta tar:
![[Pasted image 20230111193347.png]]
Y dentro de la ruta /var/backups, vemos que se encuentra un fichero de backup de contraseñas:
![[Pasted image 20230111193056.png]]
Por tanto primero para obtener permiso de lectura de este fichero lo vamos a descomprimir con tar, gracias a que con esta herramienta podemos leer el contenido de este fichero:
![[Pasted image 20230111193555.png]]
Y ahora ya hemos extraído la información de este fichero en formato tar:
![[Pasted image 20230111193624.png]]
Y ahora ya tenemos permisos para leer el fichero .old_pass.bak:
![[Pasted image 20230111193838.png]]
Y aquí tenemos la contraseña de root:
![[Pasted image 20230111193936.png]]
Introducimos esta contraseña como el usuario root con el comando su root:
![[Pasted image 20230111194055.png]]
![[Pasted image 20230111194108.png]]
Hacemos un mínimo tratamiento de la TTY para tener un prompt:
![[Pasted image 20230111194142.png]]
Y ya podemos entrar al directorio de root y visualizar la flag de root:
![[Pasted image 20230111194233.png]]


