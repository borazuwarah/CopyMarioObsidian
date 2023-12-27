Para configurar las redes de Linux, tendremos que modificar el fichero de configuración de netplan, que lo encontramos en la ruta /etc/netplan:
![[Pasted image 20230119203125.png]]
Este fichero con extensión .yaml será el que tengamos que modificar:
![[Pasted image 20230119203207.png]]
Y podremos añadir nosotros la configuración que queramos:
![[Pasted image 20230119203731.png]]
Ahora sólo tenemos que aplicar los cambios con el comando netplan apply:
![[Pasted image 20230119203853.png]]
Y si hacemos un ifconfig ya vemos la nueva configuración:
![[Pasted image 20230119203916.png]]
