Un servidor proxy consiste en un programa o dispositivo que hace de intermediario en las peticiones de recursos que realiza un cliente a otro servidor, por tanto voy a instalar el servidor proxy en mi máquina Ubuntu Server con un sudo apt install squid:
![[Pasted image 20230125103929.png]]
Activamos el servicio de squid y consultamos cual es el puerto que está utilizando:
![[Pasted image 20230124210408.png]]
Y ahora con este comando podemos comprobar por qué puerto por defecto que está escuchando este proxy, que es el 3128:
![[Pasted image 20230124210430.png]]
Ahora vamos a entrar dentro de la configuración de squid, que está dentro de /etc/squid:
![[Pasted image 20230124210455.png]]
Una vez dentro de este fichero que es enorme podemos borrarlo todo e ir haciendo nuestra configuración poco a poco:
![[Pasted image 20230125104953.png]]
Para eliminar todo el texto podemos hacerlo con el editor gedit:
![[Pasted image 20230125105217.png]]
Y lo primero será añadir un nombre del proxy y el puerto por el cual estará corriendo, que en mi caso puedo dejar el que viene por defecto:
![[Pasted image 20230125105433.png]]
Y ahora también podemos configurar la cache del proxy, ya que el proxy necesita una configuración para guardar logs y archivos. En este caso establezco una configuración genérica:
![[Pasted image 20230125105637.png]]
Y ahora configuramos los logs del proxy:
![[Pasted image 20230125110117.png]]
A continuación las ACL, que esto básicamente serán las reglas que se van a aplicar, es decir, define un tipo de tráfico. Por tanto creamos una variable que será websDenegadas que almacenarán dominios que estén dentro de un fichero, y lo mismo con la variable palabrasDenegadas que almacenará palabras prohibidas.
![[Pasted image 20230125110646.png]]
Pero todavía no hemos aceptado ni denegado nada, sólo estamos estableciendo variables, por lo que ahora debemos dar la orden de las prohibiciones:
![[Pasted image 20230125111033.png]]
Así nos quedaría nuestro fichero de configuración:
![[Pasted image 20230125111246.png]]
Ahora vamos a crear los ficheros donde se contienen los dominios bloqueados y las palabras bloqueadas:
![[Pasted image 20230125111353.png]]
Y ahora creamos el otro fichero de palabrasDenegadas:
![[Pasted image 20230125111429.png]]
Y ya tenemos los dos archivos:
![[Pasted image 20230125111508.png]]
Ahora vamos a comprobar queno haya ningún error sintáctico dentro de nuestro fichero de squid; y para ello usamos el comando squid -k parse y vemos que está todo correcto:
![[Pasted image 20230125111656.png]]
Iniciamos el servicio de squid con systemctl:
![[Pasted image 20230125111755.png]]
Y con nmap comprobamos que está funcionando por el puerto 3128:
![[Pasted image 20230125111837.png]]
# PROBAMOS SQUID DESDE UN CLIENTE
Vamos a abrir un cliente windows para comprobar si el proxy está funcionando, por tanto vamos a los ajustes del sistema e indicamos la IP y el puerto del servidor proxy:
![[Pasted image 20230125112216.png]]
![[Pasted image 20230125112249.png]]
Y ahora ya tenemos el proxy activado, por lo que si intentamos acceder a uno de los dominios denegados como marca.es, veremos que se bloquea:
![[Pasted image 20230125112348.png]]
Pero en cambio si vamos a cualquier otra web sí podremos porque sólo el marca y el sport estaban bloqueados:
![[Pasted image 20230125112433.png]]
## IMPLEMENTAR AUTENTICACIÓN Y GESTIÓN DE LOGS
Añadimos estas nuevas líneas dentro del fichero de configuración del proxy, donde decimos primero donde estará el fichero que guardará las claves, después el número máximo de usuarios conectados simultáneamente:
![[Pasted image 20230125113216.png]]
Y también debemos crear esta línea dentro de las líneas de autenticación, para activarlo:
![[Pasted image 20230125113532.png]]
Y ahora dentro del control de acceso creamos esta nueva regla para que se permita la autenticación:
![[Pasted image 20230125113643.png]]
Así debería quedarnos:
![[Pasted image 20230125113712.png]]
### CREAR CREDENCIALES PARA QUE SÓLO PUEDAN NAVEGAR LOS USUARIOS REGISTRADOS
Para crear credenciales para los usuarios, necesitamos un paquete que se llama htpasswd, que lo podemos obtener con el comando apt install apache2-utils:
![[Pasted image 20230125114415.png]]
Y ahora por ejemplo vamos a crear el usuario Juan con una contraseña que nos va a encriptar (en mi caso será la contraseña alvarez):
![[Pasted image 20230125114515.png]]
Ahora dentro del fichero tenemos el usuario juan con su contraseña encriptada:
![[Pasted image 20230125114656.png]]
Ahora una vez que lo tengamos todo listo, ya podemos reiniciar el servicio con un systemctl restart squid:
![[Pasted image 20230125114803.png]]
Y ahora si vamos al equipo cliente veremos que nos está pidiendo unas credenciales, donde el usuario era juan y la contraseña alvarez:
![[Pasted image 20230125115206.png]]
Y el tráfico denegado sigue denegado de esta forma, ya que sólamente con esta configuración estamos estableciendo unas credenciales:
![[Pasted image 20230125115554.png]]
## DENEGAR TRÁFICO A UN DETERMINADO EQUIPO
Si queremos denegar todo el tráfico a un determinado equipo, tenemos que ir dentro de las líneas de ACLs y añadir esta, donde pondremos la IP:
![[Pasted image 20230125115808.png]]
Y añadimos la etiqueta de este equipo en esta otra línea:
![[Pasted image 20230125115912.png]]
Reiniciamos el servicio como siempre y veremos que ya no podemos navegar desde el equipo Windows:
![[Pasted image 20230125120108.png]]
Por último, si queremos comprobar los logs de accesos desde las máquinas clientes, podemos acceder al fichero /var/log/squid.conf (que lo habíamos configurado anteriormente), y vemos que podemos acceder a ellos e incluso vemos la dirección IP del equipo cliente con las acciones que hizo:
![[Pasted image 20230125120352.png]]
