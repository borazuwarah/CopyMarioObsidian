## (Virtual hosting etc-hosts, path traversal para ver puertos internos de ruta proc - net - tcp, y comando locate para localizar donde se guarda un el tomcat-users.xml e inicio de sesión en tomcat, y activar pseudo-consola) -- INCOMPLETA

Hacemos un fuzzing en esta web y vemos que sólo nos encuentra un directorio de files y assets, al que no tenemos permiso:Hacemos los reconocimientos de siempre:
![[Pasted image 20230312220429.png]]
Lanzamos un whatweb porque vemos el puerto 80 y 8080 abierto:
![[Pasted image 20230312220438.png]]
Esto es lo que nos encontramos por el puerto 80:
![[Pasted image 20230312220446.png]]
Y esto otro por el puerto 8080:
![[Pasted image 20230312220456.png]]
Si hacemos clic en alguna de las secciones de la web nos damos cuenta que tiene aplicado virtual hosting, por tanto tenemos que modificar el fichero /etc/hosts:
![[Pasted image 20230312220505.png]]
Lo modificamos de esta manera, apuntando la IP a la dirección que vemos en la captura anterior:
![[Pasted image 20230312220514.png]]
Y ahora ya nos funcionan todos los apartados de la web:
![[Pasted image 20230312220528.png]]
Hacemos un fuzzing en esta web y vemos que sólo nos encuentra un directorio de files y assets, al que no tenemos permiso:
![[Pasted image 20230312220544.png]]
![[Pasted image 20230312220548.png]]
No obstante, el hecho de que tengamos en la URL la palabra file nos indica que puede ser vulnerable a path traversal:
![[Pasted image 20230312220559.png]]
Por tanto vamos a probar en ubicarnos al etc/password y efectivamente es vulnerable:
![[Pasted image 20230312220607.png]]
Vemos que hay un usuario llamado ash que tiene capacidad de ejecutar una bash, que es un usuario válido:
![[Pasted image 20230312220618.png]]
Y nos encontramos con esta información en formato hexadecimal, por tanto vamos a extraer únicamente estas dos columnas para poder decodificarlas todas a la vez, y esto lo haremos seleccionando con ctrl y alt mientras seleccionamos:
![[Pasted image 20230312220633.png]]
Pero ahora que podemos listar archivos, se nos muestra esta información por el puerto 8080, diciendo que los usuarios se encuentran ubicados en esta ruta:
![[Pasted image 20230312220646.png]]
Si accedemos a esta ruta, pero nos aparece vacío, y esto es porque no se encuentra en esta ruta:
![[Pasted image 20230312220654.png]]
Por tanto ahora debemos saber donde se encuentra este fichero de tomcat-users.xml, por tanto podemos crear un contenedor de ubuntu para instalar tomcat y así saber la ubicación de este fichero, por tanto levantamos un contenedor de ubuntu e instalamos tomcat9:
![[Pasted image 20230312220722.png]]
Y ahora con el comando locate
podemos buscar donde se encuentra este fichero dentro del sistema, para ello instalamos locate también:

Y ahora una vez instalado podemos ejecutar el comando para saber donde se encuentra ese fichero, para ello primero ejecutamos el comando updatedb para que se actualice la base de datos para el comando locate y luego ya buscamos con el comando locate, donde nos encuentra dos directorios:
![[Pasted image 20230312220744.png]]
Por tanto ahora vamos a buscar estos dos archivos dentro de la máquina objetivo con path traversal, aunque debemos poner primero view-source para acceder a estas rutas:
![[Pasted image 20230312220755.png]]
Y aquí ya vemos unas credenciales:
![[Pasted image 20230312220802.png]]
Ahora que tenemos estas credenciales, vamos a entrar a iniciar sesión en tomcat escribiendo en la url manager/html:
![[Pasted image 20230312220816.png]]
Y aquí pondremos las credenciales que hemos encontrado antes:
![[Pasted image 20230312220825.png]]
Y nos entra pero aparece un mensaje que no tenemos permisos para continuar:
![[Pasted image 20230312220833.png]]
Por tanto vamos a buscar alguna vulnerabilidad de tomcat por google y encontramos esta página:
![[Pasted image 20230312220841.png]]
Y nos dicen que si ponemos en la url host-manager que funciona:
![[Pasted image 20230312220850.png]]
Por tanto vamos a probar y vemos que tras poner las mismas credenciales ya estamos dentro:
![[Pasted image 20230312220858.png]]
Una vez que estamos dentro sólo nos falta subir un archivo war malicioso creado con msfvenom y subirlo de alguna manera, pero en este caso no tenemos apartado de subir archivos, por tanto tendremos que subirlo con curl. Pero primero creamos el payload con msfvenom:
![[Pasted image 20230312220910.png]]
Utilizamos este payload y le añadimos el lhost y lport:


Ahora que ya tenemos el archivo malicioso war ya podemos subirlo con curl, y por internet podemos obtener instrucciones de cómo hacerlo:
![[Pasted image 20230312220924.png]]
![[Pasted image 20230312220928.png]]
Vamos a subirlo con curl:
![[Pasted image 20230312220934.png]]
Entonces ahora en la web con poner sólo shell ya nos estaría abriendo nuestro fichero malicioso:
![[Pasted image 20230312220941.png]]
Y con netcat habríamos recibido la conexión:
![[Pasted image 20230312220948.png]]
Ahora vamos a activar una pseudo consola para estar más cómodos:
![[Pasted image 20230312220958.png]]
Y navegando por los directorios vemos que para el usuario ash no tenemos permiso para entrar:
![[Pasted image 20230312221008.png]]
Ahora tenemos que efectuar un user pivoting para convertirnos en el usuario ash: