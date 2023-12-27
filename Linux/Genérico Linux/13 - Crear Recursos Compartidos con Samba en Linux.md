En mi caso voy a instalar Samba en una máquina Ubuntu Server, donde primero dejaré los repositorios actualizados para asegurarme de instalar la última versión de Samba:
![[Pasted image 20230413130612.png]]
Y ahora ya podemos instalar samba con el comando apt install samba:
![[Pasted image 20230413130621.png]]
Ahora voy a crear el usuario mario y docente, donde en mi caso sólo tengo que crear el usuario docente porque el de mario ya está creado:
![[Pasted image 20230413130635.png]]
Y ahora vamos a asignar estos usuarios al servicio SAMBA:
![[Pasted image 20230413130645.png]]
Para listar estos usuarios que hemos añadido a samba, usamos el comando pdbedit con los siguientes parámetros:
![[Pasted image 20230413130658.png]]
Y nos muestra los dos usuarios y con más información relacionada:
![[Pasted image 20230413131809.png]]
Ahora creamos dos carpetas que serán los recursos compartidos utilizando el comando mkdir con el parámetro -p para crear de forma automatizada una carpeta dentro de la otra:
![[Pasted image 20230413131842.png]]
Y lo mismo con las otras carpetas:
![[Pasted image 20230413131852.png]]
Y ya lo tenemos:
![[Pasted image 20230413131859.png]]
### CREACIÓN DE LOS RECURSOS COMPARTIDOS
Vamos a crear un recurso compartido modificando el fichero /etc/samba/smb.cnf para este directorio usando la carpeta de tablon, que cual tendrá sólo permisos de lectura:
![[Pasted image 20230413131946.png]]
Y ahora creamos otro usando la carpeta mario que tenga permisos de lectura y escritorio para el usuario mario, y sólo permiso de lectura al usuario docente:
![[Pasted image 20230413132018.png]]
Y ahora establecemos como dueño de este recurso compartido al usuario mario:
![[Pasted image 20230413132111.png]]
En este punto, si intentamos escribir dentro de este recurso compartido siendo el docente, veremos que no tenemos lo permisos:
![[Pasted image 20230413132120.png]]
Y si queremos mostrar los recursos compartidos podemos usar el comando smbclient -L localhost:
![[Pasted image 20230413132137.png]]
## COMPROBANDO LOS PERMISOS Y CONECTÁNDONOS CON SMBCLIENT
Para acceder a un recurso compartido, puedo hacerlo desde un cliente windows usando el explorador de archivos, escribiendo la siguiente línea:
![[Pasted image 20230413132200.png]]
Ahora me pedirá iniciar sesión, donde puedo hacerlo con cualquiera de los dos usuarios creados anteriormente:
![[Pasted image 20230413132207.png]]
Y escribimos desde este usuario, desde mario:
![[Pasted image 20230413132214.png]]
También podríamos hacer esto mismo desde una máquina Linux (en mi caso desde Kali); y podríamos comprobar lo mismo:
![[Pasted image 20230413132220.png]]
Ahora vamos a acceder como docente con smbclient; y veremos que no tenemos permisos para escribir y sí de lectura, tal y como configuramos antes:
![[Pasted image 20230413132228.png]]
Accedemos de igual forma a este recurso compartido y vemos que no tenemos permisos de escritura:
![[Pasted image 20230413132234.png]]
Y lo mismo ocurre con el usuario mario, que tampoco puede escribir, ya que este recurso compartido es de sólo lectura:
![[Pasted image 20230413132243.png]]
Por último, si quisiera que estos recursos compartidos puedas estar siempre disponible, podríamos utilizar el comando systemctl enable smb para que el servicio samba siempre esté disponible cuando arranquemos nuestro servidor Linux; y a continuación usaremos el comando systemctl restart smb para que se apliquen estos cambios.