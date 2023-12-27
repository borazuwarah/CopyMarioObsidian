Hacemos el reconocimiento con nmap:
![[Pasted image 20230702122506.png]]
Y esta es la página web:
![[Pasted image 20230702122536.png]]
Podemos hacer fuzzing y nos encontramos con el directorio files:
![[Pasted image 20230702122609.png]]
Y aquí tenemos su contenido:
![[Pasted image 20230702122622.png]]
Y en cuanto al servicio FTP está activado el login como anonymous, por tanto vamos a subir un archivo php malicioso:
![[Pasted image 20230702122858.png]]
![[Pasted image 20230702122907.png]]
Y dentro del directorio /ftp nos encontramos con el archivo subido:
![[Pasted image 20230702122931.png]]
Lo ejecutamos y ya tenemos ejecución remota de comandos:
![[Pasted image 20230702122947.png]]
Urlencodeamos el comando para enviarnos una reverse shell:
![[Pasted image 20230702123720.png]]
Lo ejecutamos, nos ponemos en escucha con netcat y ya estamos dentro:
![[Pasted image 20230702123742.png]]
![[Pasted image 20230702123749.png]]
Una vez dentro, podemos visualizar la receta dentro del directorio raiz, la cual es love:
![[Pasted image 20230702124718.png]]
Nos encontramos con un archivo llamado suspicious.pcapng, el cual es posible que contenga algo interesante, por lo que nos lo copiamos dentro del directorio ftp para descargarlo en nuestra máquina atacante y analizarlo:
![[Pasted image 20230702124855.png]]
![[Pasted image 20230702124920.png]]
Vemos que este archivo tiene extensión .pcapng, por tanto es posible que podamos leerlo con wireshark con tcpdump:
![[Pasted image 20230702125116.png]]
Una vez aquí hacemos clic derecho sobre alguno de los paquetes y hacemos clic en Follow stream:
![[sdfgsdfghsdfhdfghdfgh.png]]
Y se nos abre esta ventana donde capturamos unas credenciales:
c4ntg3t3n0ughsp1c3
![[Pasted image 20230702125617.png]]
Probamos estas credenciales con el usuario lennie y vemos que funciona:
![[Pasted image 20230702125657.png]]
Vemos una carpeta llamada scripts donde hay un script, el cual ejecuta un comando como root y llama a otro script llamado print.sh situado en /etc:
![[Pasted image 20230702130004.png]]
Y el contenido de print.sh es este:
![[Pasted image 20230702130026.png]]
Por tanto vamos a modificar el contenido del archivo print.sh y nos enviaremos una reverse shell como root si estamos escuchando con netcat:
![[Pasted image 20230702130934.png]]
Esperamos un poco y cuando veamos el archivo llamado reporte ya habremos recibido la reverse shell como root:
![[Pasted image 20230702131138.png]]
![[Pasted image 20230702131010.png]]
