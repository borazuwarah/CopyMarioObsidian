Hacemos el reconocimiento con nmap y vemos que el puerto 22 está abierto pero además en el puerto 30024 está corriendo un servicio ftp:
![[Pasted image 20230626194412.png]]
Y podemos acceder vía FTP como anonymous, y nos encontramos además con un id_rsa:
![[Pasted image 20230626194805.png]]
Y tenemos un id_rsa pero está encriptado:
![[Pasted image 20230626194842.png]]
Y también dentro del servidor FTP tenemos un note.txt que contiene el nombre de usuario:
![[Pasted image 20230626195304.png]]
Ahora usamos ssh2john para desencriptarlo:
![[Pasted image 20230626195045.png]]
Y ahora on john the ripper iniciamos el crackeo:
![[Pasted image 20230626195118.png]]
Ahora entramos vía ssh, donde ejecutamos el comando chmod 600 id_rsa y entramos poniendo esta contraseña cupcake:
![[Pasted image 20230626200124.png]]
Encontramos un fichero note.txt dentro de la máquina, donde vemos además que no tenemos acceso a casi ningún comando. En este fichero nos dicen que han configurado un servidor HTTP en el cual nadie de fuera puede acceder:
![[Pasted image 20230626200217.png]]
Por tanto salimos de esta conexión y realizamos ssh tunneling port forwarding, de tal forma que lo haremos así, por lo que estamos habilitando tráfico por el puerto 1337:
![[Pasted image 20230626201307.png]]
Y ahora desde nuestra máquina local, tenemos que configurar el fichero proxychains que se encuentra dentro de /etc/proxychains4.conf:
![[Pasted image 20230626201439.png]]
Y tenemos que poner en esta parte el puerto que hayamos puesto para iniciar sesión anteriormente:
![[Pasted image 20230626201526.png]]
Por tanto ahora podemos hacer un escaneo de nmap usando proxychains por el tunel que acabamos de crear, y nos encontrarán puertos internos que estén disponibles:
![[Pasted image 20230626201812.png]]
Y ahora mismo, sabiendo que la máquina tiene disponible en el puerto interno el 80, podemos montarnos en nuestra máquina todo lo del puerto 80 de la víctima a nuestro equipo:
```bash
ssh errorcauser@10.10.179.82 -i id_rsa -L 8080:127.0.0.1:80
```
Y ahora si entramos en el navegador a nuestro localhost por el puerto 8080, tenemos acceso al puerto 80 interno de la máquina:
![[Pasted image 20230626202338.png]]
Vamos a hacer un escaneo de vulnerabilidades de wordpress con nmap y vemos 2 plugins vulnerables:
```bash
nmap -p 8080 --script http-wordpress-enum --script-args check-latest,search-limit=1500 127.0.0.1
```
![[Pasted image 20230629201124.png]]
Y si buscamos en searchsploit nos encontramos con que tenemos un RCE:
![[Pasted image 20230629201312.png]]
Nos lo descargamos y ya tenemos el CVE:
![[Pasted image 20230629201407.png]]
Podemos buscarlo por metasploit y lo localizamos:
![[Pasted image 20230629201433.png]]
Y la configuración que tenemos que hacerle al exploit tiene que ser la siguiente, poniendo el RHOSTS Y RPORT de donde está corriendo el wordpress:
![[Pasted image 20230629201704.png]]
Lanzamos el exploit y estamos dentro, aunque no tenemos muchas funcionalidades todavía:
![[Pasted image 20230629201812.png]]
### INTRUSIÓN MANUAL
También podemos entrar con un exploit de python que encontremos en google, por lo que si buscamos por la vulnerabilidad pública CVE-2020-25213 nos encontramos con este repositorio de github:
![[Pasted image 20230629202318.png]]
Por tanto nos bajamos el exploit y lo ejecutamos de esta forma:
![[Pasted image 20230629202337.png]]
Y ahora nos mandamos una reverse shell:
![[Pasted image 20230629202456.png]]
![[Pasted image 20230629202506.png]]
Una vez dentro, tras una búsqueda nos encontramos con un archivo llamado /var/log/bash.log, que si vemos el contenido, nos encontramos con unas credenciales, las cuales son G00dP@$sw0rd2020 para el usuario :
![[Pasted image 20230629202834.png]]
![[Pasted image 20230629202820.png]]




