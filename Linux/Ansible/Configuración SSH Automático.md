PASO 1 - Copiar llaves SSH para que no se nos pida siempre ingresar la contraseña a los servidores que queramos automatizar su control:
![[Pasted image 20221217101732.png]]
Y ya tenemos un par de claves SSH dentro del directorio .ssh:
![[Pasted image 20221217101740.png]]
Ahora tengo mi máquina anfitrión de KDE o Kali Linux y mi máquina Ubuntu que voy a administrar con ansible:
![[Pasted image 20221217101748.png]]
Ahora vamos a importar esta clave SSH al Ubuntu con este comando:
![[Pasted image 20221217101757.png]]
Ahora ya puedo entrar en el servidor sin necesitad de proporcionar ninguna contraseña, por ejemplo si ejecuto el comando de ssh directamente podremos entrar sin problemas:
![[Pasted image 20221217101804.png]]
Ahora por último tenemos que configurar que también podamos conectarnos como usuario root a través de SSH; y para ello debemos editar el fichero de configuración /etc/ssh/sshd_config y cambiar a "yes" el apartado de PermitRootLogin yes:
![[Pasted image 20230106081754.png]]
Y ahora dentro de los equipos que queramos administrar tenemos que establecer una nueva contraseña para root:
![[Pasted image 20230106082205.png]]
Y ya podremos conectarnos como root en ssh:
![[Pasted image 20230106082229.png]]
Y ahora por último vamos a hacer que la conexión sea automática al igual que ocurre si usamos el usuario mario:
![[Pasted image 20230106082354.png]]
Y ya podremos conectarnos de forma automática tanto con el usuario mario como con el usuario root dentro de este equipo:
![[Pasted image 20230106082522.png]]
