Esto sirve para automatizar la ejecución de muchos comandos en los distintos servidores; es como una receta o un script donde se guardan los distintos comandos a ejecutar y en qué servidores. Por tanto creamos el fichero con extensión yml:
![[Pasted image 20230106084145.png]]
Y a este fichero le pondremos esta estructura para hacer un ping a todas las máquinas dentro del grupo servers, y luego crear dos archivos y hacer un ping:
![[Pasted image 20230106084400.png]]
Y ahora para lanzar este playbook a la máquina o al grupo de máquinas que estén dentro del grupo servers, debemos ejecutar este comando:
![[Pasted image 20230106084540.png]]
Y ahora vemos que en la máquina administrada se crearon correctamente estos dos archivos dentro del actual directorio de trabajo:
![[Pasted image 20230106084650.png]]
En caso de querer ejecutar un comando en un solo equipo, debemos especificarlo en el playbook de esta manera:
![[Pasted image 20230106084909.png]]

