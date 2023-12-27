Hacemos el escaneo de nmap, donde vemos que tiene abierto sólo el puerto 80:
![[Pasted image 20230227205357.png]]
Y ahora vamos a hacer el escaneo exhaustivo del puerto que tiene abierto, que es el 80:
![[Pasted image 20230227205417.png]]
Y si accedemos al navegador con esta IP vemos que es una web de juegos:
![[Pasted image 20230227205430.png]]
Si intentamos iniciar sesión con una inyección SQL vemos que no podemos:
![[Pasted image 20230227205441.png]]
Pero podemos enviar igualmente este login con los caracteres especiales utilizando burpsuite, donde esta vez pondré un correo válido para que se envíe la petición y sea interceptada por burpuite:
![[Pasted image 20230227205455.png]]
![[Pasted image 20230227205459.png]]
Pulsamos control + r para enviar la petición al repeater y verlo todo en tiempo real:
![[Pasted image 20230227205506.png]]
Y en la parte del email podemos enviar un intento de inyección SQL:
![[Pasted image 20230227205513.png]]
Y en la respuesta podemos ver que la inyección fue exitosa:
![[Pasted image 20230227205524.png]]
Entonces ahora si lanzamos la petición con esta inyección, veremos cómo hemos iniciado sesión:
![[Pasted image 20230227205532.png]]
Ahora que estamos logueados, si intento entrar dentro de la rueda de los ajustes, me dará un error:
![[Pasted image 20230227205607.png]]
Pero sin embargo se nos muestra la dirección donde debemos ir:
![[Pasted image 20230227205616.png]]
Para ello tenemos que modificar el fichero etc/hosts para que la IP apunte a este dominio y podamos entrar:
![[Pasted image 20230227205630.png]]
Y ahora ya hemos podido acceder correctamente a esta dirección:
![[Pasted image 20230227205638.png]]
Ahora en este punto vamos a utilizar sqlmap para obtener las credenciales para acceder, pero
debemos hacerlo en el login del principio:
![[Pasted image 20230227205645.png]]
Para ello interceptamos la petición con burpsuite y guardamos el contenido de la petición en un
documento .txt:
![[Pasted image 20230227205652.png]]
Lo pasamos a un bloc de notas:
![[Pasted image 20230227205658.png]]
Y ahora ejecutamos este primer comando con sqlmap para conocer las bases de datos:
![[Pasted image 20230227205704.png]]
Y ya tenemos las dos bases de datos:
![[Pasted image 20230227205716.png]]
Ahora vamos a conocer las tablas de la base de datos main:
![[Pasted image 20230227205729.png]]
Y nos encontró estas 3 tablas, que la interesante es la de user:
![[Pasted image 20230227205742.png]]
Ahora vamos a acceder a las columnas de la tabla users:
![[Pasted image 20230227205749.png]]
Y ya tenemos el resultado:
![[Pasted image 20230227205755.png]]
Y ahora vamos a acceder a los registros de cada una de estas columnas:
![[Pasted image 20230227205804.png]]
Y nos encontró esta credencial, aunque viene hasheada:
![[Pasted image 20230227205810.png]]
El hash identifier nos dice que es MD5:
![[Pasted image 20230227205817.png]]
Pegaremos la contraseña hasheada en un txt para descifrarla con john the ripper:
![[Pasted image 20230227205853.png]]
Y ahora que ya sabemos que la clave está cifrada con MD5, si utilizamos este comando con john the ripper y aplicando el diccionario rockyou.txt que lo tenemos situado en el escritorio, se nos habrá descifrado la clave:[[John The Ripper]]
![[Pasted image 20230227205905.png]]
Ahora ya podemos iniciar sesión:
![[Pasted image 20230227205912.png]]
Ya estamos dentro:
![[Pasted image 20230227205923.png]]
Ahora vamos al menú de ajustes para ver si es vulnerable a server site template injection, ya que en el nombre se llega a multiplicar el 7 por 7:
![[Pasted image 20230227205934.png]]
Ahora el siguiente objetivo es ejecutar comandos dentro de este recuadro de full name, para ello
vamos a utilizar un payload que podemos encontrarlo dentro de un repositorio de github donde se ubican los payloads más utilizados:
![[Pasted image 20230227205956.png]]
Filtramos dentro de go to file por server side template injection:
![[Pasted image 20230227210003.png]]
Y buscamos esto para localizar payloads que nos sirvan para ejecutar código remoto:
![[Pasted image 20230227210013.png]]
Y aquí podemos elegir el que queramos:
![[Pasted image 20230227210024.png]]
Y habremos comprobado que tenemos la opción de ejecutar comandos remotamente, en este caso ejecutamos el comando para conocer el id:
![[Pasted image 20230227210033.png]]
Pero yo puedo ejecutar cualquier comando en vez del id, entonces voy a poner hostname -i para
conocer la IP:
![[Pasted image 20230227210039.png]]
Ahora que ya conocemos la IP y vemos que tenemos ejecución remota de comandos, vamos a
comprobar si hay conectividad entre esta máquina y la mía, para ello lanzo un ping remotamente y
en mi máquina la pongo a escuchar por la interfaz que está conectada a hackthebox:
![[Pasted image 20230227210046.png]]
Lo ponemos a escuchar:
![[Pasted image 20230227210053.png]]
Y ahora hago un ping de la máquina remota a mi máquina host en la interfaz de hackthebox:
![[Pasted image 20230227210059.png]]
Y vemos que el ping lo recibimos por la interfaz tun0:
![[Pasted image 20230227210106.png]]
![[Pasted image 20230227210109.png]]
Ahora vamos a obtener una consola remota a esta máquina, para ello lo primero será crear un
archivo html con un código que vamos a compartir con esta máquina e inyectar nuestro código:
![[Pasted image 20230227210119.png]]
Y aquí insertamos nuestro código que lo que hace es compartir una consola con nuestra dirección IP (es la IP de la interfaz de hackthebox que estamos utilizando):
![[Pasted image 20230227210134.png]]
Y ahora este código lo vamos a compartir por el navegador por el puerto 80:
![[Pasted image 20230227210143.png]]
De tal forma que ahora estamos compartiendo este fichero:
![[Pasted image 20230227210152.png]]
Y ahora es cuando lanzamos un curl desde la web para que reciba este código y nos envíe una bash a nuestra máquina host:
![[Pasted image 20230227210201.png]]
Y debemos asegurarnos de estar escuchando por el puerto 443 que hemos definido en nuestro html desde nuestro equipo host:
![[Pasted image 20230227210214.png]]
Y ahora ya tenemos la conexión remota desde netcat que lo teníamos escuchando:
![[Pasted image 20230227210224.png]]
Ahora que estamos dentro de este contenedor de docker, vamos a navegar por los directorios hasta llegar a la flag:
![[Pasted image 20230227210232.png]]
![[Pasted image 20230227210234.png]]
No obstante ahora estamos dentro de un contenedor de docker, por tanto debo obtener acceso a la máquina víctima, por tanto haremos un ping desde el contenedor a la máquina real, y vemos que hay conectividad:
![[Pasted image 20230227210244.png]]
También podemos ejecutar el comando ifconfig para conocer la IP del contenedor:
![[Pasted image 20230307084539.png]]
Vamos a detectar puertos internos en uso con un script de Python, el cual será este:
```python
import sys
import socket

if len(sys.argv) == 2:

    target = sys.argv[1]
else:
    print("Indica la IP a escanear. Ej: portScan 10.10.10.1")

try:
    for port in range(1,65535):
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        socket.setdefaulttimeout(1)

        result = s.connect_ex((target,port))
        if result ==0:
            print(f"Port {port} is open")
        s.close()

except socket.error:
        print("[-] Servidor no responde")
        sys.exit()
```
Este script nos lo compartimos al contenedor de la máquina víctima:
![[Pasted image 20230307085548.png]]
Y lo descargamos desde la misma:
![[Pasted image 20230307085609.png]]
Y ahora ejecutamos este script y nos dice que tanto el puerto 22 y 80 están abiertos:
![[Pasted image 20230307085714.png]]
Por tanto, ya que vemos que el puerto 22 que es el puerto de ssh está abierto, vamos a probar en iniciar sesión como el usuario Augustus y con la contraseña que antes encontramos (superadministrator):
