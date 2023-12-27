# PREPARATIVOS
Vamos a tener un servidor DHCP con Ubuntu Server donde va a tener como cliente una máquina Windows 11; los dos bajo una red interna de virtualbox:
![[Pasted image 20230115151052.png]]
Y en la máquina servidor utilizaremos este comando para cambiar la IP del servidor Linux, indicando el adaptador que queramos cambiar:
![[Pasted image 20230115151201.png]]
Y ahora en la máquina Windows vamos a poner una IP dentro del rango de la IP del servidor, para que haya conectividad:
![[Pasted image 20230115153912.png]]
![[Pasted image 20230115153852.png]]
Y ahora veremos como desde la máquina Linux no podemos enviar un PING a la Windows; y esto ocurre porque el firewall está impidiendo que hagamos peticiones ICMP a la máquina Windows, por lo que crearemos una nueva regla en el firewall:
![[Pasted image 20230115154109.png]]
Crearemos una nueva regla:
![[Pasted image 20230115154203.png]]
Aceptando peticiones del protoclo ICMP:
![[Pasted image 20230115154231.png]]
Y le ponemos por ejemplo este nombre:
![[Pasted image 20230115154304.png]]
Y ahora si hacemos un PING desde el servidor a la máquina Windows, veremos que ya tenemos total conectividad:
![[Pasted image 20230115154359.png]]
Ahora por último será configurar una IP estática en Linux utilizando este comando para editar el fichero de configuración:
![[Pasted image 20230115182059.png]]
Y dejamos esta configuración con la IP que queramos dentro del adaptador que hayamos elegido configurar una IP estática:
![[Pasted image 20230115182413.png]]
Y ahora con el comando sudo netplan apply aplicaremos estos cambio:
![[Pasted image 20230115182441.png]]
Y ya tenemos una IP estática en nuestra máquina:
![[Pasted image 20230115182520.png]]
# CONFIGURACIÓN SERVIDOR DHCP
Lo primero será abrir el fichero de configuración de DHCP:
![[Pasted image 20230115182611.png]]
Y en este punto tenemos que poner el nombre de la interfaz que queramos editar, que en este caso es enp0s3:
![[Pasted image 20230115182715.png]]
Y ahora tendremos que editar este otro fichero que será donde estableceremos los ajustes necesarios para nuestro servidor DHCP:
![[Pasted image 20230115182841.png]]
![[Pasted image 20230115182903.png]]
Y en la parte de abajo establecemos esta configuración; donde pondremos un grupo que por ejemplo será grupo red windows, luego ponemos la IP acabada en 0 para representar la totalidad de equipos y su submáscara de red. Más tardes ponemos la frecuencia por la que se aplicará esta configuración, y luego ponemos la dirección de la puerta de enlace donde pone option routers; y por último el servidor DNS:
![[Pasted image 20230115183731.png]]
Ahora guardamos este fichero y reiniciamos el servicio DHCP:
![[Pasted image 20230115183959.png]]
Y con el comando systemctl status isc-dhcp-server veremos que se encuentra activo:
![[Pasted image 20230115184050.png]]
Ahora vamos a la máquina Windows que la tenemos sin conexión y sin ninguna IP configurada:
![[Pasted image 20230115184200.png]]
Por lo que vamos a abrir el menú de centro de redes y recursos compartidos donde podremos configurar conectarnos a un servidor DHCP:
![[Pasted image 20230115184602.png]]
Y tras dejar activado que nos den una IP utilizando el servidor DHCP, vemos que si utilizamos un ipconfig en la terminal veremos que nos ha asignado una IP comprendida entre el 10 y 200 que configuramos anteriormente en el fichero de configuración de DHCP en el servidor de Linux, además de habernos dado correctamente la IP de la puerta de enlace:
![[Pasted image 20230115184721.png]]
## OPCIÓN 2
Instalamos el servicio de DHCP:
```bash
apt install isc-dhcp-server
```
![[Pasted image 20231101112049.png]]
Y en virtualbox le asignamos una red interna a la máquina:
![[Pasted image 20231101112202.png]]
Y asignamos la dirección IP estática correspondiente:
![[Pasted image 20231101112259.png]]
Comprobamos que se haya cambiado correctamente la dirección IP:
![[Pasted image 20231101112440.png]]
Y debemos editar el fichero de configuración de dhcp, de tal forma que primero haremos una copia de seguridad:
![[Pasted image 20231101112613.png]]
Y ahora lo editamos con nano:
![[Pasted image 20231101112631.png]]
Donde podemos eliminarlo todo y añadir las siguientes líneas de configuración:
```bash
# Archivo de configuración del servidor DHCP

subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.150;
    option subnet-mask 255.255.255.0;
    option routers 192.168.1.1;
    option domain-name "sri.local";
    option domain-name-servers 1.1.1.1, 9.9.9.9;
    default-lease-time 600;
    max-lease-time 7200;
}
```
![[Pasted image 20231101114152.png]]
Guardamos estos cambios y reiniciamos y comprobamos el correcto funcionamiento del servicio con el comando systemctl:
![[Pasted image 20231101114223.png]]
Ahora vamos a probar el servicio utilizando otro equipo y poniéndolo dentro de la red interna en virtualbox, por lo que usamos una máquina cliente linux:
![[Pasted image 20231101114449.png]]
![[Pasted image 20231101114501.png]]
Por lo que entramos dentro de dicha máquina y al ejecutar el comando ip a vemos que tenemos la IP 192.168.1.101 asignada automáticamente mediante el protocolo dhcp:
![[Pasted image 20231101114629.png]]
Y haremos lo mismo pero con una máquina Windows 7:
![[Pasted image 20231101114804.png]]
Al encender esta máquina windows 7, vemos que nos ha asignado la siguiente dirección IP 192.168.1.102:
![[Pasted image 20231101114926.png]]
Ahora, por último, vamos a hacer la reserva de una IP determinada (de la máquina Windows 7) como nombre alvarez, por lo que dentro del fichero de configuración del servidor dhcp (el fichero dhcp.conf), debemos de ver la mac de mi máquina windows 7 de la siguiente forma:
![[Pasted image 20231101120256.png]]
Y ahora esta dirección MAC la añadimos en el fichero de configuración de dhcp.conf:
![[Pasted image 20231101120534.png]]
Reiniciamos el servicio:
![[Pasted image 20231101120003.png]]
Y ahora reiniciamos la máquina windows 7 para comprobar que se le haya asignado correctamente la IP 192.168.1.111:
![[Pasted image 20231101120551.png]]

