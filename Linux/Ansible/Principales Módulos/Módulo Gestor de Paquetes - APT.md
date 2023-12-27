Con este módulo podemos ejecutar el comando APT para instalar paquetes sobre los equipos que queramos administrar. Esto lo haremos realizando la siguiente configuración dentro del playbook y teniendo en cuenta el fichero de configuración de ansible para que no haya errores (utilizamos become: true para ejecutar el comando con permisos de root)
![[Pasted image 20230107134735.png]]
Y para eliminar un programa, lo haríamos de esta forma:
![[Pasted image 20230108113358.png]]
Ahora para aplicar un apt autoremove, haríamos esto:
![[Pasted image 20230108113601.png]]
Para actualizar todos los repositorios, sería de esta forma:
![[Pasted image 20230108113926.png]]
