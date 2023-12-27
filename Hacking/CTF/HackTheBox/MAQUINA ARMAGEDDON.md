## (Vulnerabilidad Drupal 7 con metasploit, escalada privilegios obteniendo credenciales de mysql y entrando y vulnerabilidad snap con dirty shock)
Haremos los reconocimientos con nmap de siempre, y vemos que solo tiene abierto el puerto 22 y el 80:
![[Pasted image 20221218150603.png]]
![[Pasted image 20221218150606.png]]
Y esta es la web:
![[Pasted image 20221218150613.png]]
Tenemos la oportunidad de crear una cuenta así que la vamos a crear, pero vemos que tiene que se aprobada por el administrador:
![[Pasted image 20221218150623.png]]
![[Pasted image 20221218150626.png]]
Si hacemos fuzzing, encontramos algunos directorios y vemos uno que se llama scripts, el cual puede ser muy interesante:
![[Pasted image 20221218150633.png]]
Y en esta ruta tenemos una serie de scripts:
![[Pasted image 20221218150648.png]]
Una vez llegados a este punto, debemos mirar la versión de drupal para ver si existen vulnerabilidades; y podemos ver en el código fuente que estamos ante la versión 7:
![[Pasted image 20221218150654.png]]
Si buscamos por searchsploit drupal 7 vemos que existen muchos exploits y se nos menciona algo que cie drupalgeddon y que podemos explotarlo con metasploit [[Hacking Drupal]]:
![[Pasted image 20221218150701.png]]
Lo buscamos en metasploit:
![[Pasted image 20221218150707.png]]
Establecemos la configuración de LHOST, RHOST y LPORT:
![[Pasted image 20221218150715.png]]
![[Pasted image 20221218150717.png]]
Lanzamos el ataque con metasploit y ya estamos dentro:
![[Pasted image 20221218150726.png]]
Cuando entramos con metasploit tenemos un meterpreter, pero si queremos una shell normal debemos escribir el comando shell y ya tenemos una shell normal como si la intrusión hubiera sido con netcat:
![[Pasted image 20221218150735.png]]
Ahora debemos buscar si existen credenciales de bases de datos dentro de los archivos de configuración de drupal, lo cual se encuentra en la ruta /var/www/html/sites/default/settings.php:
![[Pasted image 20221218150751.png]]
Por tanto ahora que tenemos unas credenciales de bases de datos mysql, vamos a loguearnos dentro de esta base de datos, ejecutando el comando show databases:
![[Pasted image 20221218150758.png]]
Listamos las tablas:
![[Pasted image 20221218150803.png]]
Y listamos los registros de las columnas name, pass de la tabla users:
![[Pasted image 20221218150809.png]]
Esta contraseña viene hasheada, pero podemos hacerle un ataque de fuerza bruta con john the ripper; y vemos que nos la encuentra:[[John The Ripper]]
![[Pasted image 20221218150815.png]]
Ahora que tenemos un usuario y la contraseña, probamos a ver si podemos acceder por ssh utilizando estas credenciales; y vemos que sí:
![[Pasted image 20221218150821.png]]
Aquí ya tenemos la flag de user; y si hacemos un sudo -l vemos que este usuario puede instalar cualquier paquete snap:
![[Pasted image 20221218150837.png]]
Cuando veamos que podemos ejecutar cualquier comando por snap, significa que podemos elevar nuestros privilegios con una vulnerabilidad conocida como Dirty Sock. Si miramos por internet, encontramos un exploit de github en este link que nos permite elevar nuestros privilegios con solo ejecutarlo:
https://github.com/f4T1H21/dirty_sock
![[Pasted image 20230209110046.png]]
Nos lo descargamos en nuestra máquina host:
![[Pasted image 20230209110114.png]]
Lo compartimos montando un servidor http con python a la máquina víctima:
![[Pasted image 20230209110202.png]]
Y desde la máquina víctima lo descargamos:
![[Pasted image 20230209110234.png]]
Lo ejecutamos:
![[Pasted image 20230209110258.png]]
Y nos convertimos en el usuario root:
![[Pasted image 20230209110317.png]]
![[Pasted image 20230209110337.png]]
