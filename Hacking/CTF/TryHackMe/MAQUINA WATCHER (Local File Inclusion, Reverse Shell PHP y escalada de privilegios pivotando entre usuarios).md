Haremos el reconocimiento con nmap:
![[Pasted image 20230605111648.png]]
Vamos al puerto 80 y nos encontramos con esta web:
![[Pasted image 20230605111756.png]]
Una vez en esta web, vamos a hacer fuzzing con gobuster en busca de directorios:
```bash
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://10.10.37.34/
```
Y vemos que nos encuentra un robots.txt si le pasamos el parámetro -x para buscar por extensiones de archivos, por lo que vamos a acceder:
![[Pasted image 20230605114127.png]]
Y vemos que nos dicen algo de una flag:
![[Pasted image 20230605112845.png]]
Y si buscamos la ruta donde vemos ese allow, podemos acceder a la flag:
![[Pasted image 20230605112927.png]]
Y si vamos a la otra dirección vemos que no nos deja acceder:
![[Pasted image 20230605113008.png]]
Ahora, si vamos a la página principal y revisamos el código fuente, veremos que se hace un llamamiento a archivos .php del sistema, por lo que posiblemente estemos ante un path traversal o local file inclusion:
![[Pasted image 20230605113221.png]]
Por tanto vamos a probarlo:
![[Pasted image 20230605113430.png]]
![[Pasted image 20230605113444.png]]
Y ahora gracias a este local file inclusion, si podemos leer el archivo de antes que no podíamos acceder a él en el robots.txt:
![[Pasted image 20230605113659.png]]
Ahora que tenemos estas credenciales del protocolo ftp, vamos a probar en iniciar sesión:
```bash
 ftpuser:givemefiles777
```
Y ya estamos dentro:
![[Pasted image 20230606092920.png]]
Y vemos que hay una carpeta files que tiene permiso de escritura, por lo que podríamos subir algo malicioso y leerlo posteriormente a través del local file inclusion:
![[Pasted image 20230606093347.png]]
Ahora el objetivo es ganar acceso a la máquina remota, por lo que tenemos aquí una carpeta llamada files a la que tenemos permisos de escritura, por lo que podemos subir un archivo PHP malicioso que nos proporcione una reverse shell aprovechándonos de LFI. Por lo que usamos este código:
![[Pasted image 20230609171842.png]]
Lo editamos y añadimos nuestra IP y puerto por el cual estaremos con netcat escuchando:
![[Pasted image 20230609172058.png]]
Y lo subimos:
![[Pasted image 20230609171925.png]]
Por tanto ahora desde el navegador ponemos esta url llamando al fichero php malicioso:
![[Pasted image 20230609172339.png]]
Y ya hemos obtenido la reverse shell:
![[Pasted image 20230609172403.png]]
Ahora para encontrar la flag 3, nos ubicamos dentro de la carpeta del servidor web por donde hemos entrado (aunque también podríamos buscar la flag con find, y nos encontramos con ella):
![[Pasted image 20230609172644.png]]
Ahora podemos ver que como el usuario toby podemos ejecutar cualquier comando:
![[Pasted image 20230609173404.png]]
Si preguntamos a la inteligencia artificial cómo convertirnos en el usuario toby en este contexto, nos da estas instrucciones:
![[Pasted image 20230609173616.png]]
Por tanto lo probamos y podemos ejecutar un comando como toby:
![[Pasted image 20230609173640.png]]
Por tanto vamos a lanzar una bash desde toby:
![[Pasted image 20230609173701.png]]
En el directorio personal del usuario toby tenemos la flag 4:
![[Pasted image 20230609173800.png]]
Dentro del directorio personal del usuario Toby nos encontramos que hay una nota que dice algo de una tarea cron:
![[Pasted image 20230610125533.png]]
Y aquí nos encontramos con un script donde tenemos permisos de escritura, llamado cow.sh:
![[Pasted image 20230610125613.png]]
Y esto es lo que hace el script:
![[Pasted image 20230610125727.png]]
Por tanto ahora podemos escalar privilegios editando este script y añadiendo código para que nos envíe una shell reversa:
![[Pasted image 20230610130336.png]]
Y nos ponemos en escucha con netcat y recibimos la reverse shell gracias a que este script se ejecuta por estar como tarea cron; y recibimos una shell como el usuario mat:
![[Pasted image 20230610130410.png]]
Y obtenemos la flag 5:
![[Pasted image 20230610130626.png]]
Y tenemos esta nota donde nos dice que tenemos permisos para ejecutar código python como root:
![[Pasted image 20230610130701.png]]
En la carpeta scripts tenemos dos scripts de python donde tenemos permisos de escritura y ejecución:
![[Pasted image 20230610130812.png]]
Además, si ejecutamos el comando sudo -l nos encontramos con que podemos ejecutar el script will_script.py como will:
![[Pasted image 20230610132146.png]]
Y este es el contenido de ambos scripts, donde vemos que uno llama al otro:
![[Pasted image 20230610132451.png]]
Por tanto, editamos el contenido de cmd.py para poner un comando que lance una bash como el usuario Will:
![[Pasted image 20230610132405.png]]
Por tanto ejecutamos el comando que nos salía al poner sudo -l y nos convertirmos en will:
```bash
sudo -u will /usr/bin/python3 /home/mat/scripts/will_script.py *
```
![[Pasted image 20230610132635.png]]
Por último, vemos que el usuario Will se encuentra dentro del grupo adm:
![[Pasted image 20230610132749.png]]
Por tanto vamos a buscar archivos que se encuentren dentro del grupo adm con el comando find:
```bash
find / -group adm 2>/dev/null
```
Y nos encuentra una key en base64:
![[Pasted image 20230610132949.png]]
Si visualizamos esta key, vemos que está en base64, pero podemos decodificarla:
![[Pasted image 20230610133048.png]]
Por tanto vamos a pegar este contenido de la clave id_rsa en nuestra máquina atacante y lo usamos para acceder vía ssh a la máquina víctima como root:
![[Pasted image 20230610133240.png]]
