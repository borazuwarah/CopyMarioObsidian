## (Vulnerabilidad SCF Shell Command File para obtener hashes de credenciales de usuario de una impresora, crear recurso compartido con impacket-smbserver y evil-winrm para ganar acceso remoto una vez tengamos las credenciales)

Haremos como siempre el primer reconocimiento con nmap:
![[Pasted image 20230312215957.png]]
Vemos que tiene montado un servidor web, por tanto vamos a acceder y vemos que nos pide unas credenciales; y si probamos con admin/admin accederemos y veremos que se trata de una impresora por red:
![[Pasted image 20230312220012.png]]
Podemos ver que se trata de un Windows 10 si ejecutamos crackmapexec: [[Crackmapexec#Detectar dominio y hostname con crackmapexec]]
![[Pasted image 20230312220021.png]]
Y esto es lo que nos reporta whatweb:
![[Pasted image 20230312220030.png]]
Vemos que tenemos un apartado para subir un archivo:
![[Pasted image 20230312220037.png]]
Por tanto podemos buscar un archivo malicioso para subir; y existe una vulnerabilidad que se llama SCF, el cual consiste en subir un archivo malicioso y cuando un usuario revise el archivo subido, a nosotros nos llegará el hash con las credenciales si estamos escuchando por smb; por tanto si buscamos en google scf malicious file accederemos a esta web:
![[Pasted image 20230312220053.png]]
Copiamos este texto y lo modificamos primero poniendo nuestra dirección IP y después el nombre del recurso compartido que crearemos luego, por tanto cogemos este texto y lo pegamos en un archivo scf con dichas configuraciones, marcando un archivo pwned.ico que no existe:
![[Pasted image 20230313004917.png]]
![[Pasted image 20230313004958.png]]
Ahora nos montamos el recurso compartido con este archivo:
![[Pasted image 20230312220117.png]]
Y ahora en la web subimos este fichero file.scf, que al subirlo como en su interior está configurado para que cargue el mismo fichero que tenemos montado por internet:
![[Pasted image 20230312220128.png]]
Y vemos ahora nos llega un usuario y un hash a nuestro recurso compartido, que debe ser el usuario que revisa manualmente el archivo subido:
![[Pasted image 20230312220138.png]]
Este hash lo vamos a cargar dentro de un documento llamado hash:
![[Pasted image 20230313005416.png]]
![[Pasted image 20230312220145.png]]
Ahora vamos a romper este hash con john the ripper, donde encontramos el usuario y la contraseña:[[John The Ripper]]
![[Pasted image 20230312220154.png]]
Comprobamos con crackmapexec que las credenciales sean correctas por el protocolo smb, y vemos que sí: [[Crackmapexec#Comprobar y validar credenciales con crackmapexec]]
![[Pasted image 20230312220203.png]]
Ahora vamos a comprobar lo mismo pero por winrm, y vemos que también:
![[Pasted image 20230312220211.png]]
Y ahora que tenemos estas credenciales, con evil-winrm ya podemos conectarnos de esta manera, aprovechándonos del protocolo Windows Remote Management, el cual permite el control de sistemas Windows de forma remota, gracias a las credenciales que tenemos:
![[Pasted image 20230312220219.png]]
Y navegando por los directorios ya tenemos la flag:
![[Pasted image 20230312220226.png]]
Ahora para escalar los privilegios, vamos a hacer una enumeración de los procesos del sistema con el comando ps, donde vemos uno que se llama spoolsv, el cual es vulnerable:

![[Pasted image 20230314112913.png]]
Si buscamos en google como escalar privilegios explotando el servicio spoolsv, nos encontramos con este repositorio de github y con la vulnerabilidad pública:
![[Pasted image 20230314113242.png]]
![[Pasted image 20230314113254.png]]
Y nos dice que debemos ejecutar este script dentro de la máquina víctima:
![[Pasted image 20230314113349.png]]
Por tanto vamos a clonarnos este repositorio y lo vamos a compartir con la máquina víctima con impacket-smbserver para poder ejecutarlo allí:
![[Pasted image 20230314113827.png]]
Y lo descargamos en la máquina víctima:
![[Pasted image 20230314113844.png]]
Y ahora vamos a ejecutarlo siguiendo las instrucciones del repositorio de github, pero al ejecutar el primer comando nos lanza un error:
![[Pasted image 20230314114117.png]]
Por tanto vamos a mirar por google como quitar esta restricción y poder ejecutar este script; y nos encontramos el link a stackoverflow:
![[Pasted image 20230314114256.png]]
![[Pasted image 20230314114311.png]]
Por tanto vamos a ejecutar este comando dentro de la víctima y ver si así ya se nos arregla el error; y vemos que ahora ya no nos sale ese error:
![[Pasted image 20230314114441.png]]
Seguimos ejecutando el resto de los comandos del script:
![[Pasted image 20230314114607.png]]
![[Pasted image 20230314114654.png]]
Ahora vamos a ver el usuario que nos ha creado este script utilizando el comando net user, para ver sus características; y vemos que se encuentra dentro del grupo de administrador:
![[Pasted image 20230314114841.png]]
Ahora que ya tenemos un nuevo usuario con permisos de administrador, vamos a conectarnos a este nuevo usuario otra vez con la herramienta evil-winrm; y vemos que se conecta bien:
![[Pasted image 20230314115112.png]]
Y vemos que con este nuevo usuario podemos entrar dentro de la carpeta del Administrador y acceder a la flag de root:
![[Pasted image 20230314115214.png]]



