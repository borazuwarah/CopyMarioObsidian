Hacemos el escaneo de nmap:
![[Pasted image 20230704112110.png]]
Esta es la página web:
![[Pasted image 20230704112145.png]]
Y abajo del todo vemos un lugar donde subir un CV:
![[Pasted image 20230704112215.png]]
Si hacemos fuzzing nos encontramos con estos 3 directorios:
![[Pasted image 20230704112352.png]]
Y si hacemos lo mismo pero con extensiones de php, nos encontramos con esto otro:
![[Pasted image 20230704113014.png]]
En este diretorio upload.php, vemos lo siguiente:
![[Pasted image 20230704113040.png]]
Si miramos el código fuente de esta página, vemos que acepta extensiones de .pdf:
![[Pasted image 20230704113125.png]]
Si intentamos subir cualquier archivo, siempre nos da el mismo error:
![[Pasted image 20230704114213.png]]
No obstante, si hacemos fuzzing por extensiones de archivos, podemos intuir que posiblemente haya algo como pdf.php, por lo que vamos a probar si existe:
```bash
wfuzz -c -z file,/opt/SecLists/Discovery/Web-Content/common.txt --hc 404,403 http://10.10.212.207/cvs/FUZZ.pdf.php
```
![[Pasted image 20230828174912.png]]
![[Pasted image 20230704115049.png]]
Probamos en ejecutar un comando y funciona:
![[Pasted image 20230704115227.png]]
Por lo que ahora adquirimos acceso a la máquina, pero al internarlo obtenemos este error con netcat:
![[Pasted image 20230704115424.png]]
Aunque vemos que si intentamos hacer el tratamiento de la TTY nos da un error un poco extraño:
![[Pasted image 20230704115612.png]]
Podemos ubicarnos dentro del directorio personal de lachlan, donde veremos la flag de user y también un -bash_history con cierto contenido:
![[Pasted image 20230704120247.png]]
Y si probamos estas credenciales, vemos que son correctas:
```bash
lachland:thisistheway123
```
![[Pasted image 20230717160005.png]]Pero vemos que a los pocos segundos nos expulsa de la conexión:
![[Pasted image 20230717160044.png]]
Esto es debido a una tarea cron que se ejecuta como root, la cual podemos visualizar en esta parte:
![[Pasted image 20230717160150.png]]
Pero vemos que root ejecuta el comando pkill:
![[Pasted image 20230717160338.png]]
Además vemos que tenemos un directorio llamado /bin dentro del directorio de lachlan:
![[Pasted image 20230717161407.png]]
Por lo que además vemos que si creamos un archivo dentro de este directorio, se estará ejecutando como root desde la tarea de crontab, por lo que vamos a entrar por ssh y muy rápido inyectamos el comando malicioso para cambiar los permisos de la bash:
```bash
echo "chmod u+s /bin/bash" > bin/pkill ; chmod 777 bin/pkill
```
![[Pasted image 20230717161512.png]]
