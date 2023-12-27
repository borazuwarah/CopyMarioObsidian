## (Vulnerabilidad en strapi, buscar directorios, versión de scrapy y explotar exploit con python - Vulnerabilidad pkexec para escalar privilegios)

Lo primero será hacer los reconocimientos de siempre:
![[Pasted image 20230316215517.png]]
![[Pasted image 20230316215524.png]]
![[Pasted image 20230316215527.png]]
![[Pasted image 20230316215530.png]]
Una vez hecho todo esto, vamos a acceder a la web, donde nos dará error por lo que tendremos que añadirlo en el /etc/hosts:
![[Pasted image 20230316215542.png]]
Modificamos el fichero hosts:
![[Pasted image 20230316215550.png]]
Y ahora ya podremos acceder:
![[Pasted image 20230316215558.png]]
Ahora que estamos aquí, vamos a hacer un fuzzing para inspeccionar directorios ocultos, primero
directorios:
![[Pasted image 20230316215607.png]]
Y nos encontró esto:
![[Pasted image 20230316215614.png]]ç
Aunque si ponemos estos directorios veremos que no funciona ninguno:
![[Pasted image 20230316215622.png]]
Vamos a hacer otro escaneo de virtual hosting con gobuster para encontrar más dominios ocultos,
pero esta vez utilizaremos un diccionario específico mucho más grande descargado de github:
![[Pasted image 20230316215631.png]]
![[Pasted image 20230316215634.png]]
Y nos encontró este directorio, api-prod-horizontall.htb, por tanto para que nos funcione en el navegador, tenemos que resolverlo en el fichero /etc/hosts:
![[Pasted image 20230316215649.png]]
Y ya podemos acceder a la página, que es esta:
![[Pasted image 20230316215656.png]]
Ahora vamos a hacer otro escaneo de este directorio web, pero con el diccionario normal:
![[Pasted image 20230316215704.png]]
Vamos a probar con admin y nos lleva a una web de strapi para loguearnos:
![[Pasted image 20230316215712.png]]
Aunque lo interesante es que con searchsploit nos encontró vulnerabilidades de strapi:
![[Pasted image 20230316215726.png]]
Ahora el objetivo es conocer la versión de strapi para poder buscar un exploit y vulnerabilidades,
por tanto vamos a seguir buscando directorios pero esta vez con wfuzz por ejemplo, dentro de
admin:[[WFUZZ]]
![[Pasted image 20230316215745.png]]
Y nos encuentra esto:
![[Pasted image 20230316215750.png]]
El de init es interesante porque tiene muchos caracteres e igual tenemos información interesante,
por tanto hacemos un curl de esta dirección y obtenemos la versión de strapi:
![[Pasted image 20230316215800.png]]
Esta versión de strapi coincide con el exploit encontrado antes con searchsploit:
![[Pasted image 20230316215806.png]]
Vamos a descargar por ejemplo el penúltimo, que tiene mejor pinta:
![[Pasted image 20230316215813.png]]
Y si ejecutamos el exploit vemos sus instrucciones, donde sólo nos pide la URL:
![[Pasted image 20230316215820.png]]
Lo ejecutamos y nos restablece unas credenciales:
![[Pasted image 20230316215828.png]]
Y ahora estamos más o menos dentro de la máquina, aunque no podemos ejecutar casi comandos,
pero sin embargo si me mando un ping a mi mismo funciona:
![[Pasted image 20230316215838.png]]
Y por otro lado me pongo a escuchar trazas icmp y me llega el ping:
![[Pasted image 20230316215844.png]]
Ahora podríamos mandarnos una shell reversa, pero no va a funcionar:
![[Pasted image 20230316215852.png]]
Por tanto vamos a hacer montando un servidor web con python para descargar el script con un curl desde la máquina víctima, por tanto primero montamos el html:
![[Pasted image 20230316215903.png]]
Ahora montamos el servidor web:
![[Pasted image 20230316215909.png]]
Y ahora desde la máquina víctima ejecutamos el comando curl de esta dirección y le pipeamos un
bash, mientras estamos escuchando con netcat para recibir la shell:
![[Pasted image 20230316215916.png]]
Lanzamos el comando curl para recibir el comando que tenemos en el servidor:
![[Pasted image 20230316215922.png]]
Y donde estábamos escuchando con netcat ya tenemos la conexión:
![[Pasted image 20230316215929.png]]
Ahora navegando por los directorios, ya tenemos la flag:
![[Pasted image 20230316215935.png]]
Si ejecutamos el comando which pkexec y lo concatenamos con un ls -l podremos saber si la máquina es vulnerable para escalar privilegios por esta vía; y por ejemplo en este caso vemos que sí ya que se encuentra dentro del path:
![[Pasted image 20230316215942.png]]
Aunque también podemos comprobarlo con el comando find / -perm -4000 2>/dev/null:
![[Pasted image 20230316215948.png]]
Y buscamos pkexec en google y entramos en este repositorio de github:
![[Pasted image 20230316220011.png]]
Ahora clonamos este repositorio:
![[Pasted image 20230316220019.png]]
Comprimimos el contenido de todo este repositorio y lo compartimos con el servidor Python con la máquina víctima:
![[Pasted image 20230316220029.png]]
![[Pasted image 20230316220032.png]]
![[Pasted image 20230316220035.png]]
Nos lo enviamos a la máquina víctima y con el comando unzip lo descomprimimos dentro de la máquina víctima y ahora con el comando make lo compilamos:
![[Pasted image 20230316220044.png]]
![[Pasted image 20230316220047.png]]
Y ahora ejecutamos el script y ya somos el usuario root:
![[Pasted image 20230316220053.png]]