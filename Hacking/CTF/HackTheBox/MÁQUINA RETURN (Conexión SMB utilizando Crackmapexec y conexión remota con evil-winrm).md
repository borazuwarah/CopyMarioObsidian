Lo primero será hacer un PING para comprobar si estamos ante una máquina Windows o Linux fijándonos en el TTL, que en este caso estamos ante un Windows porque su TTL es 127:
![[Pasted image 20230509120323.png]]
Ahora, tras hacer el escaneo de puertos con nmap, vemos que todos estos están abiertos:
![[Pasted image 20230509120336.png]]
![[Pasted image 20230509120340.png]]
Y en el resultado podemos ver que el puerto 445 está abierto, que este es el puerto SMB:
![[Pasted image 20230509120351.png]]
Por tanto vamos a inspeccionar este puerto con una herramienta llamada crackmapexec, donde ya sabemos que es un Windows 10, se llama PRINTER y tiene el servicio SMB firmado:
![[Pasted image 20230509120400.png]]
Ahora vamos a entrar a la web desde el navegador, ya que tiene abierto el puerto 80:
![[Pasted image 20230509120407.png]]
Y en la parte de settings tenemos este menú, donde vemos que la web se conecta a un servidor por el puerto 389:
![[Pasted image 20230509120417.png]]
Aquí podemos cambiar el servidor y poner nuestro ordenador, mientras nos ponemos en escucha con netcat por el puerto 389:
![[Pasted image 20230509120426.png]]
![[Pasted image 20230509120430.png]]
Y ahora si pulsamos en update, tendremos la conexión donde se nos entrega una password:
![[Pasted image 20230509120439.png]]
Y ahora con crackmapexec podemos comprobar si esta credencial es correcta si se nos pone el signo +:
![[Pasted image 20230509120446.png]]
Vamos a conectarnos a través de winrm (administración remota de windows), ya que con crackmapexec vemos que también podemos entrar:
![[Pasted image 20230509120454.png]]
Pues ahora vamos a explotar la conexión winrm con una herramienta que se llama evil-winrm, donde la instalaré de esta manera:
![[Pasted image 20230509120501.png]]
Y ahora ejecutamos este comando para conectarnos por winrm:
![[Pasted image 20230509120516.png]]
![[Pasted image 20230509120520.png]]
Ahora si vamos al escritorio de la máquina víctima, tenemos un bloc de notas:
![[Pasted image 20230509120528.png]]
Para leerla usamos el comando type, similar al comando cat de linux:
![[Pasted image 20230509120537.png]]