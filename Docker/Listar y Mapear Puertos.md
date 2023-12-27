Para ello, por ejemplo vamos a crear un contenedor de jenkins y acceder a varios contenedores desde el navegador. Cada contenedor tendrá un puerto diferente.

Una vez que tengamos el contenedor de jenkins corriendo, si queremos acceder a él desde el navegador, tendremos que fijarnos en los puertos que se marcan en la siguiente captura (se está corriendo en el puerto 80:80):
![[Pasted image 20221217110507.png]]
A continuación, voy a crear otro contenedor de jenkins que reciba la orden de mapear el puerto 8080 del contenedor al puerto 8080 de mi máquina. De esta forma podré acceder desde el navegador:

![[Pasted image 20221217110514.png]]

Ahora si ejecuto el comando docker ps -a, puedo ver el contenedor de jenkins que he creado al principio y también el otro que acabo de crear, donde se me muestra que el puerto 8080 ha sido mapeado al puerto 8080 de mi máquina:

![[Pasted image 20221217110520.png]]

De esta manera, desde el navegador, si accedo al puerto 8080 escribiendo hostname:8080 accederé a la ventana de jenkins, gracias a que se está corriendo en ese puerto:

![[Pasted image 20221217110529.png]]

También puedo crear otro contenedor y tener jenkins corriendo al mismo tiempo pero esta vez en otro puerto:

![[Pasted image 20221217110536.png]]



