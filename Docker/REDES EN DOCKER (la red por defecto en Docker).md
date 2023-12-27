Si utilizo el comando ip a | grep docker puedo ver la IP que tiene asignada Docker:
![[Pasted image 20221231201539.png]]
Y si hago un ifconfig con mi equipo anfitrión veremos que la IP de mi tarjeta de red es diferente, a pesar de que tengo una interfaz de docker que sí muestra la misma IP:
![[Pasted image 20221231201549.png]]
Vamos a hacer la prueba de crear un contenedor y veremos como la IP de este contenedor se encuentra dentro del rango de IP de docker:
![[Pasted image 20221231201601.png]]
Y una vez creado el contenedor puedo ejecutar el comando para inspeccionar este contenedor, donde encontraré los detalles sobre su dirección IP:
![[Pasted image 20221231201612.png]]
Es importante mencionar que la red bridge es la red que trae por defecto Docker, es decir, si no especificamos una IP diferente para nuestro contenedor, se irá agrupando en la red bridge:
![[Pasted image 20221231201621.png]]
# CÓMO CREAR UNA RED NOSOTROS MISMOS EN DOCKER
En primer lugar, mencionar que si utilizamos el comando docker network nos saldrán las instrucciones sobre las redes en docker:
![[Pasted image 20221231201634.png]]
Y si utilizo el comando docker network create prueba voy a crear una red que se llama prueba:
![[Pasted image 20221231201642.png]]
Puedo buscar las redes instaladas en docker con el comando network ls, aunque si quiero filtrar por el nombre de la red que acabo de crear debo utilizar el comando  docker network | grep prueba
![[Pasted image 20221231201651.png]]
Y ahora si quiero crear una red con una dirección IP, una máscara de subred y un gateway, lo hago de esta manera:
![[Pasted image 20221231201658.png]]
Podemos volver a buscar las redes que tenemos en Docker:
![[Pasted image 20221231201712.png]]
Y si inspeccionamos el contenido de la red que acabamos de crear (MiRedPrueba), veremos como tiene su IP asignada, máscara y gateway correctamente. Primero vamos a poner el comando para ver la configuración de esta red:
![[Pasted image 20221231201720.png]]
Y aquí se nos muestra la configuración:
![[Pasted image 20221231201730.png]]
Aunque tenemos también mucha otra información interesante:
![[Pasted image 20221231201739.png]]
## AGREGAR CONTENEDORES A UNA RED DISTINTA A LA DE POR DEFECTO
Podemos crear un nuevo contenedor y asignarle a la red que hemos creado anteriormente (MiRedPrueba). Para ello usamos el comando docker run --network MiRedPrueba -it ubuntu bash:
![[Pasted image 20221231201753.png]]
Y ahora si inspeccionamos este contenedor con el comando docker inspect nombre_del_contenedor, podremos ver que tiene la configuración de red de MiRedPrueba:
![[Pasted image 20221231201801.png]]
## CONECTAR CONTENEDORES A LA MISMA RED
Primero voy a crear dos contenedores, uno de Fedora y otro de Ubuntu conectados a la red que creamos antes:
![[Pasted image 20221231201820.png]]
Una vez creados estos contenedor, al estar dentro de una red personalizada, puedo hacer ping y conectarlos usando su nombre simplemente, en vez de tener que poner toda la dirección IP:
![[Pasted image 20221231201831.png]]
## CONECTAR UN CONTENEDOR A VARIAS REDES A LA VEZ
Puedo añadir conexión a otra red de a un contenedor, de tal forma que estará conectado a dos redes diferentes, con el comando docker network connect nombre_red nombre_contenedor:
![[Pasted image 20221231201844.png]]
Ahora si inspecciono las redes de este contenedor, veremos que se encuentra conectado a la red MiRedPrueba:
![[Pasted image 20221231201853.png]]
Y ahora la otra red:
![[Pasted image 20221231201904.png]]
Ahora si queremos desconectar una red de un contenedor, lo haremos de esta manera:
![[Pasted image 20221231201934.png]]
## ELIMINAR REDES
Si quiero eliminar una red, primero debo eliminar los contenedores donde están conectados a la red que quiero eliminar. Una vez hecho eso, puedo utilizar el comando docker network rm MiRedPrueba:
![[Pasted image 20221231201951.png]]
Si utilizo el comando docker network ls veremos como esta red ya no existe, está borrada:
![[Pasted image 20221231202002.png]]
## ASIGNAR UNA IP A UN CONTENEDOR
(primero creamos la red, después el contenedor y por último asignamos la IP que queramos)

Hay que establecer un gateway previamente, como hacemos a continuación:

Yo puedo crear una red y asignársela al contenedor, de esta manera el contenedor estará dentro del rango de las IP que he creado antes:
![[Pasted image 20221231202017.png]]
Después puedo ejecutar un contenedor a partir de una imagen de ubuntu por ejemplo:
![[Pasted image 20221231202025.png]]
Después si hago un docker inspect y el nombre del contenedor (lo obtengo a través de docker ps -a):
![[Pasted image 20221231202035.png]]
Me encontraré con que el contenedor tiene asignada una IP dentro del rango de IP que establecí anteriormente y está bajo la red MiRedPrueba que cree anteriormente:
![[Pasted image 20221231202048.png]]
Pero puedo especificar que mi contenedor tenga una dirección IP en concreto de esta manera:
![[Pasted image 20221231202057.png]]
Y ahora como vemos el contenedor ya tiene asignada exactamente esta dirección IP:
![[Pasted image 20221231202106.png]]
## CONECTAR UN CONTENEDOR A LA MISMA RED QUE NUESTRO EQUIPO ANFITRIÓN (y mismo hostname y todo igual)
En mi caso tengo esta dirección IP en mi equipo anfitrión:
![[Pasted image 20221231202122.png]]
Y puedo ejecutar un contenedor diciéndole que coja la misma dirección IP que mi ordenador anfitrión:
![[Pasted image 20221231202132.png]]
Ahora voy a ejecutar este contenedor después de haber salido para conocer el nombre de este contenedor usando este comando:
![[Pasted image 20221231202144.png]]
Y si hago un ifconfig dentro del contenedor, podré ver como tiene la misma dirección IP que mi ordenador anfitrión:
![[Pasted image 20221231202154.png]]
## LA RED NONE
En docker también hay una red que se llama none, esto significa que los contenedores que metamos aquí no van a tener red.

Por ejemplo voy a ejecutar un contenedor de Ubuntu con esta red none:
![[Pasted image 20221231202210.png]]
Y ahora si hago un docker inspect nombre_contenedor veremos como no tiene conexión a internet:
![[Pasted image 20221231202216.png]]
## PORT FORWARDING EN DOCKER
Yo puedo hacer que el puerto 80 de mi máquina sea el puerto 80 del contenedor. Por ejemplo vamos a correr un contenedor de Ubuntu exponiendo el puerto 80:
![[Pasted image 20230417085647.png]]
Y ahora si miramos la actividad del puerto 80 de nuestra máquina anfitrión, veremos como está siendo ocupado por docker:
![[Pasted image 20230417085722.png]]
Por tanto ahora si dentro del contenedor me creo un servidor web, veremos cómo podemos acceder al mismo desde nuestra máquina anfitrión con el localhost:
![[Pasted image 20230417085914.png]]
![[Pasted image 20230417085927.png]]
