En este fichero podemos establecer los distintos hosts que queramos administrar de manera automatizada; pero tenemos que crear una carpeta llamada ansible dentro del directorio /etc; y ahí insertar el fichero hosts:
![[Pasted image 20230106080513.png]]
Y ahora debemos añadir estas líneas, donde pondremos primero las IP de aquellos equipos que queramos administrar, y luego abajo ponemos para que siempre se utilice la última versión de Python:
![[Pasted image 20230106080859.png]]
Y ahora podemos probar en lanzar un ping al equipo que tenemos añadido dentro del fichero hosts; y si vemos algo como esto es que tenemos conectividad:
![[Pasted image 20230106081118.png]]
Si tuviera configurado otro grupo de servidores, podría mandarlo a todos poniendo dos puntos:
![[Pasted image 20221217101938.png]]
También puedo mandar el ping a un sólo servidor, sin especificar ningún grupo:
![[Pasted image 20221217101946.png]]
También podemos lanzar un comando cualquiera utilizando el módulo shell, por ejemplo crear un directorio en el actuar espacio de trabajo dentro del servidor, utilizando el usuario mario o root, aunque para ello debemos haber configurado correctamente el usuario root dentro de la parte de "Fichero de configuración automático"
![[Pasted image 20230106081238.png]]
Y vemos que el fichero se creó correctamente en el servidor:
![[Pasted image 20230106081305.png]]
