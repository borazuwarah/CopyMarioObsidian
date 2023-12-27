Lo primero será hacer las comprobaciones de siempre, haciendo un ping y luego nmap:
![[Pasted image 20230329172412.png]]
![[Pasted image 20230329172415.png]]
![[Pasted image 20230329172419.png]]
![[Pasted image 20230329172422.png]]
Lo primero es que vemos que el puerto ftp está abierto y el usuario anonymous está habilitado, por lo que podemos conectarnos con el usuario anonymous y sin contraseña:
![[Pasted image 20230329172434.png]]
Una vez dentro y navegando, vemos que hay un fichero que se llama user.txt, vamos a descargarlo con get:
![[Pasted image 20230329172445.png]]
Y tiene la flag:
![[Pasted image 20230329172453.png]]
Vamos a ganar acceso al sistema, hacer escalada de privilegios, para ello primero vamos a entrar en la página web, ya que tiene abierto el puerto 80:
![[Pasted image 20230329172508.png]]
Y en la parte de abajo nos encontramos con este aviso:
![[Pasted image 20230329172517.png]]
Por tanto vamos a inspeccionar si PRTG está instalado en la máquina, para ello buscamos entre los archivos ocultos con el comando ls -a:
![[Pasted image 20230329172529.png]]
Tras consultar en Internet, a través de este enlace https://www.reddit.com/r/sysadmin/comments/835dai/prtg_exposes_domain_accounts_and_passwords_in/, se explica que para X versiones se detectó que la aplicación guardaba backups con las contraseñas en texto plano. Por tanto vamos a buscar este archivo; y vamos a navegar dentro de ProgramData y vamos a estos directorios:
![[Pasted image 20230329172553.png]]
Y nos encontramos con todos estos archivos:
![[Pasted image 20230329172603.png]]
Nos descargamos todos los archivos de PRTG:
![[Pasted image 20230329172609.png]]
Y vemos que tenemos el PRTG configuration y la versión con .bak, que es el backup de las credenciales que se hacían cada cierto tiempo; por tanto con el comando diff vamos a inspeccionar las diferencias entre estos archivos:
![[Pasted image 20230329172620.png]]
Y este es el resultado, donde vemos unas credenciales:
![[Pasted image 20230329172627.png]]
Por tanto vamos a probarla en el servidor web que tiene la máquina, pero en la contraseña en vez del 2018 ponemos el 2019, ya que hay un año de diferencia en la creación de estos archivos:
![[Pasted image 20230329172638.png]]
Probamos las credenciales y entramos:
![[Pasted image 20230329172647.png]]
![[Pasted image 20230329172649.png]]
