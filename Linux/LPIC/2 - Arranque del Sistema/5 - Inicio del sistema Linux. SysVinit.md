Una vez cargado el gestor de arranque y todo listo, es cuando se inicia el demonio init, que se encargará de ejecutar todo lo demas y el resto de procesos para que el sistema pueda arrancar.

---------------------

Tenemos 3 tipos de procesos que se encargan de iniciar y gestonar los demonios en Linux:

SysVinit --> Utiliza scripts y niveles de ejecución para controlar el inicio, apagado y gestión de los procesos del sistema. (ya no se encuentra en sistemas modernos)
Systemd --> Desde el 2015 fue adoptado por parte de las distribuciones Linux como su sistema de inicio.
Upstar --> Utiliza eventos para gestionar el arranque o parada de los procesos.
![[Pasted image 20230819163830.png]]
