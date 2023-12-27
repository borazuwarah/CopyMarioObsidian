Haremos los escaneos de nmap de siempre:
![[Pasted image 20230318091055.png]]
Cuando estamos ante una máquina Active Directory, debemos obtener información a nivel de dominio; y para ello podemos utilizar enum4linux:
![[Pasted image 20230318091602.png]]
Y ya hemos encontrado el nombre del dominio, que se llama WINDCORP:
![[Pasted image 20230318091641.png]]
Por tanto vamos a añadir este dominio al fichero /etc/host, para que funcione la resolución DNS:
![[Pasted image 20230318091804.png]]
Dentro del puerto 593 pudimos ver en nmap que corría un servicio web HTTP, por lo que vamos a probar en acceder:
