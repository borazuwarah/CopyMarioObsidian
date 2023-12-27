Lo primero será crear la base de datos, para ello como requisitos previos será tener instalado mysql y también la imagen de mysql.

Una vez hecho esto, vamos a crear el contenedor de mysql, para ello usamos el comando docker run -d –name my-db1 -e “MYSQL_ROOT_PASSWORD=”123123” mysql. Con este comando estamos creando la contraseña para acceder a la base de datos que está dentro del contenedor:

Las instrucciones se pueden ver en dockerhub dentro de la imagen de mysql:
![[Pasted image 20221231201101.png]]
Nos bajamos la imagen de MySQL:
![[Pasted image 20221231201109.png]]
Ahora arrancamos el contenedor de MySQL y ya tenemos creado el contenedor, poniendo su contraseña:
![[Pasted image 20221231201117.png]]
Ahora, el siguiente paso será inspeccionar el contenedor de database para conocer su dirección IP:
![[Pasted image 20221231201139.png]]
Y aquí la tenemos, la vamos a copiar porque la usaremos más adelante:
![[Pasted image 20221231201150.png]]
Ahora vamos a conectarnos desde el ordenador anfitrión con mysql instalado al contenedor donde está la base de datos, poniendo la IP del contenedor que copiamos antes y la contraseña:
![[Pasted image 20221231201159.png]]
Y ya estamos dentro:
![[Pasted image 20221231201208.png]]
Ahora ya puedo gestionar la base de datos con sentencias sql, por ejemplo así:
![[Pasted image 20221231201225.png]]
Por ejemplo voy a crear una base de datos:
![[Pasted image 20221231201234.png]]
Y ya tengo creada esta base de datos:
![[Pasted image 20221231201251.png]]
![[Pasted image 20221231201255.png]]
También puedo hacer de todo, por ejemplo crear una nueva tabla e introducir datos en ella:
![[Pasted image 20221231201306.png]]
Vamos a ver el contenido:
![[Pasted image 20221231201318.png]]
Y ahora vamos a introducir registros:
![[Pasted image 20221231201329.png]]
Y por último haremos una consulta:
![[Pasted image 20221231201343.png]]
