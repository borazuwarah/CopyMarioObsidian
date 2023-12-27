Lo primero será ejecutar un nmap para ver los puertos abiertos, y nos muestra los siguientes:
![[Pasted image 20230410103543.png]]
Ahora vamos a inspeccionar esos 3 puertos abiertos con nmap:
![[Pasted image 20230410103553.png]]
Y el resultado es este, donde se nos muestra que entre otras cosas, el servidor web es un Centos:
![[Pasted image 20230410103606.png]]
Ahora vamos al navegador para conocer mejor este servidor web:
![[Pasted image 20230410103623.png]]
Pero esta web parece que es una web por default que no tiene nada interesante, por tanto tendremos que investigar más sobre esta web, para ello podemos utilizar whatweb, donde se nos muestra al final del todo cual es el backend que corre detrás de esta app:
![[Pasted image 20230410103633.png]]
Ahora ya que conocemos que el backend es office.paper, debemos modificar el fichero host para indicar que la IP de la máquina apunte a esta dirección:
![[Pasted image 20230410103644.png]]
Y aquí añadimos que la IP apunte a esa dirección:
![[Pasted image 20230410103657.png]]
Y ahora como vemos ya hay comunicación con este dominio:
![[Pasted image 20230410103709.png]]
Accederemos a él desde el navegador:
![[Pasted image 20230410103718.png]]
Y aquí vemos a un usuario:
![[Pasted image 20230410103729.png]]
Y si hacemos clic en él, vemos cómo se cambia el directorio web, y esto nos puede indicar que podemos usar path traversal:
![[Pasted image 20230410103737.png]]
Entonces ahora tenemos que buscar un apartado de login escondido; y para ello podemos ver qué web es esta con wappalyzer, y nos indica que es un wordpress:
![[Pasted image 20230410103745.png]]
Con wordpress, los menús de iniciar sesión tienen la dirección wp.login:
![[Pasted image 20230410103753.png]]
Entonces accedemos a un menú de autenticación:
![[Pasted image 20230410103804.png]]
Vamos a probar en iniciar sesión con prisonmike, y veremos que nos dice que al menos el usuario sí existe:
![[Pasted image 20230410103812.png]]
Si seguimos investigando y entramos dentro de una de las publicaciones, vemos que en un comentario se menciona algo de post ocultos:
![[Pasted image 20230410103821.png]]
Hay una vulnerabilidad en wordpress de poder ver borradores sin estar registrado, para ello buscamos por google y se nos muestra una web donde dice la vulnerabilidad:
![[Pasted image 20230410103829.png]]
Y aquí tenemos la vulnerabilidad y una pequeña explicación de la misma:
![[Pasted image 20230410103839.png]]
Copiamos esta parte:
![[Pasted image 20230410103847.png]]
Y si pegamos solo la parte del static=1 veremos que nos redirige a una web oculta, tal y como menciona en la web donde buscamos la información por google:
![[Pasted image 20230410103855.png]]
![[Pasted image 20230410103859.png]]
Y tenemos una URL para registrarse de manera secreta:
![[Pasted image 20230410103912.png]]
Entonces ahora tenemos que indicar que la IP apunte a este subdominio, tal y como hicimos antes:
![[Pasted image 20230410103921.png]]
Enton‐ ces ahora si pegamos la dirección que se nos indica en la web, accederemos a un menú de registro:
![[Pasted image 20230410103929.png]]
Nos registramos:
![[Pasted image 20230410103939.png]]
Y ya estamos dentro:
![[Pasted image 20230410103949.png]]
Y dentro de este chat hay un bot que dice que genera acceso a un documento de texto:
![[Pasted image 20230410103958.png]]
Pero en este chat no tengo permiso de escribir, entonces puedo probar en escribirle de manera privada al bot y le digo help para comprobarlo:
![[Pasted image 20230410104009.png]]
Y vemos como este bot tiene un comando que es list para listar los archivos, pero si pongo list ../ voy al directorio anterior:
![[Pasted image 20230410104018.png]]
Ahora que podemos retroceder de directorios, vamos a buscar el archivo etc/passwd, como se trata de un archivo pongo file:
![[Pasted image 20230410104027.png]]
Y podemos acceder al archivo, por lo que estamos ante un local file inclussion:
![[Pasted image 20230410104036.png]]
Pero necesitamos conocer la contraseña, por ejemplo del usuario dwight; para ello vamos a retroceder de directorio:
![[Pasted image 20230410104123.png]]
Ahora miramos la carpeta hubot y nos muestran estos archivos:
![[Pasted image 20230410104134.png]]
De estos archivos es muy interesante el fichero .env porque almacena variables de entorno y muchas veces puede contener credenciales, por lo que lo listamos y ya accedemos a unas credenciales:
![[Pasted image 20230410104204.png]]
Entonces vamos a utilizar esta contraseña para conectarnos por SSH a la máquina:
![[Pasted image 20230410104213.png]]
Y aquí tenemos la flag:
![[Pasted image 20230410104221.png]]
Ahora para escalar nuestros privilegios, podemos utilizar un script que automatiza la búsqueda de vulnerabilidades para hacer la escalada de privilegios llamado linpeas:

Dentro del repositorio tenemos unas instrucciones para bajarnos este script:

Por tanto lo que haremos será descargarlo en nuestra máquina anfitrión y lo compartiremos a la máquina víctima a través del servidor web python y pipearlo directamente desde ahí:

Y ahora lo descargargamos y pipeamos con la máquina víctima utilizando un curl:

Y ahora nos hace el escaneo:

Y en la parte de sudo version vemos que nos lo marca en un rojo como que es muy vulnerable:

20/20

Por lo que vamos a buscar información de esta versión de sudo por internet, y obtenemos cual es la vulnerabilidad pública:

Si buscamos por github esta vulnerabilidad encontramos un repositorio:

Y encontramos este repositorio donde tenemos una herramienta para explotar esta vulnerabilidad: Entramos en el script poc.sh y nos copiamos todo el contenido:

Y lo pegamos dentro de un script que creamos en la máquina víctima

Y le damos permisos de ejecución:

Ahora lo ejecutamos y vemos que funciona:

Ahora dentro del script había una parte donde nos ponía cual era la contraseña por defecto para el usuario que nos iba a crear el exploit:

Entonces iniciamos sesión con este usuario y contraseña y vemos que nos logueamos con un usuario que tiene todos los permisos, donde si hacemos un sudo su nos convertimos en root: