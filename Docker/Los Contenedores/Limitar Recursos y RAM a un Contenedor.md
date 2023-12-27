Lo primero va a ser crear un contenedor, para después limitarle los recursos, yo por ejemplo voy a crear uno de nginx:

![[Pasted image 20221217105306.png]]
Hay un comando que es docker stats código_contenedor que nos muestra cuantos recursos consume un determinado contenedor, por ejemplo:

![[Pasted image 20221217105312.png]]

En este caso está usando 13,89mb del total de 7,7gb que tiene mi ordenador. Pero puedo limitar el uso de RAM por ejemplo a un máximo que serán 500mb. Debo hacerlo al montar el contenedor con la opción -m y luego pongo los mb que quiero limitar:

**![[Pasted image 20221217105318.png]]**
Y así quedaría limitado a 500mb el contenedor de nginx:

![[Pasted image 20221217105327.png]]

También puedo incluso limitar sólamente a 1gb el uso de la ram, expresándolo en gigas:
![[Pasted image 20221217105333.png]]

Por ejemplo voy a crear un contenedor de ubuntu, después un bloc de notas en mi ordenador anfitrión y a través del comando docker cp copio el documento escribiendo el nombre del contenedor y después al directorio donde lo quiera mandar.

Antes creo el contenedor, después salgo de él para dejarlo en ejecución con docker start:

![[Pasted image 20221217105340.png]]

Y aquí copiamos el archivo al directorio home dentro del contenedor:

![[Pasted image 20221217105347.png]]

Y ahora ya podemos acceder al contenedor y ver el archivo copiado al directorio home:

![[Pasted image 20221217105356.png]]

Ahora voy a hacerlo al revés, desde el contenedor a mi ordenador anfitrión, aquí tengo el archivo  y su ruta:

![[Pasted image 20221217105405.png]]
![[Pasted image 20221217105408.png]]

Escrito docker cp nombre_contenedor:/ruta del archivo y luego un punto para indicar que lo quiero mover al directorio actual de mi ordenador anfitrión.

![[Pasted image 20221217105415.png]]




