Esta sería la estructura de la red, donde empezamos atacando a la máquina .200:
![[Pasted image 20230713150946.png]]
Y nos bajamos la VPN de esta máquina:
![[Pasted image 20230713151109.png]]
Y tenemos conexión con la primera máquina:
![[Pasted image 20230713151426.png]]
También tenemos que bajarnos un archivo comprimido de herramientas:
![[Pasted image 20230713151602.png]]
![[Pasted image 20230713151731.png]]
Y la contraseña es:
```bash
WreathNetwork
```
Estas con las herramientas:
![[Pasted image 20230713151833.png]]
![[Pasted image 20230713151851.png]]

-----------------------------

## RECONOCIMIENTO CON NMAP 1º MAQUINA

Hacemos el reconocimiento con nmap:
![[Pasted image 20230713152207.png]]
Ponemos la IP en el buscador para ver que corre sobre el puerto 80 y vemos que se aplica virtual hosting:
![[Pasted image 20230713152256.png]]
Por lo que tenemos que modificar el fichero /etc/hosts:
![[Pasted image 20230713152408.png]]
Y ahora ya podemos visualizar la web:
![[Pasted image 20230713152444.png]]
![[Pasted image 20230713152521.png]]
Podemos usar este exploit para realizar el ataque del webmin:
![[Pasted image 20230713152614.png]]
Lo tenemos en local:
![[Pasted image 20230713152635.png]]
Y al ejecutarlo ya obtenemos ejecución remota de comandos:
![[Pasted image 20230713152849.png]]
No obstante, esta shell es muy limitada, y en el exploit nos indican que si ponemos shell podremos lanzarnos una reverse shell con netcat:
![[Pasted image 20230713153147.png]]
![[Pasted image 20230713153207.png]]
Una vez dentro, también podemos visualizar el id_rsa del usuario root:
```bash
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```
## MÁQUINA 2
Intentamos hacer un ping a la segunda máquina pero vemos que no funciona:
![[Pasted image 20230713153613.png]]
Pero es posible que nos limiten el acceso por ICMP, por lo que podemos usar nmap con el parámetro -Pn; y vemos que no tenemos nmap:
![[Pasted image 20230713153646.png]]
De todos modos dentro de las tools tenemos binarios que podemos usar en la máquina, como el de nmap:
![[Pasted image 20230713153742.png]]
Y nos compartimos el binario con la máquina víctima:
![[Pasted image 20230713153843.png]]
Damos los permisos y ya podemos usar nmap:
![[Pasted image 20230713153931.png]]
Ahora ejecutamos una pequeña búsqueda con nmap, ya que al compartir este binario no contamos con los scripts de reconocimiento y demás opciones que tiene nmap, donde vemos que nos encuentra el host objetivo:
![[Pasted image 20230713154459.png]]
Por tanto ahora hacemos un escaneo a esta máquina y nos encuentra lo siguiente:
```bash
./nmap 10.200.84.150
```
![[Pasted image 20230713154725.png]]
Y si intentamos acceder desde el navegador de nuestra máquina Kali, no va a funcionar ya que no tenemos conectividad:
![[Pasted image 20230713155243.png]]
Por tanto debemos usar una herramienta que se llama sshuttle, la cual requiere que la máquina víctima tenga abierto el puerto ssh, python instalado, el id_rsa y que sea máquina linux:
```bash
sshuttle -r root@10.200.84.200 --ssh-cmd "ssh -i id_rsa" 10.200.84.0/24 -x 10.200.84.200
```
Primero ponemos la IP de la máquina víctima donde tengamos conexión por ssh, después le pasamos el id_rsa, luego marcamos el rango de red entero que queramos tunelizar y por último la IP que queramos excepcionar para no tener problemas, que será la de la máquina a la que ya tenemos acceso:
![[Pasted image 20230713160455.png]]
Aunque si no tuviéramos clave privada, lo haríamos de esta forma, simplemente proporcionando credenciales:
![[Pasted image 20230720121952.png]]
Y ahora en nuestra máquina atacante, vemos que tenemos un puerto abierto, el cual es el 43641, que es por donde estamos realizando esa tunelización:
![[Pasted image 20230713160821.png]]
Y ahora desde el Kali ya tenemos acceso al puerto 80 de la máquina intermedia a través del tunel:
![[Pasted image 20230713160944.png]]
Pero dentro de esta web vemos el directorio gitstack, por lo que probamos en acceder:
![[Pasted image 20230713161157.png]]
Para explotar esta página web, tenemos este otro exploit:
![[Pasted image 20230713161512.png]]
Y lo descargamos y renombramos:
![[Pasted image 20230713161600.png]]
Lo convertirmos de python2 a python3:
![[Pasted image 20230713161719.png]]
Aquí modificamos la IP:
![[Pasted image 20230713161946.png]]
Y conseguimos ejecutar el comando:
![[Pasted image 20230713162014.png]]
Este exploit lo que hace es guardar una backdoor en php dentro de la máquina víctima, dentro de esta ubicación (miramos el código del script):
![[Pasted image 20230715102436.png]]
Y también nos dice que para acceder desde el navegador lo tendremos que hacer de esta forma:
![[Pasted image 20230715102522.png]]
Por tanto, si lo buscamos en la web vemos que existe:
![[Pasted image 20230715102631.png]]
Por tanto, con curl y usando esta backdoor, podemos ejecutar comandos:
![[Pasted image 20230715103218.png]]
Pero vemos que si intentamos establecer conexión desde esta máquina víctima a nuestra máquina atacante, no tenemos conectividad, por lo que no podremos obtenernos una reverse shell, por lo que aquí es donde tendremos que usar shockat:
![[Pasted image 20230715103310.png]]
Pero ahora el siguiente paso será permitir el tráfico con socat por un determinado puerto, por ejemplo por el puerto 444, lo que significa que tenemos que usar este comando para permitir el tráfico:
```bash
firewall-cmd --zone=public --add-port 444/tcp
```
![[Pasted image 20230715111412.png]]
Y ahora con este comando vamos a hacer un port forwarding desde la IP de la máquina windows víctima a la máquina intermedia, usando el puerto  444, por lo que nos compartimos el netcat a la máquina intermedia:
![[Pasted image 20230715110704.png]]
![[Pasted image 20230715110717.png]]
Añadimos la regla y nos ponemos en escucha por el puerto 444:
![[Pasted image 20230715111049.png]]
Ahora desde la máquina atacante y abusando de la vulnerabilidad del gitstack, lanzamos la reverse shell generada en esta página hacia la máquina windows donde se enviará la reverse shell a la máquina intermedia por el puerto 444:
```bash
https://www.revshells.com/
```
![[Pasted image 20230715111607.png]]
![[Pasted image 20230715111616.png]]
![[Pasted image 20230715111626.png]]








-------------
