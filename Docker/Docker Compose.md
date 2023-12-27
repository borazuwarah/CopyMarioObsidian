(Para crear aplicaciones multicontenedor)
Docker compose sirve para que varios contenedores funcionen entre sí (por ejemplo un servidor con una base de datos) de una manera simplificada en un único archivo de texto, sin necesidad de hacer todo el proceso largo.

Para crear todo esto se hace con Docker compose app.

Para su instalación, utilizamos estos dos comandos:

sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

## PRIMEROS PASOS EN DOCKER COMPOSE

Lo primero será crear una carpeta para tener organizado todo lo del docker compose:

Dentro de la carpeta, el archivo que debemos de crear para utilizar Docker compose tiene extensión yml:
![[Pasted image 20230101173048.png]]
Y esta sería la estructura básica de un docker compose:
![[Pasted image 20230101173058.png]]
Para empezar a ejecutar contenedores en compose, debemos primero especificar la imagen de docker compose. Para ello vamos al navegador y ponemos docker compose versión para conocerla, en este caso estamos en la versión 3:
![[Pasted image 20230101173108.png]]
Después, ponemos esta versión en el archivo yml que creamos antes:
![[Pasted image 20230101173119.png]]
IMPORTANTE: Aquí no se puede usar el tabulador, sólo espacios, sino dará error.

Vamos a crear con docker-compose un contenedor de Jenkins por ejemplo:
![[Pasted image 20230101173134.png]]
Y una vez guardado este archivo, si lo queremos ejecutar para que se creen los contenedores, podemos usar el comando docker-compose up -d:
![[Pasted image 20230101173143.png]]
Ahora como podremos ver, ya tenemos el contenedor creado:
![[Pasted image 20230101173152.png]]
Y ahora si accedemos al puerto 8080 del localhost veremos como funciona correctamente:
![[Pasted image 20230101173204.png]]
Si queremos eliminar este docker compose debemos utilizar el comando docker-compose down, de esta manera se eliminará todo a la vez, el contenedor y el servicio:
![[Pasted image 20230101173213.png]]
Básicamente se elimina todo lo relacionado con el contenedor, por ejemplo si hacemos un docker ps -a vemos como el contenedor fue eliminado:
![[Pasted image 20230101173222.png]]
## VARIABLES DE ENTORNO EN DOCKER COMPOSE
Lo primero será crear por ejemplo un contenedor de mysql introduciendo una variable de entorno dentro del contenedor que será MYSQL_ROOT_PASSWORD=123123, la cual servirá para autenticarnos después para acceder a la base de datos:
![[Pasted image 20230101173258.png]]
Ahora guardamos el archivo y levantamos el contenedor con el comando docker-compose up -d:
![[Pasted image 20230101173307.png]]
Y ahora si entramos dentro de la base de datos con la terminal bash podremos ver que dentro de las variables de entorno que tiene este contenedor, entre ellas está la que acabamos de introducir:
![[Pasted image 20230101173315.png]]
Ahora ya podemos acceder a la base de datos, aunque primero debemos conocer la IP de este contenedor, para ello hacemos un docker inspect a este contenedor y sacamos la IP:
![[Pasted image 20230101173325.png]]
Y ahora ya podremos acceder a la base de datos creada con docker-compose:
![[Pasted image 20230101173333.png]]
Otra opción que también tenemos para introducir variables de entorno es creando un archivo con extensión .env donde se recojan todas las variables de entorno que queramos introducir, lo cual es útil si queremos introducir muchas variables a la vez:

Por ejemplo voy a introducir dos variables dentro del archivo texto.env creado anteriormente, primero la variable de MySQL y luego otra hecha por mí muy simple:
![[Pasted image 20230101173342.png]]
Ahora debemos indicar en el archivo yml que lea directamente las variables del archivo que hemos creado(el archivo texto.yml), para ello lo hago con env_file.
![[Pasted image 20230101173351.png]]
Después de guardar lo anterior y levantar el contenedor con docker-compose up, ya podremos acceder a las variables de entorno de este contenedor y veremos como se leyeron ambas variables dentro del archivo texto.env:
![[Pasted image 20230101173400.png]]
## VOLÚMENES EN DOCKER COMPOSE
Para crear un contenedor en docker compose, primero debo escribir el nombre del volumen y después en otra línea especificarlo con el directorio donde se va a configurar dicho volumen dentro del contenedor:
![[Pasted image 20230101173432.png]]
Y ahora montamos el contenedor con docker compose:
![[Pasted image 20230101173440.png]]
Y ahora si entramos dentro de las configuraciones de docker, podemos acceder a la parte de los volúmenes, donde veremos que está creado el volumen:
![[Pasted image 20230101173448.png]]
![[Pasted image 20230101173452.png]]
![[Pasted image 20230101173456.png]]
![[Pasted image 20230101173500.png]]
Ahora entramos dentro de la carpeta docker-compose-vol2 y luego dentro de data:
![[Pasted image 20230101173508.png]]
En este punto puedo crear archivos y luego en el contenedor volverán a estar dentro del directorio /home que configuramos antes con docker-compose:
![[Pasted image 20230101173517.png]]
Y ahora entramos en el contenedor al directorio home y veremos como existe el documento que acabamos de crear:
![[Pasted image 20230101173528.png]]
Ahora si queremos eliminar el volumen, hay que tener en cuenta que con el comando docker-compose down no será suficiente, por tanto, si volvemos a levantar el contenedor, se volverá a coger el mismo volumen.

