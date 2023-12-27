Hacemos los reconocimientos de siempre:
![[Pasted image 20221218205919.png]]
Hacemos un whatweb:
![[Pasted image 20221218205927.png]]
Y la web es una plantilla de apache:
![[Pasted image 20221218205935.png]]
De todos modos tenemos que asegurarnos de que la IP resuelva bien, por tanto vamos a probar en apuntar en el /etc/hosts la dirección bank.htb:
![[Pasted image 20230218082424.png]]
Entonces ahora si accedemos a esta dirección:
![[Pasted image 20221218205949.png]]
Y vemos que esta web trabaja con php:
![[Pasted image 20221218205956.png]]
Haremos un fuzzing para ver directorios ocultos:
![[Pasted image 20221218211349.png]]
Y nos encontró una ruta de uploads:
![[Pasted image 20221218211429.png]]
Que si entramos en esta ruta vemos que no tenemos los permisos:
![[Pasted image 20221218210036.png]]
Pero vemos que también tenemos otra ruta que se llama assets, por tanto vamos a entrar y nos encontramos con estos documentos:
![[Pasted image 20221218211611.png]]
![[Pasted image 20221218210045.png]]
También nos encontró una ruta que es balance-transfer:
![[Pasted image 20221218212221.png]]
Por tanto, si entramos en balance-transfer nos encontramos con muchos archivos:
![[Pasted image 20221218212252.png]]
Por tanto vamos a descargarnos uno de ellos y los examinamos con el terminal:
![[Pasted image 20221218210118.png]]
Entramos en el fichero y nos encontramos con unas credenciales ocultas en BASE64:
![[Pasted image 20221218210127.png]]
Pero tenemos que buscar alguno de estos archivos que sea diferente al resto para ver si esconde algo diferente, para ello podemos obtenerlos todos con curl:
![[Pasted image 20221218210133.png]]
Pero nosotros solo queremos sacar los números que sean diferentes, para ver si hay otro que tiene un peso diferente; entonces para ello debemos filtrar las columnas así con awk:
![[Pasted image 20221218210140.png]]
Con tr podemos quitar comillas, con awk filtramos las columnas y eliminamos columnas que no queremos, y luego ya es solo ir probando con grep hasta acertar y llegar al campo que queremos:
![[Pasted image 20221218210147.png]]
Ahora podemos descargarla con el comando wget:
![[Pasted image 20221218210152.png]]
Y si la examinamos, vemos que tiene unas credenciales visibles:
![[Pasted image 20221218210159.png]]
Entonces ahora vamos a probar a iniciar sesión:
![[Pasted image 20221218210213.png]]
Y nos entró correctamente:
![[Pasted image 20221218210219.png]]
Ahora vemos que tenemos un apartado donde subir archivos dentro de support:
![[Pasted image 20221218210226.png]]
Por tanto, como la web interpreta php, vamos a subir un archivo php malicioso que nos permita ejecutar comandos remotamente:[[0 - Consideraciones Previas#Código PHP malicioso para ganar Reverse Shell]]
![[Pasted image 20221218210233.png]]
Pero al subirlo nos da este error:
![[Pasted image 20221218210240.png]]
Por tanto tendremos que subir el archivo directamente con burpsuite para que la petición pueda ser interceptada, y así conocer qué extensión acepta esta web:
![[Pasted image 20221218210246.png]]
Ahora al dar a submit habrá sido interceptada por burpsuite, donde debemos eliminar donde pone content type y cambiarlo para que el servidor piense que se trata de una imagen:
![[Pasted image 20221218210253.png]]
Si mandamos esto al repeater podremos ir viendo las respuestas, donde podemos ver que nos dicen que la extensión HTB será aceptada e interpretada como si fuera php:
![[Pasted image 20221218210300.png]]
Lo que debemos hacer en este punto es subir el mismo archivo pero con la extensión htb:
![[Pasted image 20221218210306.png]]
Y ahora subimos este archivo:
![[Pasted image 20221218210312.png]]
Hacemos clic en el archivo y tratamos de ejecutar una cmd poniendo ?cmd=whaomi, por ejemplo:
![[Pasted image 20221218210319.png]]
Ahora que ha funcionado, vamos a lanzarnos una reverse shell, primero urlencodeando con burpsuite el comando de enviarnos una bash y después poniéndonos a escuchar con netcat: [[0 - Consideraciones Previas#CONSEGUIR REVERSE SHELL DE MÁQUINA VÍCTIMA A NUESTRO EQUIPO]]
![[Pasted image 20221218210324.png]]
![[Pasted image 20221218210327.png]]
Pegamos la url:
![[Pasted image 20221218210336.png]]
Y recibimos la conexión:
![[Pasted image 20221218210342.png]]
Y navegando por los directorios ya obtenemos la flag:
![[Pasted image 20221218210348.png]]
Para escalar privilegios, hay un script de Python dentro del directorio /var que se llama emergency, que lo que hace es escalar privilegios, por tanto solo tendremos que ejecutarlo y ya seremos usuarios root:
