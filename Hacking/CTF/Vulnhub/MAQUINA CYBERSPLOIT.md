## (Decodificar base64, Inspeccionar código fuente de la página, convertir archivo en binario, escalada de privilegios en Ubuntu 12.04 con exploit de exploitdb overlayfs y compilar exploit en .c con el comando gcc)

Lo primero vamos a activar en virtualbox la opción de permitir mvs (máquinas virtuales) y vamos a lanzar un arp-scan para detectar la máquina víctima: [[0 - Consideraciones Previas#DETECTAR EQUIPOS CONECTADOS A MI RED CON ARP-SCAN]]
![[Pasted image 20230322012544.png]]
Ahora vamos a hacer un escaneo con nmap:
```python
nmap -p- --open -sS -sC -sV --min-rate 5000 -vvv -n -Pn 192.168.0.21 -oN escaneo
```
Y nos encuentra el puerto 22 y 80 abiertos:
![[Pasted image 20230322012711.png]]
Ya que tiene el puerto 80 abierto, vamos a ver la web y es esta:
![[Pasted image 20230322012832.png]]
Vamos a hacer fuzzing a esta página web y nos encontramos con distintos directorios, entre los cuales tenemos el de robots.txt, el cual es muy interesante:
![[Pasted image 20230322013132.png]]
Accedemos dentro de esta ubicación y nos encontramos con un texto codificado (posiblemente en base64):
![[Pasted image 20230322013223.png]]
Si lo decodificamos nos encontramos con la primera flag:
![[Pasted image 20230322013246.png]]
También mirando el código fuente de la página nos encontramos con un username:
![[Pasted image 20230322013331.png]]
Por tanto, teníamos abierto el puerto 22 SSH y tenemos un usuario válido, por lo que podemos probar en introducir el este usuario y la contraseña ponemos la flag 1:
```javascript
user --> itsskv
password --> cybersploit{youtube.com/c/cybersploit}
```
Probamos estas credenciales y funcionan:
![[Pasted image 20230322015602.png]]
Y aquí visualizamos la flag 2 pero en este formato binario:
![[Pasted image 20230322015636.png]]
Si esto lo pasamos por chatgpt nos dice lo que es y nos lo convierte a la flag2:
![[Pasted image 20230322015818.png]]
Ahora, vamos a ver la versión de ubuntu que estamos utilizando, y vemos que es la 12.04, para ello podemos usar lsb_release -a: [[Escalada de Privilegios en Ubuntu 12.04]]
![[Pasted image 20230322020833.png]]
Podemos buscar por internet algún exploit para esta versión de ubuntu, y nos encontramos con un exploit de la web de Exploit-DB:
![[Pasted image 20230322021046.png]]
![[Pasted image 20230322021100.png]]
Nos lo bajamos y lo compartimos con la máquina víctima:
![[Pasted image 20230322021119.png]]
![[Pasted image 20230322021131.png]]
![[Pasted image 20230322021150.png]]
Ahora que ya lo tenemos dentro de la máquina víctima, simplemente tenemos que compilarlo para poder ejecutarlo; y por tanto utilizamos el comando gcc -o escalada 37292.c:
![[Pasted image 20230322021240.png]]
Y ahora sólo tenemos que ejecutar el script de ./escalada y ya somos usuarios root:
![[Pasted image 20230322021308.png]]
Y aquí tenemos la flag de root:
![[Pasted image 20230322021350.png]]
![[Pasted image 20230322021406.png]]

