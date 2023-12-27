Hacemos los escaneos de siempre con nmap, donde vemos que está abierto el puerto 23, que es el de telnet, por tanto vamos a analizarlo:
![[Pasted image 20230509115328.png]]
![[Pasted image 20230509115331.png]]
Ya que está abierto el puerto telnet, vamos a probar en conectarnos, y vemos que estamos ante algo llamado HP JetDirect:
![[Pasted image 20230509115340.png]]
Lo cual es esto:
![[Pasted image 20230509115349.png]]
Vamos a hacer un escaneo por UDP para ver si nos encuentra algo nuevo, ya que por TCP sólo encontró un puerto. Lo haríamos de esta manera y este sería el resultado, donde vemos que el puerto 161 está abierto:
![[Pasted image 20230509115408.png]]
Ahora vamos a hacer muchas peticiones SNMP con un comando conocido como snmpwalk que sirve para obtener información de SNMP:
![[Pasted image 20231203110750.png]]
Una vez mostrada esta información en hexadecimal, tenemos que convertirla, para ello podemos utilizar el comando xxd, donde obtendremos unas credenciales:
![[Pasted image 20230509115425.png]]
Ahora vamos a probar estas credenciales para iniciar la sesión por telnet:
```python
P@ssw0rd@123!!123�q��"2Rbs3CSs��$4�Eu�WGW�(8i
```
![[Pasted image 20230509115434.png]]
Si ponemos una ? se nos muestran las introducciones, donde pone que podemos ejecutar comandos con exec:
![[Pasted image 20230509115441.png]]
Y ya tenemos la flag:
![[Pasted image 20230509115451.png]]
Ahora que tenemos acceso a esta máquina, vamos a crear una reverse shell y ponernos a escuchar con netcat:
![[Pasted image 20230509115458.png]]
Y lanzamos este comando desde la máquina víctima para enviarnos la terminal:
![[Pasted image 20230509115508.png]]
Y ya estamos:
![[Pasted image 20230509115516.png]]
Para la escalada, usaremos el siguiente exploit procedente el local exploit suggester de metasploit:
![[Pasted image 20231203112435.png]]
Lo ejecutamos y habremos escalado privilegios:
![[Pasted image 20231203112512.png]]
