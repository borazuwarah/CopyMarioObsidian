Puedo tener un contenedor ejecutando un servidor nginx donde aloje un documento html y pueda acceder a él desde el navegador:

En primer lugar, debemos ejecutar el contenedor nginx y dentro del Dockerfile, debemos enviar el archivo html a la siguiente ruta que se nos muestra desde el repositorio oficial de nginx en dockerhub. Debemos primero escribir donde esté nuestro fichero html a la ruta que se indica en la captura de pantalla:

![[Pasted image 20221217110556.png]]

Así quedaría el Dockerfile:

![[Pasted image 20221217110607.png]]

Ahora monto la imagen con normalidad llamándola por ejemplo web. Después creamos el contenedor exponiendo los puertos que queramos, por ejemplo en mi caso el puerto 8080:

Creamos la imagen:

![[Pasted image 20221217110615.png]]

Y ahora creamos el contenedor:

![[Pasted image 20221217110624.png]]

Con el -d indicamos que es un demonio, es decir, queremos que no se paralice el contenedor y se mantenga en ejecución.

Y ahora si accedemos con el navegador al localhost:8080 tendremos acceso al fichero HTML dentro del servidor nginx dockerizado:

![[Pasted image 20221217110633.png]]
