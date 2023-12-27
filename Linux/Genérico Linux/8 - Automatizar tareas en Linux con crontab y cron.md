Para comprobar que el servicio cron está ejecutándose, utilizaremos el comando systemctl status cron:
![[conocimiento/conocimiento/images/Untitled 7.png]]
Para listar las tareas programadas por el usuario (en este caso usuario mario), podemos ejecutar el comando crontab -l:
![[Untitled 1 4.png]]
Para programar una nueva tarea, utilizamos el comando crontab -e; y se nos abre el editor de texto:
![[Untitled 2 4.png]]
Para ejecutar una tarea programada que consista en abrir el navegador y la web de [www.foc.es](http://www.foc.es/), podemos utilizar el comando env DISPLAY=:0 firefox [www.foc.es](http://www.foc.es/). Por tanto, si escribimos este comando dentro de crontab, se nos abrirá el firefox con dicha url, para que se abra a cada minuto de todos los días:
![[Untitled 3 2.png]]
Ahora, en este punto, tras esperar un minuto, se abre el firefox con la url de [www.foc.es](http://www.foc.es/):
![[Untitled 4 2.png]]
Ahora, vamos a añadir a un nuevo usuario llamado “aso” para quitarle permisos de que pueda ejecutar tareas en crontab:
![[Untitled 5 1.png]]
Para configurar que el usuario “aso” no pueda programar tareas en crontab, debemos crear los ficheros /etc/cron.allow y deny para poder regular esto:
![[Untitled 6 1.png]]
Y ahora añadimos dentro de cron.deny al usuario “aso”:
![[Untitled 7 1.png]]
![[Untitled 8.png]]
Y ahora si desde este usuario intentamos acceder a algo de crontab, veremos que no tenemos permisos para ello:
![[Untitled 9.png]]
Por último, si eliminásemos estos ficheros, todos los usuarios podrían acceder sin problemas a la configuración de crontab y podrían programar tareas.