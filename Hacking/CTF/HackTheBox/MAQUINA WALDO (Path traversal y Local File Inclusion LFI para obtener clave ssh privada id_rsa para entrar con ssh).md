Hacemos los reconocimientos de siempre:
![[29-1.png]]
Y en el servidor web hacemos un whatweb y vamos a la página:
![[29-2.png]]
![[29-3.png]]
En esta web podemos añadir campos, y si probamos en inyectar código HTML vemos que lo interpreta:
![[29-4.png]]
![[29-5.png]]
Vamos a ver cómo funciona esta petición por burpsuite, y vemos algo de path donde se ve que la web busca por los directorios de la máquina:
![[29-6.png]]
Pero si probamos en hacer un path traversal, vemos que nos lo limita y no va para atrás, por tanto podemos probar en hacer un ….//….//….//, de esta manera estamos rompiendo con la limitación y podremos retroceder directorios sin problema:
![[29-7.png]]
Pero vemos que no podemos ver el fichero passwd ni el contenido de los archivos, solo podemos de directorios, ya que la petición se tramita como /dirRead:
![[29-8.png]]
Pero para listar los archivos, se puede probar en hacer un click en donde pone list en la web:
![[29-9.png]]
Y entonces ahora la petición que nos intercepta el burpsuite es diferente, donde se puede ver que utiliza un archivo ya que la petición se tramita como /fileRead.php:
![[29-10.png]]
Y si aquí aplicamos el path traversal para ver el fichero, vemos que funciona; y esto significa que es vulnerable a Local File Inclusion LFI:
![[29-11.png]]
Vemos que hay un usuario que se llama nobody, que en la parte de path podemos ver los directorios de este usuario:
![[29-12.png]]
Y en la pestaña de file vemos que tenemos una clave privada de ssh, por lo que la vamos a guardar:[[Acceder id_rsa para iniciar sesión por SSH a la máquina]]
![[29-13.png]]
![[29-14.png]]
Le damos el permiso 600 a la id_rsa:
![[29-15.png]]
Pero si queremos obtener la id_rsa en formato correcto, lo obtendremos a través de curl y aplicando una tubería con el siguiente filtro:
![[29-16.png]]
Y ahora si iniciamos sesión con ssh utilizando este id_rsa vemos que ya estamos dentro:
![[29-17.png]]
