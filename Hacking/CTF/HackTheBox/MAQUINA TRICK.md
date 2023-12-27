Haremos los mismos reconocimientos de siempre:
![[Pasted image 20230215194055.png]]
Como vemos que tiene abierto el puerto 80 vamos a entrar en la web y analizarlo con whatweb:
![[Pasted image 20230215194106.png]]
![[Pasted image 20230215194112.png]]
Podemos buscar subdominios ocultos que la IP no esté apuntando, por tanto podemos hacer un ataque axfr (ataque de transferencia de zona), utilizando el comando dig, y vemos que encontramos un dominio oculto llamado preprod-payroll.trick.htb:
![[Pasted image 20230215194123.png]]
Y lo añadimos al /etc/hosts:
![[Pasted image 20230215194131.png]]
Y accedemos a esta extraña web:
![[Pasted image 20230215194144.png]]
Podemos probar en hacer una inyección SQL básica y vemos que podremos entrar:
![[Pasted image 20230215194156.png]]
![[Pasted image 20230215194203.png]]
En la parte de Users vemos que podemos modificar el usuario, y se puede ver el Username y la contraseña con asteriscos:
![[Pasted image 20230215194219.png]]
Si inspeccionamos el código fuente de la página, veremos cómo podemos modificar el html para quitar esos asteriscos y así poder ver la contraseña:
![[Pasted image 20230215194229.png]]
Ahora lo que haremos será comprobar subdominios ocultos en esta web, ya que vemos que trick.htb se mantendrá pero la parte de preprod-payroll es posible que cambie y que haya otras webs con distintos nombre, por tanto lo haremos con WFUZZ, pero la configuración será un poco diferente porque las combinaciones no van al final de la URL, sino en medio:
![[Pasted image 20230215194239.png]]
Y nos encuentra un subdominio que se llama marketing:
![[Pasted image 20230215194248.png]]
Lo añadimos al /etc/hosts:
![[Pasted image 20230215194256.png]]
Y si lo ponemos en el navegador nos encuentra esto:
![[Pasted image 20230215194304.png]]
Y vamos a probar si es vulnerable a path traversal, y vemos que sí:
![[Pasted image 20230215194314.png]]
Ahora podremos obtener la RSA para poder acceder por ssh, que estará dentro del directorio del usuario michael donde se almacenan las id_rsa:[[Acceder id_rsa para iniciar sesión por SSH a la máquina]]
![[Pasted image 20230215194322.png]]
Guardamos la clave:
![[Pasted image 20230215194331.png]]
Le damos los permisos propios de una id_rsa:
![[Pasted image 20230215194340.png]]
Y nos conectamos por ssh de esta manera utilizando la id_rsa:
![[Pasted image 20230215194347.png]]
