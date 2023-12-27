Las credenciales para desbloquear esta máquina son:
```bash
pinocho:f4k3n3ws
```
Hacemos el reconocimiento con nmap:
![[Pasted image 20230725104512.png]]
Y esta es la web:
![[Pasted image 20230725104538.png]]
Podemos hacer fuzzing por extensión de archivos y vemos tanto la flag de user como la de root, además de un rovots.txt:

![[Pasted image 20230725105031.png]]
![[Pasted image 20230725105118.png]]
![[Pasted image 20230725105130.png]]
Si buscamos con searchsploit, vemos que esta versión de drupal es vulnerable:
![[Pasted image 20230725105221.png]]
No obstante, ya que vemos esa dirección en el robots.txt, vamos a intentar acceder a esa ruta:
![[Pasted image 20230725105815.png]]
Podemos usar metasploit haciendo una búsqueda del drupalgeddon2:
![[Pasted image 20230725123015.png]]
Establecemos el RHOSTS y ya estamos dentro:
![[Pasted image 20230725123032.png]]
Y podemos cambiar a una shell para interactuar más cómodamente con el comando shell y luego el comando bash -i:
![[Pasted image 20230725123137.png]]
Vamos a mandarnos una reverse shell con netcat para poder hacer el tratamiento de la TTY; y para ello lo haremos con la web de revshells:
![[Pasted image 20230725124128.png]]
![[Pasted image 20230725124147.png]]
Y recibimos la conexión para hacer correctamente el tratamiento de la TTY:
![[Pasted image 20230725124210.png]]
