Lo primero será hacer los reconocimientos de siempre:
![[Pasted image 20230222130856.png]]
![[Pasted image 20230222130915.png]]
Si abrimos el navegador, esta es la web:
![[Pasted image 20230222130923.png]]
Vemos en el whatweb que nos muestra un dominio de doctor.htb, por lo que lo añadimos al etc/hosts:
![[Pasted image 20230222130938.png]]
Y vemos que ya nos reconoce la ruta:
![[Pasted image 20230222130954.png]]
Y por el puerto 8089 tenemos esto:
![[Pasted image 20230222131003.png]]
Por otra parte, si lo ponemos en plural “doctors.htb” nos entra en un panel de registro y login, por lo que vamos a registrarnos:
![[Pasted image 20230222131015.png]]
Una vez nos registramos, ya tenemos cuenta:
![[Pasted image 20230222131022.png]]
Y tenemos un apartado de New Message, donde podremos intentar un SSTI, por tanto lo intentamos:
![[Pasted image 20230222131030.png]]
Y vemos que aparentemente no se ejecuta, pero podemos mirar en el código html por detrás y veremos cómo si aparece el 49:
![[Pasted image 20230222131041.png]]
Y nos aparece este comentario diciendo que hay un directorio llamado archive:
![[Pasted image 20230222131050.png]]
Y en este directorio es donde si podemos ver el 49 y que por tanto es vulnerable a SSTI:
![[Pasted image 20230222131057.png]]
Ahora que vemos que es vulnerable a SSTI, vamos a ir al repositorio de payloadallthethings y encontramos este código de SSTI:
![[Pasted image 20230222131109.png]]
Por tanto vamos a ejecutar esto pero en vez de con un id, mejor ponemos por ejemplo un ls -l para listar los archivos:
![[Pasted image 20230222131118.png]]
Y vemos que por detrás, en el html sí funciona:
![[Pasted image 20230222131144.png]]
Ahora que tenemos ejecución remota de comandos, vamos a ganar una reverse shell, por tanto montamos un servidor de python con el código para enviarnos la shell reversa:
![[Pasted image 20230222131203.png]]
![[Pasted image 20230222131210.png]]
Y ahora desde la máquina víctima ejecutamos el curl para obtener este código e interpretarlo con bash:
![[Pasted image 20230222131222.png]]
Escuchamos con netcat y recibimos la conexión:
![[Pasted image 20230222131230.png]]Pero no tenemos permisos para leer la flag:
![[Pasted image 20230222131240.png]]
Ahora si miramos dentro del directorio /var/logs/ apache2, hay un directorio llamado backup que esconde unas credenciales del usuario shaun:
![[Pasted image 20230222131254.png]]
![[Pasted image 20230222131259.png]]
![[Pasted image 20230222131304.png]]
Si ejecutamos un su shaun y ponemos de clave guitar123 ya somos el usuario shaun:
![[Pasted image 20230222131311.png]]
Ahora ejecutamos el comando para obtener una pseudo-consola y listo:
![[Pasted image 20230222131325.png]]
Y ya tenemos la flag:
![[Pasted image 20230222131333.png]]