Otra forma de compartir archivos es especificando en docker-compose que se haga un reflejo de una carpeta de nuestro equipo anfitrión con el ordenador, por ejemplo todo lo que esté dentro del directorio donde estoy trabajando se replique en el directorio /home dentro del contenedor. Este tipo de volumen se conoce como volumen de host.

Lo haríamos de esta manera, en la parte de volumes indicamos que lo que esté dentro de la carpeta docker-compose se replique en el directorio home de dentro del contenedor:
![[Pasted image 20230101173538.png]]
Y ahora si accedemos dentro del contenedor veremos como están dentro estos archivos:
![[Pasted image 20230101173547.png]]
## REDES EN DOCKER COMPOSE
En primer lugar, vamos a crear una red con docker compose:
![[Pasted image 20230101173603.png]]
Después montamos el contenedor:
![[Pasted image 20230101173610.png]]
Y si hacemos un docker inspect podemos ver como este contenedor ya tiene asignada una red:
![[Pasted image 20230101173617.png]]
## CREAR DOS CONTENEDORES O MÁS CON DOCKER COMPOSE
Para eso lo hacemos dentro del archivo docker-compose.yml duplicando la parte donde creamos y configuramos los contenedores:
![[Pasted image 20230101173630.png]]
Y ahora al montarlo con docker-compose podremos ver cómo se crean los dos contenedores dentro de la misma red:
![[Pasted image 20230101173637.png]]
Y ahora si inspeccionamos los dos contenedores, veremos como los dos se encuentran dentro de la misma red:
![[Pasted image 20230101173647.png]]
![[Pasted image 20230101173651.png]]
### OTRO EJEMPLO DE CREAR DOS CONTENEDORES A LA VEZ DOCKER COMPOSE
Por ejemplo, creamos la base de datos postgres con un contenedor adminer que será lo que gestione la base de datos.
![[Pasted image 20230101173721.png]]
Ahora ejecutamos con docker-compose up y ya tendremos levantado dos contenedores de la base de datos con su administrador:
![[Pasted image 20230101173729.png]]
![[Pasted image 20230101173735.png]]
Si iniciamos sesión con las credenciales que configuramos en Docker-compose podremos acceder a la base de datos:
![[Pasted image 20230101173743.png]]
![[Pasted image 20230101173747.png]]
## CONSTRUIR IMÁGENES EN DOCKER COMPOSE (dockerfile)
Debemos crear un Dockerfile dentro de nuestro directorio, para hacerlo más simple voy a crear una carpeta llamada build y lo haré todo desde dicha carpeta:
![[Pasted image 20230101173759.png]]
Ahora si quiero crear una imagen con docker compose debo crear el fichero .yml dando la orden build y un punto, para que encuentre un Dockerfile y construya la imagen:
![[Pasted image 20230101173857.png]]
Y después creamos el Dockerfile, que lo haré con una imagen de centos y con una carpeta llamada prueba creada dentro del directorio /home:
![[Pasted image 20230101173905.png]]
Ejecutamos con docker-compose build:
![[Pasted image 20230101173913.png]]
Y ahora si comprobamos nuestras imágenes, veremos como tenemos una que se llama compose_prueba:
![[Pasted image 20230101173923.png]]
## VINCULAR DOS CONTENEDORES CON DOCKER COMPOSE
Lo haremos en donde pone links (ahí vinculamos un contenedor con otro):
![[Pasted image 20230101174002.png]]
## LIMITAR RECURSOS DE UN CONTENEDOR USANDO DOCKER-COMPOSE
Por ejemplo para limitar el uso de memoria y cpu lo hago de la siguiente manera:
