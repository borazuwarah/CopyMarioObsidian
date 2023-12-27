Haremos el reconocimiento básico con nmap:
![[Pasted image 20221231202557.png]]
![[Pasted image 20221231202613.png]]
Y esta es la web:
![[Pasted image 20221231202644.png]]
Vemos que el whatweb nos dice que por detrás corre algo llamado nostromo 1.9.6, por tanto vamos a buscar vulnerabilidades; y vemos que hay un exploit para la ejecución remota de comandos:
![[Pasted image 20221231202747.png]]
Y este exploit tras probarlo vemos que no funciona bien, pero dentro de él se nos muestra la vulnerabilidad pública, por lo que buscaremos por internet un exploit que funcione correctamente:
![[Pasted image 20221231202759.png]]
Y nos encontramos con este exploit que permite la ejecución remota de comandos:
![[Pasted image 20221231202808.png]]
Nos lo clonamos y lo ejecutamos tal y como se nos muestran en las instrucciones, y vemos que tenemos ejecución remota de comandos:
![[Pasted image 20221231202817.png]]
Pero vemos que no podemos ni movernos por los directorios ni ejecutar una reverse shell:
![[Pasted image 20221231202829.png]]
  
Pero sí podemos leer archivos y por ejemplo leemos el etc/passwd y vemos que hay un usuario llamado david:
![[Pasted image 20221231202846.png]]
![[Pasted image 20221231202850.png]]
Pero sin embargo no podemos acceder al directorio para obtener la clave pública ssh:
![[Pasted image 20221231202901.png]]
Pero sí podemos acceder a los archivos de configuración del servidor nostromo y se nos muestra mucha información:
![[Pasted image 20221231202911.png]]
Y se nos muestran unas credenciales:
![[Pasted image 20221231202920.png]]
![[Pasted image 20221231202924.png]]
Ahora tenemos esta contraseña hasheada, por lo que tendremos que deshashearla con john the ripper de esta manera:[[John The Ripper]]
![[Pasted image 20221231202935.png]]
Ahora que tenemos esta password, tenemos que encontrar donde poder utilizarla, ya que vemos que por ssh no nos funciona.
