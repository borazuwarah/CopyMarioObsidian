## (Recursos Compartidos de SMB y listar recursos compartidos con smbmap y desencriptar contraseñas con gpp-decrypt - fichero groups.xml, smbclient, smbmap y kerberoasting con TGS)
Lo primero que haremos serán los reconocimientos de siempre con nmap:
![[Pasted image 20230116154608.png]]
Vemos que tiene abierto el puerto 445, que es el puerto smb, por tanto vamos a usar crackmapexec para analizar dicho puerto:
![[Pasted image 20230116154627.png]]
Estamos viendo un dominio, por tanto vamos a apuntarlo en el /etc/hosts:
![[Pasted image 20230116154637.png]]
Ahora vamos a comprobar si el servicio smb tiene recursos compartidos a nivel de red, para ello lo hacemos con smbclient:
![[Pasted image 20230116154651.png]]
Ahora vamos a ver a cuales de estos recursos compartidos me puedo conectar; y para ello puedo utilizar smbmap; y vemos que podemos conectarnos a Replication:
![[Pasted image 20230116154703.png]]
Y nos conectamos a Replication con este otro comando, ya que este dominio podemos leerlo:
![[Pasted image 20230116154716.png]]
Y dentro de la carpeta active.htb tenemos esto:
![[Pasted image 20230116154724.png]]
Una vez en este punto, debemos de buscar un archivo llamado groups.xml, porque aquí se suelen guardar credenciales, el cual podemos encontrarlo dentro de esta ruta:
![[Pasted image 20230116154735.png]]
Por tanto vamos a descargar el archivo con el parámetro –download, y le vamos a llamar groups.xml:
![[Pasted image 20230116154747.png]]
Y si hacemos un cat de lo que hay en este fichero, veremos que se esconde una contraseña y el usuario SVC_TGS:
![[Pasted image 20230116154803.png]]
Ahora podemos utilizar una herramienta que se llama gpp-decrypt que nos permite desencriptar esta contraseña:
![[Pasted image 20230116154823.png]]
Ahora vamos a ir probando con smbmap si con esta contraseña y alguno de los usuarios obtenidos en el fichero groups.xml podemos conectarnos a más recursos compartidos; y vemos que podemos conectarnos a bastantes más:
![[Pasted image 20230116154841.png]]
Ahora que tenemos estas credenciales en texto claro, con crackmapexec podemos comprobar si estas credenciales son correctas; y vemos que sí:[[Crackmapexec#Comprobar y validar credenciales con crackmapexec]]
![[Pasted image 20230116160210.png]]
Entonces con estas credenciales válidas podemos listar recursos compartidos con crackmapexec; y podemos listar archivos dentro de varias ubicaciones, entre las que tenemos la de users: [[Crackmapexec#Listar recursos compartidos con crackmapexec]]
![[Pasted image 20230116160349.png]]
Ahora con smbmap veremos qué tenemos estas ubicaciones:
![[Pasted image 20230116160528.png]]
Vamos dentro del usuario SVC_TGS y al escritorio, donde vemos la flag de user:
![[Pasted image 20230116160632.png]]
Nos la descargamos:
![[Pasted image 20230116160719.png]]
Ahora vamos a comprobar si esta máquina o este usuario es vulnerable a un Kerberoasting, ya que este ataque consiste en capturar las credenciales del sistema, para ello si ponemos el dominio seguido del usuario para iniciar sesión en el sistema (que es ADMINISTRADOR), podremos obtener credenciales en caso de existir.

Lo primero será tener sincronizado el reloj con la misma hora que el domain controller, y para ello tenemos una herramienta que se llama ntpdate:

![[Pasted image 20230126103119.png]]

Para efectuar este ataque debemos tener credenciales de un usuario y su contraseña, para poder comprobar si podemos solicitar un TGS con este comando:

![[Pasted image 20230126104404.png]]

Entonces viendo que podemos solicitar un TGS con las credenciales de este usuario, podemos ejecutar el mismo comando pero pasando la opción -request:

![[Pasted image 20230126104553.png]]

Por tanto una vez obtenido este hash, podemos tratar de crackearlo para que nos proporcione la contraseña del usuario administrador. Por lo que vamos a copiar todo este hash y lo vamos a guardar en un documento .txt:

![[Pasted image 20230126104909.png]]
Lanzamos el ataque con john para crackear este hach y obtenemos la contraseña:
![[Pasted image 20230126105336.png]]
Ahora vamos a validar con crackmapexec que esta contraseña sirva para el usuario administrador; y vemos que sí: [[## Comprobar y validar credenciales con crackmapexec]]
![[Pasted image 20230126105512.png]]
Por tanto ahora vamos a conectarnos de manera remota con estas credenciales utilizando psexec.py y conseguimos una cmd como el usuario Administrator (hay que tener en cuenta que psexec funciona cuando el usuario tiene altos privilegios como en este caso):
![[Pasted image 20230126105848.png]]
Una vez dentro obtenemos la flag de user:
![[Pasted image 20230126110129.png]]
Y ahora la de root:
![[Pasted image 20230126110230.png]]

