## (Analizar metadatos de un PDF con exiftool, vulnerabilidad pdfkit, mirar dentro de carpeta config para escalar privilegios, consultar permisos SUID, vulnerabilidad RCE con yml deserializatión a través de código Ruby)

Haremos el reporte con nmap, donde vemos abierto el puerto 80 para un servidor web y el puerto ssh:
![[Pasted image 20230310194925.png]]
Vamos a consultar la web con whatweb y viéndola en el navegador, pero vemos que tenemos que añadir la url al fichero /etc/hosts:
![[Pasted image 20230310195021.png]]
Lo añadimos:
![[Pasted image 20230310195104.png]]
Y ahora ya si podremos hacer un whatweb y acceder a la página por el navegador:
![[Pasted image 20230310195143.png]]
![[Pasted image 20230310195156.png]]
Vamos a darle un PDF de prueba montándonos un servidor web con python y vemos que recibe la conexión y procesa el pdf:
![[Pasted image 20230310202429.png]]
![[Pasted image 20230310202439.png]]
Con la herramienta exitfool vamos a inspeccionar los metadatos de este documento pdf; y vemos que está generado con una aplicación que se llama pdfkit:[[Exiftool - Extraer Ubicación de una Foto y sus Metadatos]]
![[Pasted image 20230310202733.png]]
Y nos encontramos con que esta versión de esta herramienta tiene una vulnerabilidad pública CVE:
![[Pasted image 20230310203025.png]]
Vamos a buscar alguna herramienta en github que nos permita explotar esta vulnerabilidad, donde nos encontramos con esta hecha en python:
![[Pasted image 20230310203653.png]]
Aquí tenemos las instrucciones para su uso:
![[Pasted image 20230310203718.png]]
Por tanto vamos a lanzarla mientras nos ponemos en escucha con netcat por el puerto 443:
![[Pasted image 20230310203746.png]]
Y recibimos la conexión:
![[Pasted image 20230310203803.png]]
Una vez dentro intentamos obtener la flag de user pero vemos que no tenemos permisos por lo que tenemos que investigar y nos encontramos que dentro del directorio /home del usuario ruby tenemos el fichero de linpeas.sh, que sirve para comprobar distintas formas de escalar privilegios:
![[Pasted image 20230310204003.png]]
No obstante, si miramos un poco más, nos encontramos que dentro del directorio /home/ruby/.bundle hay un fichero de configuración:
![[Pasted image 20230310205734.png]]
Si miramos dentro de config, nos encontramos con las credenciales del usuario henry, las cuales son: henry:Q3c1AqGHtoI0aXAYFH
![[Pasted image 20230310210047.png]]
Viendo esto, podemos probar en conectarnos por ssh con este usuario henry y probar con esta contraseña; y estamos dentro:
![[Pasted image 20230310210224.png]]
Vamos a comprobar donde tenemos permisos SUID (Se tratan de permisos que permiten a un usuario ejecutar un archivo con los privilegios del propietario del archivo), por tanto ejecutamos este comando:
```bash
find / -perm -u=s -type f 2>/dev/null
```
 Y vemos el permiso de sudo:
 ![[Pasted image 20230311234934.png]]
 Por tanto vamos a ejecutar el comando sudo -l para ver los permisos de sudo asignados al usuario que somos; y podemos ver este script: ![[Pasted image 20230311235052.png]]
 Vamos a ver el contenido de este script y vemos que está llamando a un fichero que se llama dependencies.yml:  ![[Pasted image 20230311235557.png]]
 Vemos que este fichero ya se encuentra en nuestro actual directorio, por lo que vamos a inspeccionarlo:
 ![[Pasted image 20230311235826.png]]
 Viendo esto, hay un tipo de ataque que se llama yml deserialización, el cual consiste en una técnica de ataque en la que un atacante manipula los datos de entrada en un archivo YAML para ejecutar código malicioso en un sistema vulnerable. Por tanto tenemos que buscar algún código adaptado a ruby (porque hemos visto que el script que llama al fichero yml está hecho en ruby), por lo que ponemos en google yaml deserialization ruby: [[3 - YML Deserialization]]
 ![[Pasted image 20230312000735.png]]
 Y en esta web tenemos un tutorial, donde nos dice que en la parte de git set podemos insertar un comando: ![[Pasted image 20230312000803.png]]
 Por lo que vamos a poner este código tal cual dentro del fichero dependencies.yml:
 ![[Pasted image 20230312001340.png]]
 ![[Pasted image 20230312001416.png]]
 Y ahora por ejemplo como se puede ver hemos dado la orden de crear un fichero llamado pwned, por tanto vamos a ejecutar el script y vamos a ver si llama y ejecuta correctamente este código dentro del fichero yml:
 ![[Pasted image 20230312001505.png]]
 Y ahora vemos que efectivamente ha creado el archivo:
 ![[Pasted image 20230312001528.png]]
 Vamos a dar todos los permisos a la bash:
 ![[Pasted image 20230312001638.png]]
 Ahora probamos en ejecutar el script:
 ![[Pasted image 20230312001703.png]]
 Y ahora si ejecutamos el comando bash -p nos debería lanzar una bash como root:
 ![[Pasted image 20230312001725.png]]
 
 