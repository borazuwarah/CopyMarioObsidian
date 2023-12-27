## (Path traversal Grafana obteniendo fichero base de datos, contraseña en base64, escalada de privilegios inspeccionando git commits y vulnerabilidad consul para enviar reverse shell como root)
Empezamos haciendo un escaneo de nmap:
![[Pasted image 20230303022329.png]]
Y estos son los servicios que corren por detrás de estos puertos:
![[Pasted image 20230303022555.png]]
Vemos que tiene el puerto 80 abierto, por lo que hacemos un whatweb:
![[Pasted image 20230303022735.png]]
Y hacemos lo mismo con el puerto 3000 que también corre un servicio web:
![[Pasted image 20230303023115.png]]
Y por el puerto 80 corre esta web:
![[Pasted image 20230303023204.png]]
Y por el puerto 3000 corre esta otra web de grafana:
![[Pasted image 20230303023224.png]]
Y vemos la versión de este grafana, la cual es la 8.2.0, por lo que buscamos con searchsploit y nos encontramos una versión muy parecida, la 8.3.0, por lo que fácilmente será vulnerable:
![[Pasted image 20230303023752.png]]
Nos descargamos este exploit de python; y si miramos su contenido, vemos cómo lo que está haciendo es un intento de path traversal, donde nos dice que en la web existe una ruta llamada /public/puglins/el_plugin_que_sea y luego a partir de ahí se intenta leer un archivo interno de la máquina:
![[Pasted image 20230303024056.png]]
Por tanto podemos hacer esto mismo con un curl de esta forma y seleccionando alguno de estos plugins, donde habría que ir probando para ver si alguno existe para que se acontezca el path traversal:
![[Pasted image 20230303024551.png]]
Ahora en este punto podemos buscar en el archivo de configuración de grafana, por si encontramos unas credenciales; y las encontramos tanto de un usuario y su contraseña:
![[Pasted image 20230307120219.png]]
![[Pasted image 20230307120240.png]]
Ahora vamos a obtener el fichero de bases de datos de grafana, para ver si encontramos unas credenciales para acceder a la base de datos; y si preguntamos a chatgpt, nos dice cual es la ruta donde se guarda este archivo por defecto:
![[Pasted image 20230307120428.png]]
Por tanto vamos a guardarnos ese fichero, que también nos dice que se llama grafana.db:
![[Pasted image 20230307120847.png]]
Ahora vamos a inspeccionar este fichero grafana.db con el comando strings:
![[Pasted image 20230307120922.png]]
Y dentro de este fichero si filtramos por la palabra grafana, nos encontramos con otras credenciales de la base de datos:
![[Pasted image 20230307121226.png]]
Y podemos entender que las credenciales son las siguientes:
```ruby
db: grafana
user: grafana
password: dontStandSoCloseToMe63221!
```
Así que vamos a probar en acceder con estas credenciales a la base de datos MySQL de la máquina:
![[Pasted image 20230307121535.png]]
Y al utilizar el comando show databases nos encontramos con una base de datos un poco extraña:
![[Pasted image 20230307121747.png]]
Por tanto vamos a acceder a esta y a extraer su información, poniendo la instrucción show tables y luego select * from users:
![[Pasted image 20230307121833.png]]
![[Pasted image 20230307121911.png]]
Vamos a decodificar esta contraseña que parece base64, y vemos que la contraseña es anEnglishManInNewYork027468:
![[Pasted image 20230307122002.png]]
Por tanto con esta credencial vamos a iniciar sesión por ssh y con el usuario developer, que vimos que era un usuario válido cuando empezamos a hacer la máquina:
![[Pasted image 20230307122348.png]]
Una vez dentro podemos inspeccionar la carpeta /opt, donde vemos que hay una carpeta que se llama my-app, por lo que suponemos que alguien estuvo desarrollando una aplicación y seguramente estaría haciendo commits a github de la misma:
![[Pasted image 20230308075022.png]]
Por tanto vamos a inspeccionar el commit más reciente con el siguiente comando, donde vemos un servicio que se llama consul.sh y un token que usaremos más adelante:
![[Pasted image 20230308075134.png]]
Por tanto vamos a buscar vulnerabilidades asociadas a este servicio; y encontramos un exploit en github:
![[Pasted image 20230308075224.png]]
![[Pasted image 20230308075328.png]]
Vamos a compartir este exploit con la máquina víctima dentro del directorio /tmp para tener los permisos de escritura:
![[Pasted image 20230308075356.png]]
![[Pasted image 20230308075443.png]]
Y ahora siguiendo las instrucciones del repositorio de github, tenemos que ejecutar esta herramienta, poniendo el token que hemos podido ver antes y nuesta IP de la máquina atacante y puerto donde estaremos escuchando con netcat: [[0 - Consideraciones Previas#CONSEGUIR REVERSE SHELL DE MÁQUINA VÍCTIMA A NUESTRO EQUIPO]]
![[Pasted image 20230308075857.png]]
Y ahora con netcat habremos recibido la conexión como el usuario root:
![[Pasted image 20230308075921.png]]
![[Pasted image 20230308075941.png]]


