Haremos los reconocimientos de siempre:
![[Pasted image 20230220135258.png]]
![[Pasted image 20230220135303.png]]
Y vemos que esta es la web:
![[Pasted image 20230220135315.png]]
Y si hacemos fuzzing nos encontramos con esto, donde vemos un directorio de upload:
![[Pasted image 20230220135323.png]]
Y esto es con lo que nos encontramos por el puerto 10000 que también estaba abierto:
![[Pasted image 20230220135332.png]]
Por otra parte, también tenemos abierto el puer 6379, que corre un servicio llamado redis, el cual es un tipo de base de datos al que nos podemos conectar:
![[Pasted image 20230220135345.png]]
Para este servicio si buscamos información por internet encontramos una web de hacktricks que nos enseña a cómo hacer pentesting en reddis:
![[Pasted image 20230220135400.png]]
![[Pasted image 20230220135405.png]]
Siguiendo las instrucciones, nos conectamos a este servicio de esta manera:
![[Pasted image 20230220135414.png]]
Desde este servicio podemos movernos de cierta manera por los directorios, incluido el directorio
de ssh de esta manera:
![[Pasted image 20230220135422.png]]
Ahora lo que podemos hacer es generar un par de claves ssh pública y privada e importarlas para
conectarnos por ssh a la máquina, por tanto vamos a crear las clave ssh en nuestra máquina
primero:
![[Pasted image 20230220135433.png]]
Y ahora dentro del directorio .ssh ya tenemos una clave pública y privada ssh creada:
![[Pasted image 20230220135442.png]]
Ahora, según se dice en las instrucciones, debemos convertir la id_rsa a un fichero con un formato
en específico, tal y como vemos en la segunda línea:
![[Pasted image 20230220135450.png]]
Ahora ejecutamos estos comandos tal cual y la clave ssh ya estará dentro de la máquina víctima,
por lo que después al conectarnos no nos pedirá la clave:
![[Pasted image 20230220135459.png]]
Una vez creado el fichero, utilizamos el siguiente comando para importar nuestra clave dentro de la máquina:
![[Pasted image 20230220135509.png]]
Y ahora, ejecutamos estos comandos para guardar definitivamente la clave ssh en la máquina
víctima:
![[Pasted image 20230220135518.png]]
Ahora que la clave está importada, si me conecto por ssh ya no me pedirá la contraseña de ssh y
estaremos dentro:
![[Pasted image 20230220135528.png]]
