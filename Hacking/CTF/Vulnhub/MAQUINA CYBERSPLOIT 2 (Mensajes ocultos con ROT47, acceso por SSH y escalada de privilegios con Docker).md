El primer paso será ejecutar el arp-scan para detectar la máquina dentro de nuestra red: [[0 - Consideraciones Previas#DETECTAR EQUIPOS CONECTADOS A MI RED CON ARP-SCAN]]
![[Pasted image 20230322131430.png]]
Vemos que el ttl es el de Linux, por lo que ya encontramos la máquina:
![[Pasted image 20230322131507.png]]
Ahora lanzamos un nmap y vemos el puerto 22 y 80 abiertos:
![[Pasted image 20230322131608.png]]
Vemos la web en el navegador ya que vemos que tiene abierto el puerto 80:
![[Pasted image 20230322131711.png]]
Si miramos el código fuente de la página nos encontramos con algo interesante, y es un usuario anonymous y un texto en formato sha384:
![[Pasted image 20230322132333.png]]
Y también abajo del todo nos encontramos con algo que pone ROT47:
![[Pasted image 20230322133759.png]]
Y si buscamos por internet esto de ROT47 [[ROT47 - Sistema para Ocultar Contenido de un Mensaje]] nos encontramos con que se trata de un cifrado simple que se utiliza para ocultar el contenido de un mensaje; y tenemos herramientas online para descifrar estos mensajes:
![[Pasted image 20230322210657.png]]
Por tanto entramos en la página de rot47.net y podemos probar en poner uno de los textos que nos salía antes en la web:
![[Pasted image 20230322210740.png]]
Pegamos por ejemplo la parte de la derecha en la web:
![[Pasted image 20230322210806.png]]
Y si hacemos clic en ROT47 nos devuelve un usuario shailendra:
![[Pasted image 20230322210828.png]]
Y ahora hacemos lo mismo con el otro dato de la web cybersploit1:
![[Pasted image 20230322210856.png]]
Ahora si intentamos entrar por SSH con estas creenciales, vemos que funciona:
![[Pasted image 20230322211007.png]]
Una vez dentro de la máquina, si ejecutamos el comando ifconfig y si miramos el contenido del fichero hint.txt, podemos ver pistas sobre docker, donde tenemos un tutorial de cómo explotar esta vulnerabilidad [[4 - Escalada de Privilegios en Docker que no pide ser Usuario Sudo]]:
![[Pasted image 20230322211444.png]]
![[Pasted image 20230322211455.png]]
Por tanto vamos a buscar información por internet de cómo escalar privilegios en docker; y nos encontramos con este repositorio de github de hacktricks:
![[Pasted image 20230322211824.png]]
Y en este repositorio nos dan unas instrucciones de unos comandos que tenemos que ejecutar para convertirnos en root en la máquina víctima con el docker instalado:
![[Pasted image 20230322211909.png]]
Por tanto ejecutamos los comandos:
```python
docker run -it -v /:/host/ ubuntu:18.04 chroot /host/ bash

docker run -it --rm --pid=host --privileged ubuntu bash

nsenter --target 1 --mount --uts --ipc --net --pid -- bash

docker run -it -v /:/host/ --cap-add=ALL --security-opt apparmor=unconfined --security-opt seccomp=unconfined --security-opt label:disable --pid=host --userns=host --uts=host --cgroupns=host ubuntu chroot /host/ bash
```
Una vez ejecutados estos comandos, nos convertirmos en usuarios root:
![[Pasted image 20230322212042.png]]
![[Pasted image 20230322212022.png]]
