Hacemos el reconocimiento con nmap:
![[Pasted image 20231213104608.png]]
Y esto es lo que corre por el puerto 80, una página en blanco:
![[Pasted image 20231213104642.png]]
Pero sin embargo, haciendo un escaneo por UDP, podemos ver los siguientes protocolos, donde vemos abierto snmp:
![[Pasted image 20231213105035.png]]
Y ahora el siguiente paso es usar onesixtyone para hacer un ataque de fuerza bruta contra este protocolo y encontrar la clave de acceso. Empleando un diccionario específico para enumerar este protocolo:
```bash
onesixtyone -c /usr/share/wordlists/seclists/Discovery/SNMP/common-snmp-community-strings-onesixtyone.txt 192.168.0.27
```
Y vemos que la clave es root:
![[Pasted image 20231213105501.png]]
Y dentro de este reporte podemos intuir que obtendremos información interesante del protocolo FTP, que está corriendo por el puerto 8000, por lo que con grep filtramos por la palabra ftp; y vemos las credenciales del usuario frank:
![[Pasted image 20231213105753.png]]
```bash
frank:my_FTP_is_c00l
```
Y vemos que las credenciales son correctas:
![[Pasted image 20231213105849.png]]
Y podemos quiza subir una webshell, ya que probablemente esto se comunica con el puerto 80:
![[Pasted image 20231213105907.png]]
Editamos el fichero pentestmonkey:
![[Pasted image 20231213110016.png]]
La subimos por ftp:
![[Pasted image 20231213110125.png]]
La llamamos desde el navegador:
![[Pasted image 20231213110107.png]]
Y recibimos la conexión:
![[Pasted image 20231213110225.png]]
Y vemos que como el usuario alan podemos ejecutar una sh:
![[Pasted image 20231213110400.png]]
Y así nos convertimos en el usuario alan:
![[Pasted image 20231213110425.png]]
Y ahora vemos que como root podemos ejecutar /usr/bin/most como root:
![[Pasted image 20231213110519.png]]
Y al ejecutarlo vemos que como root podemos hacer búsquedas de algún archivo en concreto:
![[Pasted image 20231213110737.png]]
Por lo que accedemos al id_rsa del usuario root:
```bash
sudo /usr/bin/most /root/.ssh/id_rsa > id_rsa
```
![[Pasted image 20231213110842.png]]
Accedemos a ella:
![[Pasted image 20231213110924.png]]
Y ahora hacemos un ataque de fuerza bruta con john the ripper a este hash, hasta obtener la contraseña:
```bash
ssh2john id_rsa > hash
john --wordlist=/usr/share/wordlists/rockyou.txt hash
```
![[Pasted image 20231213111146.png]]
Las credenciales del usuario root son las siguientes:
```bash
root:rootbeer
```
Por tanto, otorgamos permisos 600 al id_rsa y como root podemos entrar a la máquina por ssh de la siguiente forma:
![[Pasted image 20231213111256.png]]
