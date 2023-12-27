Hacemos el reconocimiento con nmap:
```bash
nmap -p- --open -sS -sC -sV --min-rate 5000 -n -vvv -Pn 192.168.0.43 -oN escaneo
```
![[Pasted image 20230723111912.png]]
Y esto es lo que nos encontramos en el puerto 8000:
![[Pasted image 20230723111951.png]]
Como vemos esto de escuchar, probamos en hacer sniffing con wireshark para ver qué encontramos, lo que vemos un id_rsa:
![[Pasted image 20230723112416.png]]
Hacemos clic derecho sobre este texto y seleccionamos la opción as a printable text:
![[Pasted image 20230723112543.png]]
Y estamos guardando parte del id_rsa:
![[Pasted image 20230723112611.png]]
Y luego si vemos otro paquete, vemos el final:
![[Pasted image 20230723112725.png]]
Ahora al editar el contenido, tenemos que quitar esta línea:
![[Pasted image 20230723112837.png]]
Así nos debería de quedar:
![[Pasted image 20230723112904.png]]
Vamos a crackear el hash:
```bash
ssh2john id_rsa > hash
```
![[Pasted image 20230929083853.png]]
Tenemos una contraseña pero no tenemos el usuario, por lo que podemos fijarnos en el reporte de nmap y descubrir cual es el usuario ssh ya que es una versión algo antigua:
![[Pasted image 20230929084023.png]]
![[Pasted image 20230929084041.png]]
Y nos bajamos un diccionario con nombres de usuario:
```bash
https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/Names/names.txt
```
Y lanzamos el ataque para ver si encontramos algún usuario válido:
![[Pasted image 20230929084413.png]]
```bash
python3 CVE-2018-15473.py -p 22 192.168.0.82 -w names.txt
```
Y vemos que el usuario abel es usuario válido:
![[Pasted image 20230929084501.png]]
```bash
abel:idontknow
```
Iniciamos sesión por ssh con estas credenciales usando este id_rsa:
![[Pasted image 20230929084731.png]]
Una vez dentro, si ejecutamos el comando crontab -l vemos que hay un comando que se ejecuta de forma automatizada:
![[Pasted image 20230929085226.png]]
Y como vemos que se está ejecutando el comando cp como root, podemos probar en explotar un path hijacking abusando del comando cp:
[[8 - Manipular el PATH - Path Hijacking]]
```bash
nano cp
chmod 777 cp
export PATH=.:$PATH
bash -p
```
Por lo que creamos un archivo llamado cp con el siguiente contenido para cambiarle el permiso a la bash:
![[Pasted image 20230929085600.png]]
