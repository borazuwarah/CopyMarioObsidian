Haremos el reconocimiento de nmap:
![[Pasted image 20230608150953.png]]
Y esta es la página web:
![[Pasted image 20230608151142.png]]
En este punto, comenzamos investigando el puerto 21 haciendo un ftp login como anonymous, y vemos que hay un archivo zip:
![[Pasted image 20230608152245.png]]
Pero el archivo .zip tiene una contraseña:
![[Pasted image 20230608152321.png]]
Y ahora podemos usar fcrackzip para crackear la contraseña de este fichero:
![[Pasted image 20230608152714.png]]
Y ahora ya podemos descomprimirlo, y nos saca un secret.zip y un respectmydrip.txt:
![[Pasted image 20230608152755.png]]
Y el .txt nos dice esto:
![[Pasted image 20230608153036.png]]
Ahora haremos fuzzing en la web, pero debemos hacerlo con extensiones de archivos; y vemos que nos encuentra un robots.txt:
![[Pasted image 20230608153920.png]]
Y así sería en la web:
![[Pasted image 20230608153344.png]]
Accedemos a la segunda y no funciona:
![[Pasted image 20230608153901.png]]
Pero accedemos al primero de los dos ficheros y nos encontramos con esto:
![[Pasted image 20230608153427.png]]
Ahora en este punto, vamos a hacer fuzzing al parámetro del archivo dripispowerful para encontrar una ruta válida hasta ese archivo con estos parámetros de wfuzz:
```bash
wfuzz -c --hc=404 -t 200 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u 'http://192.168.0.21/?FUZZ=/etc/dripispowerful.html'
```
Pero nos encuentra demasiadas rutas:
![[Pasted image 20230608154309.png]]
Por tanto hacemos una búsqueda igual pero excluyendo las líneas que tengan 21 letras, para buscar aquella que sea diferente, donde nos encuentra drip. El símbolo "/" seguido de "?" dentro del parámetro "-u" en el comando de wfuzz indica que se está especificando una URL con parámetros. En las URL, los parámetros se añaden después del símbolo "?" y se utilizan para transmitir información adicional al servidor web:
![[Pasted image 20230608154951.png]]
Y ahora sí podemos acceder al archivo:
![[Pasted image 20230608155050.png]]
Podemos hacer un curl a esto mismo y obtenemos una contraseña:
```bash
curl http://192.168.0.21/?drip=/etc/dripispowerful.html
```
![[Pasted image 20230608155138.png]]
Y también tenemos dos usuarios:
![[Pasted image 20230608155302.png]]
Por tanto vamos a probar en acceder por ssh y con uno de los usuarios no ha funcionado:
![[Pasted image 20230608155448.png]]
Pero sin embargo con el otro usuario sí podemos acceder:
![[Pasted image 20230608155535.png]]
Una vez dentro, ejecutamos una búsqueda de binarios SUID; y nos encontramos con polkit, el cual es una vulnerabilidad:
![[Pasted image 20230608160814.png]]
Y nos encontramos con este exploit:
![[Pasted image 20230608160851.png]]
![[Pasted image 20230608160948.png]]
Nos compartimos el exploit:
![[Pasted image 20230608161002.png]]
Lo ejecutamos:
![[Pasted image 20230608161403.png]]
Y si esperamos un rato, nos habrá creado un nuevo usuario llamado Ahmed pero vemos que además ya somos root:
![[Pasted image 20230608161224.png]]
Accedemos a la flag de root:
![[Pasted image 20230608161503.png]]
