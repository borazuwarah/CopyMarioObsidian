Hacemos el reconocimiento con nmap:
![[Pasted image 20230702115218.png]]
Vemos que por el puerto 3333 corre un servidor web, por lo que debemos ponerlo en el buscador y accedemos:
![[Pasted image 20230702115231.png]]
Ahora vamos a hacer fuzzing a esta página web en busca de directorios, donde nos encontramos con el directorio internal:
![[Pasted image 20230702115246.png]]
Lo probamos en la web y podemos acceder a una web donde podemos subir algún archivo:
![[Pasted image 20230702115300.png]]
Probamos en crear un archivo .php malicioso para subirlo en la web:
![[Pasted image 20230702115320.png]]
Pero si lo intentamos subir no nos deja:
![[Pasted image 20230702115332.png]]
Por lo que tendremos que recurrir a burpsuite e interceptamos la petición justo en el momento de subir el archivo:
![[Pasted image 20230702115346.png]]
Pero podemos probar en cambiarle el nombre por phtml y veremos que ahora sí nos lo acepta:
![[Pasted image 20230702115401.png]]
![[Pasted image 20230702115409.png]]
Y ahora si navegamos al directorio uploads, nos encontramos con el archivo malicioso que acabamos de subir:
![[Pasted image 20230702115421.png]]
Y tenemos ejecución remota de comandos:
![[Pasted image 20230702115435.png]]
Ahora url-encodeamos el comando para mandarnos una reverse shell y lo pegamos en la url mientras permanecemos en escucha con netcat:
![[Pasted image 20230702115446.png]]
Y ya hemos recibido la shell reversa:
![[Pasted image 20230702115457.png]]
Dentro del directorio de bill obtenemos la flag de user:
![[Pasted image 20230702115511.png]]
Tras hacer una búsqueda de binarios con permisos de root, llama la atención el systemctl:
![[Pasted image 20230702115526.png]]
Nos informamos en gtfobins cómo escalar privilegios con este binario y nos encontramos con esto:
![[Pasted image 20230702115536.png]]
Por tanto vamos a crear un script con este contenido pero modificando la ruta absoluta del binario systemctl y poniendo también el comando para visualizar la flag de root dentro de /tmp/output:
```bash
#!/bin/bash

TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/output"
[Install]
WantedBy=multi-user.target' > $TF
/bin/systemctl link $TF
/bin/systemctl enable --now $TF
```
Y lo ejecutamos:
![[Pasted image 20230702115559.png]]
Y ahora tenemos un archivo llamado output con el contenido de la flag de root:
![[Pasted image 20230702121729.png]]
Aunque también podríamos haber ejecutado el siguiente comando:
```bash
chmod u+s /bin/bash
```
Y de esta forma le habremos cambiado los permisos a la bash, para poder ejecutar el comando bash -p y obtener una bash como root:
![[Pasted image 20230702204101.png]]