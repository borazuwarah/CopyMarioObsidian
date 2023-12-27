Tenemos la conexión entre la shell de mongo y la base de datos:
![[Pasted image 20230601115236.png]]
### MOSTRAR LAS BASES DE DATOS
Para mostrar las bases de datos del sistema, usamos el comando show dbs, las cuales serán las bases de datos predefinidas por el sistema:
![[Pasted image 20230602093451.png]]
### CONOCER LA BASE DE DATOS ACTUAL
Para saber la base de datos que se encuentra actualmente en uso, usamos el comando db; y vemos que por defecto utiliza una que se llama test:
![[Pasted image 20230602093609.png]]
### OBTENER AYUDA COMANDO DB.HELP()
También podemos obtener ayuda utilizando el comando db.help()
![[Pasted image 20230602093804.png]]
### CREAR BASE DE DATOS
Para crear una base de datos en mongoDB usamos el comando use seguido del nombre de la base de datos:
![[Pasted image 20230602093946.png]]
Y ahora si ponemos el comando db vemos que ya estamos dentro de la nueva base de datos:
![[Pasted image 20230602094038.png]]
No obstante si listamos las bases de datos, no vemos la que acabamos de crear:
![[Pasted image 20230602094130.png]]
Y esto ocurre porque no tiene ningún dato, entonces hasta que no lleguemos a insertar ningún dato no va a funcionar:
### INSERTAR DATOS EN MONGODB CON UNA COLECCION
Para insertar datos, debemos crear una colección y estar dentro de la base de datos en uso, utilizando la data en formato json:
![[Pasted image 20230602094621.png]]
Y ahora ya tenemos la base de datos creada porque hemos insertado un dato:
![[Pasted image 20230602094657.png]]
### VER COLECCIONES DE MI BASE DE DATOS
Podemos ver las colecciones que están dentro de la base de datos, y por ejemplo tenemos una llamada coleccion que hemos creado anteriormente:
![[Pasted image 20230602094755.png]]
### ELIMINAR UNA BASE DE DATOS
Para eliminar una base de datos en mongoDB, podemos hacerlo con db.dropDatabase(), y nos eliminará la base de datos que tengamos actualmente en uso:
![[Pasted image 20230602094952.png]]
