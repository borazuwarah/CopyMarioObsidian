IMÁGENES - Es un instalador donde podemos incorporar nuestra aplicación. Es el punto de inicio para crear contenedores.

Hay imágenes oficiales de por ejemplo Ubuntu, Apache, etc, que fueron creadas por sus creadores oficiales.

DOCKER FILE - Es un documento de texto que contiene todos los comandos que queramos ejecutar en la linea de comandos para armar una imágen.

Hay una página que se llama docker hub donde puedo descargar imágenes, por ejemplo voy a descargar una que será hello world:

![[Pasted image 20221217104849.png]]
![[Pasted image 20221217104852.png]]

Para ejecutar este contenedor de “hello-world” debo escribir en el terminal, tal y como se muestra en las instrucciones de la captura anterior, el comando docker run hello-world.

Con esto estamos descargando la imagen y la ejecutamos al mismo tiempo, si solo quiero instalarla debo poner docker pull hello-world:

![[Pasted image 20221217104859.png]]
Si inserto el comando docker-images, puedo consultar las imágenes que tengo descargadas en mi sistema:

![[Pasted image 20221217104907.png]]

Para buscar una imagen en concreto puedo usar grep, por ejemplo que me busque solo las imágenes de ubuntu:

![[Pasted image 20221217104914.png]]
Podemos descargar también otras versiones diferentes de un imagen a través de la ventana de los tags en docker hub:

![[Pasted image 20221217104921.png]]

Y para descargar por ejemplo la versión 21.10 con el tag impish, lo pongo así en terminal:

![[Pasted image 20221217104929.png]]
Para eliminar una imágen, por ejemplo una de kdenlive, debo hacer lo siguiente: docker rmi id_contenedor_kdenlive, y después ya puedo borrar la imágen con el comando docker rmi nombre_imagen:

![[Pasted image 20221217104936.png]]

## **ELIMINAR VARIAS IMÁGENES A LA VEZ**

Para ello utilizo el comando docker images -aq, y se me muestran los id de las imágenes que tengo. Después elimino los contenedores que de esas imágenes con el comando docker rm id_del_contenedor. Después ya puedo borrar las imágenes utilizando el comando docker rmi $(docker images -aq)

![[Pasted image 20221217104948.png]]

También puedo buscar y descargar imágenes directamente desde el terminal sin tener que ir a la web de docker hub, para ello utilizo el comando docker search ubuntu (por ejemplo):

![[Pasted image 20221217104955.png]]
Para instalar una de estas imágenes que tengo disponibles, utilizo el comando docker pull ubuntu, y se me instalaría esta imágen, luego puedo consultarlo otra vez a través del comando docker images.

Para ejecutar esta imagen, escribo el comando docker run ubuntu, o puedo ejecutar ubuntu con un comando incluido al mismo tiempo:

![[Pasted image 20221217105003.png]]

Si quiero ejecutar una imagen durante más tiempo, por ejemplo para ejecutar la terminal bash durante más tiempo y de manera interactiva, debo utilizar el comando docker run -it ubuntu bash:

![[Pasted image 20221217105011.png]]





