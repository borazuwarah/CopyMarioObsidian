Puedo crear un usuario en el Dockerfile como hice anteriormente y luego ejecutar la terminal con ese mismo usuario:

![[Pasted image 20221217110720.png]]

Después de crear la imagen, ahora si ejecuto un terminal el usuario mario aparecerá en el prompt:

![[Pasted image 20221217110727.png]]

¿Pero qué ocurre cuando tenemos dos usuarios y unas veces quiero ejecutar el contenedor con un usuario y otra veces con el otro?

Primero monto la imagen que tenga por ejemplo dos usuarios:

![[Pasted image 20221217110735.png]]

Una vez añadidos dos usuarios al contenedor, a la hora de ejecutar el contenedor puedo elegir con qué usuario se va a ejecutar:

![[Pasted image 20221217110747.png]]
