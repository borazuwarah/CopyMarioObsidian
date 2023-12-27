Son aquellos volúmenes que quedan inoperativos y no sirven, es decir, que ningún contenedor los está usando. Para acceder a ellos puedo poner docker volume ls -f dangling=true:
![[Pasted image 20221231200541.png]]
Si utilizo un -q obtengo los id de estos volúmenes:
![[Pasted image 20221231200549.png]]
Y para borrarlos utilizo xargs docker volume rm:
![[Pasted image 20221231200558.png]]
Y ahora como vemos ya no tengo ningún volumen dangling:
![[Pasted image 20221231200612.png]]
## LA INSTRUCCIÓN VOLUME DENTRO DEL DOCKERFILE
Primero creo un contenedor indicando que el volumen va a estar dentro del directorio /home del contenedor:
![[Pasted image 20221231200629.png]]
Ahora construimos esta imagen:
![[Pasted image 20221231200638.png]]
Y ahora, al ejecutar este contenedor, se me habrá creado un volumen anónimo:
![[Pasted image 20221231200648.png]]
Ahora voy a crear un archivo dentro del directorio home de esta imagen de Ubuntu:
![[Pasted image 20221231200709.png]]
Ahora vamos a localizar y dirigirnos hacia el volumen creado anteriormente para ver si tenemos allí el fichero pruebaa.txt que creamos dentro de este contenedor (debo ser root):
![[Pasted image 20221231200738.png]]
A continuación nos dirigimos al directorio volumes y podremos ver nuestro volumen:
![[Pasted image 20221231200746.png]]
Como estamos viendo hay una carpeta que tiene el nombre del volumen, entonces debemos entrar en esta carpeta y después en la carpeta _data:
![[Pasted image 20221231200758.png]]
Y dentro de esta carpeta _data ya tendremos disponible el documento pruebaa.txt que hemos creado antes dentro del contenedor:
![[Pasted image 20221231200807.png]]
Ahora puedo incluso eliminar el contenedor pero el volúmen va a persistir ahí:
![[Pasted image 20221231200830.png]]
## COMPARTIR UN VOLUMEN ENTRE VARIOS CONTENEDORES
Vamos a compartir un volúmenes entre dos o más contenedores, de tal forma que varios contenedores podrán acceder a la misma información del mismo volumen:

Primero por ejemplo ejecuto un contenedor pero dando la orden de crear una carpeta llamada datos en su directorio raíz:
![[Pasted image 20221231200912.png]]
Después, creo OTRO CONTENEDOR llamado ubuntu2 indicando que coja el volumen del anterior contenedor y el nombre de la imagen, que en este caso es Ubuntu:
![[Pasted image 20221231200921.png]]
Después voy al primer contenedor que he creado que se llamaba objective_thomson y pruebo en crear un archivo llamado prueba:
![[Pasted image 20221231200935.png]]
Y si ejecuto el otro contenedor, tenemos los datos sincronizados y compartidos:
![[Pasted image 20221231200945.png]]
Incluso puedo crear un tercer contenedor que será ubuntu3 y también tendría acceso al fichero:
![[Pasted image 20221231201002.png]]
