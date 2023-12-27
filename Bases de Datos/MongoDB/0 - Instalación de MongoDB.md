Vamos a descargar la edición MongoDB Community Server:
![[Pasted image 20230601110554.png]]
En la instalación, en esta ventana, nos preguntan si queremos que mongodb funcione como un servicio (que siempre esté activo y disponible) o si queremos que esté apagado y debamos encenderlo cada vez que queramos usarlo, lo cual es la opción que elegiremos:
![[Pasted image 20230601111017.png]]
Y ahora procedemos con la instalación:
![[Pasted image 20230601111101.png]]
Una vez que mongodb se haya instalado, tenemos que ir a un cmd y poner el comando mongod --version para comprobar que se haya instalado correctamente, pero vemos que no funciona porque no tenemos añadida esta dirección al path:
![[Pasted image 20230601113026.png]]
Por lo que tenemos que localizar el mongodb y acceder a la ruta para añadirla al path:
![[Pasted image 20230601113303.png]]
Por lo que ahora vamos al apartado de mi equipo->propiedades-> variables de entorno:
![[Pasted image 20230601113430.png]]
Y aqui hacemos doble clic en path y añadimos la ruta anterior:
![[Pasted image 20230601113531.png]]
Y ahora ya tenemos el mongo funcionando correctamente desde el cmd:
![[Pasted image 20230601113627.png]]
Ahora tenemos que crear una carpeta llamada data y dentro otra llamada db en el disco duro para poder iniciar correctamente el mongodb:
![[Pasted image 20230601113836.png]]
Y ahora si ejecutamos el comando mongod, se queda la base de datos cargada en el terminal:
![[Pasted image 20230601113926.png]]
Y tenemos que fijarnos en el puerto en el que está esperando las conexiones:
![[Pasted image 20230601114032.png]]
Ahora tenemos que descargar e instalar la mongosh para poder acceder a la base de datos:
![[Pasted image 20230601114414.png]]
![[Pasted image 20230601114440.png]]
Descomprimimos el zip y abrimos esa shell:
![[Pasted image 20230601114910.png]]
Damos a enter y ya habremos establecido la conexión:
![[Pasted image 20230601114931.png]]

