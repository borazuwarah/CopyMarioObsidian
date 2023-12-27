Hacemos el reconocimiento con nmap:
![[Pasted image 20230702185911.png]]
Y esta es la página web:
![[Pasted image 20230702185948.png]]
Hacemos fuzzing y nos encontramos con dos directorios, img y r:
![[Pasted image 20230702190540.png]]
Y parece que tenemos que seguir haciendo fuzzing sobre r:
![[Pasted image 20230702190612.png]]
Y ahora nos encuentra a:
![[Pasted image 20230702190645.png]]
![[Pasted image 20230702190657.png]]
Y ahora hacemos así sucesivamente con el resto de palabras:
```bash
wfuzz --hc 404 -c -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u 'http://10.10.165.154/r/a/b/b/i/t/FUZZ'
```
Y con esta url nos encuentra esta web:
![[Pasted image 20230702191018.png]]
Podemos mirar el código fuente de la página y nos encontramos con unas credenciales:
![[Pasted image 20230702191109.png]]
Probamos las credenciales vía ssh y entramos:
```bash
alice:HowDothTheLittleCrocodileImproveHisShiningTail
```
![[Pasted image 20230702191233.png]]
Lo curioso es que si queremos entrar en el directorio root, tenemos los permisos y ahí se encuentra la flag de user:
![[Pasted image 20230702193532.png]]
También si hacemos un sudo -l y ponemos la contraseña, vemos que como rabbit podemos ejecutar el script de walrus_and_the_carpenter.py:
![[Pasted image 20230702192836.png]]
Si miramos el contenido del script, vemos que al principio del todo pone import random:
![[Pasted image 20230702193710.png]]
Por tanto podemos crear un fichero llamado random.py y así estaríamos ejecutando como rabbit el contenido de archivo random.py:
![[Pasted image 20230702193826.png]]
Ahora vamos a ejecutar el script walrus_and_the_carpenter.py, donde vamos a ver que nos convertiremos en rabbit:
![[Pasted image 20230702193918.png]]
Y dentro del directorio de rabbit, vemos que se encuentra un archivo de dumpeo llamado teaParty:
![[Pasted image 20230702194045.png]]
Nos compartimos este archivo con nuestra máquina local para analizarlo:
![[Pasted image 20230704102027.png]]
Si lo analizamos en local con el comando strings vemos que está ejecutando el comando date como el usuario Hatter:
![[Pasted image 20230704102409.png]]
Por tanto podemos manipular el path para conseguir que Hatter ejecute el comando date con el comando que queramos y poder convertirnos en Hatter:
![[Pasted image 20230704103311.png]]
Y ahora si ejecutamos el binario de .teaParty, se habrá ejecutado el comando date como el usuario hatter, el cual contiene el código para convertirnos en este usuario:
![[Pasted image 20230704103251.png]]
Dentro del directorio Hatter nos encontramos con una .txt con una contraseña, pero es la del usuario hatter que es el usuario que ya somos:
```bash
WhyIsARavenLikeAWritingDesk?
```
![[Pasted image 20230704103746.png]]
Comprobamos las capabilities:
![[Pasted image 20230704104147.png]]
Buscamos en gtfobins la capabilitie de perl:
![[Pasted image 20230704104317.png]]
Y vamos al apartado capabilities para seguir las instrucciones:
![[Pasted image 20230704104340.png]]
