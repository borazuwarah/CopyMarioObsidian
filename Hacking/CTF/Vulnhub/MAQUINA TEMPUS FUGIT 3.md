Lo primero será hacer el reconocimiento de siempre con nmap y también podemos utilizar netdiscover para ver que redes tenemos dentro de nuestra interfaz de red, por tanto ejecutamos el netdiscover eth0 ya que es la interfaz de red que se está utilizando:
![[Pasted image 20221218202416.png]]
Si hacemos el escaneo con nmap vemos que tiene abierto el puerto 80:
![[Pasted image 20221218202424.png]]
Ahora el reporte de whatweb:
![[Pasted image 20221218202430.png]]
Y esta es la web:
![[Pasted image 20221218202437.png]]
Vamos a hacer fuzzing con dirb y vemos que nos encuentra algunos directorios:
![[Pasted image 20221218202444.png]]
Si ponemos cualquiera de estas urls nos salen unos avisos extraños que nos redirigen a la ventana principal de vuelta:
![[Pasted image 20221218202451.png]]
Pero si en la url escribimos algo aleatorio, es decir, una ruta que no existe, vemos que nos carga otra página y que en la parte de abajo se ve impresa la URL que hemos puesto:
![[Pasted image 20221218202458.png]]
Por tanto probamos si es vulnerable a SSTI; y vemos que tras poner un 2*2 nos sale un 4, por tanto sí es vulnerable:
![[Pasted image 20221218202505.png]]
Ahora si vamos a la web de payloadsallthethings podemos obtener el código para explotar el SSTI:
![[Pasted image 20221218202512.png]]
Lo probamos pero con este no funciona, aunque sí con este otro:
`{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}`

![[Pasted image 20221218202529.png]]
Entonces ahora obtenemos la reverse shell:
![[Pasted image 20221218202546.png]]
![[Pasted image 20221218202550.png]]
Una vez que estamos dentro podemos leer el fichero [app.py](http://app.py) y encontramos la primera flag, así como otras contraseñas y la librería que se está utilizando (sqlcipher):
![[Pasted image 20221218202602.png]]
Aunque vemos que viene el base64, por lo que debemos decodificarla:
![[Pasted image 20221218202608.png]]
Ahora, dentro del directorio static, vemos que hay un fichero con extensión .db, que es una base de datos:
![[Pasted image 20221218202616.png]]
Ahora, para leer este tipo de archivos, ya que intuimos que se trata de sqlite, ya que si se está utilizando flask podemos pensar que la base de datos también es una librería de python, por tanto para abrir este archivo utilizamos sqlcipher de esta forma y poniendo la otra contraseña que vimos antes; y ya podemos ver las tablas de la base de datos sqlite:
![[Pasted image 20221218202623.png]]
Y ahora vemos el contenido de la tabla users, donde hay usuarios con sus contraseñas:
![[Pasted image 20221218202630.png]]
Lo decodificamos ya que viene en base64 y obtenemos la flag 3, aunque todavía nos falta la 2:
![[Pasted image 20221218202640.png]]
Por tanto ahora vamos a probar en entrar con estos usuarios dentro del panel de login de la web:
![[Pasted image 20221218202647.png]]
Y ya estamos logueados:
![[Pasted image 20221218202656.png]]
Y aquí vemos otra flag en formato base64:
![[Pasted image 20221218202708.png]]
La decodificamos y obtenemos la flag 2:
![[Pasted image 20221218202716.png]]
Ahora, aprovechando que estamos dentro de la máquina, podemos mirar los puertos internos de la máquina; y para ello podemos crearlo con python utilizando este código (personalizando la parte de nuestra IP):
![[Pasted image 20221218202735.png]]
Lo ejecutamos en la máquina víctima y nos encuentra el puerto 443 y 57255:
![[Pasted image 20221218202742.png]]
