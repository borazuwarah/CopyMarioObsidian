Hacemos los reconocimientos de siempre con nmap:
![[Pasted image 20230421023914.png]]
![[Pasted image 20230421023918.png]]
Vemos el dominio timelapse.htb, por lo que lo añadimos en el etc/hosts:
![[Pasted image 20230421023927.png]]
Como se trata de una máquina windows, vamos a lanzar crackmapexec sobre el puerto smb 445 para obtener información; y vemos que se trata del domain controller[[Crackmapexec]]:
![[Pasted image 20230421023935.png]]
Ahora, como vemos que el puerto 445 smb está abierto, podemos listar los recursos compartidos de esta máquina utilizando la herramienta smbclient haciendo uso de una null session (sin credenciales)[[SMBclient]]:
![[Pasted image 20230421023948.png]]
Pero también tenemos una herramienta que se llama smbmap que nos hace lo mismo pero además mostrando los permisos que tenemos sobre cada uno de estos recursos [[SMBmap]]:
![[Pasted image 20230421023957.png]]
Ahora, con smbclient podemos conectarnos a alguno de estos recursos compartidos, por ejemplo a shares:
![[Pasted image 20230421024005.png]]
Y dentro del directorio /Dev nos encontramos con un fichero .zip que parece ser una copia de seguridad:
![[Pasted image 20230421024013.png]]
Para bajarlo lo hacemos con el comando get:
![[Pasted image 20230421024021.png]]
![[Pasted image 20230421024023.png]]
Y con 7z podemos ver el contenido de este fichero zip, y vemos que se trata de una copia de seguridad de unas credenciales:
![[Pasted image 20230421024031.png]]
Para crackear este zip, tenemos una herramienta que se llama fcrackzip, que debemos ejecutarla de esta manera utilizando por ejemplo el diccionario rockyou sobre el fichero zip objetivo, y vemos que funciona correctamente [[FCRACKZIP - Cracking de Ficheros ZIP]]:
![[Pasted image 20230421024044.png]]
Y ahora ya podemos descomprimir el fichero sin problema:
![[Pasted image 20230421024053.png]]
Ahora el siguiente paso será extraer la clave de este fichero: