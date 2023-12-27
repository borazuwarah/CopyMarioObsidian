Si encontramos un proyecto donde tenga una carpeta .git, significa que se trata de un proyecto de github:

![[Pasted image 20221217112510.png]]

En estos casos podemos ejecutar un comando que es git log, para conocer los cambios que se le hicieron a este proyecto; y vemos que hay una actualización con un token que dice que la carpeta .env se ha borrado:

![[Pasted image 20221217112517.png]]

Pues ahora vamos a conocer exactamente qué han modificado con la web de antes y pegando ese token en el comando de git show, para conocer la información de este token:

![[Pasted image 20221217112525.png]]

Y se nos muestra esta información, donde se nos especifican todos los cambios del proyecto:

![[Pasted image 20221217112539.png]]


