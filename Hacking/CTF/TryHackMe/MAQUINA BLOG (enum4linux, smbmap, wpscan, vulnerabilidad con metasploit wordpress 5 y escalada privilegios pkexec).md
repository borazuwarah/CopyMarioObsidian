Hacemos el escaneo de nmap y nos encontramos con estos puertos abiertos:
![[Pasted image 20230406073923.png]]
![[Pasted image 20230406074029.png]]
![[Pasted image 20230406074056.png]]
Podemos lanzar enum4linux para conocer más información y usuarios del dominio:
![[Pasted image 20230406074309.png]]
Y nos muestra un recurso compartido llamado BillySMB:
![[Pasted image 20230406074344.png]]
Aunque también podemos detectar recursos compartidos con smbmap:
![[Pasted image 20230406074550.png]]
Podemos conectarnos a este recurso compartido con smbclient sin proporcionar contraseña:
![[Pasted image 20230406075618.png]]
Pero todo este recurso compartido resulta ser un rabbit hole. Por lo que tenemos que investigar el puerto 80, donde nos encontramos con esta página web:
![[Pasted image 20230404130710.png]]
Si hacemos fuzzing nos encontrramos con estos directorios:
![[Pasted image 20230404130839.png]]
![[Pasted image 20230404130850.png]]
No obstante, para que la web nos funcione mejor vamos a añadirla al /etc/hosts:
![[Pasted image 20230410094544.png]]
Y la web ya nos carga bien:
![[Pasted image 20230410094601.png]]
Y ahora el fuzzing nos habrá encontrado más cosas:
![[Pasted image 20230410094646.png]]
Ahora que vemos que se trata de un wordpress, podemos utilizar la herramienta wpscan [[conocimiento/conocimiento/Hacking/Hacking Generico/Hacking Web/Hacking WordPress|Hacking WordPress]]:
```bash
wpscan --url http://10.10.123.131:80 -e vp,u
```
Y vemos que nos encuentra varios usuarios:
![[Pasted image 20230507111927.png]]
Por tanto, podemos hacer un ataque de fuerza bruta contra el xmlrpc utilizando wpscan, con los siguientes parámetros:
```bash
wpscan --url http://10.10.123.131:80 -U kwheel -P rockyou.txt
```
![[Pasted image 20230507112321.png]]
Por tanto iniciamos sesión dentro del wp-admin y vemos que nos funciona:
![[Pasted image 20230507112410.png]]
En este punto estamos viendo que la versión de wordpress es la 5.0, la cual es vulnerable a una remote code execution, por lo que miramos en metasploit y vemos que existe:
![[Pasted image 20230507113232.png]]
A continuación configuramos todas las opciones dentro de metasploit:
![[Pasted image 20230507113323.png]]
Y vemos que funciona, aunque nos da una sesión de meterpreter, por lo que para mayor comodidad podemos lanzar una bash:
![[Pasted image 20230507113522.png]]
![[Pasted image 20230507113533.png]]
Podemos ver los binarios SUID y nos encontramos con pkexec:
![[Pasted image 20230507113855.png]]
Por lo que buscamos un exploit para explotar esta vulnerabilidad y lo ejecutamos en la máquina víctima:
![[Pasted image 20230507113929.png]]
Lo compartimos con la máquina víctima, lo ejecutamos y ya somos root:
![[Pasted image 20230507114002.png]]
Por último, para encontrar la flag de user, simplemente podemos usar el comando find y ya la habremos encontrado:
![[Pasted image 20230507114726.png]]
