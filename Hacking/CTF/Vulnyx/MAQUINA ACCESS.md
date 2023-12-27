Hacemos el reconocimiento con nmap:
![[Pasted image 20231126114645.png]]
Y vemos que dentro del puerto 6666 hay un websocket, los cuales se utilizan a menudo en aplicaciones web en tiempo real, como chat, juegos y aplicaciones de transmisión de video. También se pueden utilizar en aplicaciones de escritorio y móviles, por lo que nos conectamos con el comando wscat:
```
wscat -c ws://192.168.0.30:6666
```
![[Pasted image 20231126115124.png]]
Y encontramos el usuario noname, lo cual significa que podemos hacer un ataque de fuerza bruta al puerto 5555 que estaba abierto con un servicio ftp:
```bash
noname:phoenix
```
![[Pasted image 20231126115744.png]]
Accedemos vía ftp y funciona correctamente, donde además vemos un archivo index.html:
![[Pasted image 20231126115602.png]]
Y vemos que ese archivo html se corresponde con el contenido del puerto 80:
![[Pasted image 20231126115727.png]]
Vamos a subir una webshell, por ejemplo la de pentestmonkey:
![[Pasted image 20231126115923.png]]
Y la cargamos:
![[Pasted image 20231126120015.png]]
Nos ponemos en escucha con netcat por el puerto 443 y llamamos a la webshell desde el navegador, pero vemos que no se ejecuta el código, sino que se descarga, debido a que existe un filtro:
![[Pasted image 20231126120217.png]]
Nos informamos en hacktricks y vemos otras posibles extensiones:
```
https://book.hacktricks.xyz/pentesting-web/file-upload
```
![[Pasted image 20231126120358.png]]
Por lo que vamos probando en renombrar el archivo, hasta que vemos que con php5 funciona correctamente:
![[Pasted image 20231126120533.png]]
![[Pasted image 20231126120544.png]]
![[Pasted image 20231126120553.png]]
Y vemos que podemos ejecutar perl como el usuario noname:
![[Pasted image 20231126120934.png]]
Vemos las instrucciones en gtfobins:
![[Pasted image 20231126121052.png]]
Por lo que ejecutamos perl como el usuario noname y pivotamos a dicho usuario:
```
sudo -u noname perl -e 'exec "/bin/bash";'
```
![[Pasted image 20231126121159.png]]
Y con el comando sudo -l nuevamente vemos que podemos ejecutar como el usuairo powerful el binario rev:
![[Pasted image 20231126121316.png]]
Viendo las instrucciones de gtfobins vemos que podemos leer archivos como el usuario powerful:
![[Pasted image 20231126121408.png]]
Por lo que hacemos la prueba con el archivo bash_history:
```
LFILE=/home/powerful/.bash_history
sudo -u powerful rev $LFILE | rev
```
![[Pasted image 20231126121549.png]]
Probamos estas credenciales y funciona:
```
powerful:Th3_p0w3R_of_H4ck1nG
```
![[Pasted image 20231126121617.png]]
Probamos en entrar por ssh y vemos que tenemos acceso:
![[Pasted image 20231126121754.png]]
Y tenemos este otro binario siendo el usuario powerful:
![[Pasted image 20231126121854.png]]
