Un pod es la unidad donde vamos a controlar los contenedores. En los pods puedo meter varios contenedores a la vez. 
## CREAR UN POD
Vamos a crear un pod que contendrá un contenedor de Ubuntu, lo cual se hace a partir de un fichero yml que por ejemplo lo llamaremos app1.yml, pero debemos hacer que esté en ejecución, por lo que diremos que en su interior haya un bucle while:
![[Pasted image 20230123212031.png]]
Y lo creamos con este otro comando:
![[Pasted image 20230121235259.png]]
Y para ver los pods creados lo haremos de esta forma:
![[Pasted image 20230123210459.png]]
En caso de que veamos que tenemos el pod en Pending, por tanto si nos sale este problema debemos ejecutar este comando para convertir un pod de pending a running:
![[Pasted image 20230122052015.png]]
Y ya lo tenemos corriendo:
![[Pasted image 20230122052040.png]]
## CONSULTAR DETALLES DE UN POD
Para consultar los detalles de un pod, utilizaremos el comando describe de la siguiente manera:
![[Pasted image 20230122052939.png]]
También podemos incluso conocer su dirección IP:
![[Pasted image 20230123210704.png]]
## CONOCER RECURSOS QUE CONSUME UN POD
![[Pasted image 20230123210811.png]]
## CONOCER LOGS DE UN POD
Para ver los logs de un pod de kubernetes, ejecuto este comando:
![[Pasted image 20230123212109.png]]
## EJECUTAR COMANDOS EN UN POD
Para ello lo hacemos de una forma similar a docker, pero aquí primero seleccionamos el pod de ubuntu y después el contenedor de ubuntu-contenedor que esta dentro del pod:
![[Pasted image 20230123212225.png]]
Ahora por ejemplo voy a crear un fichero:
![[Pasted image 20230123213119.png]]
Y ahora ejecutamos el comando para abrirnos un contenedor y vemos que tenemos el fichero creado:
![[Pasted image 20230123213151.png]]
## COPIAR FICHEROS EN KUBERNETES
Vamos a copiar un fichero de la máquina anfitrión al contenedor; y para ello lo haríamos de esta forma:
![[Pasted image 20230123215027.png]]
Y ahora si accedemos dentro del contenedor, nos encontramos con que se copió correctamente al directorio que le habíamos indicado, que era el /home:
![[Pasted image 20230123215248.png]]
