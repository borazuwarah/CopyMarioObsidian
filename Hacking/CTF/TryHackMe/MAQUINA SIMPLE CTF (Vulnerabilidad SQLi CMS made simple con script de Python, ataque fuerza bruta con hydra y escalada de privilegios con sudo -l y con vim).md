Hacemos un escaneo de nmap y encontramos estos puertos abiertos:
![[Pasted image 20230331115709.png]]
![[Pasted image 20230331115752.png]]
Lo primero destacable es que el puerto ftp permite conexión como anonymous:
![[Pasted image 20230331115831.png]]
Por lo que nos conectamos:
![[Pasted image 20230331120011.png]]
En cuanto a la web, vemos que se trata de una plantilla de apache por defecto:
![[Pasted image 20230331120052.png]]
Si hacemos fuzzing vemos que nos encuentra un directorio que se llama simple:
![[Pasted image 20230331120329.png]]
Lo probamos y vemos que funciona:
![[Pasted image 20230331120346.png]]
Y en la parte de abajo podemos ver la versión de este cms:
![[Pasted image 20230331120658.png]]
Buscams en searchsploit y vemos que hay una vulnerabilidad para esta versión:
![[Pasted image 20230331120744.png]]
Y si entramos dentro de este exploit ya obtenemos la vulnerabilidad pública:
![[Pasted image 20230331120943.png]]
Investigamos y vemos que por github tenemos un exploit parecido pero que funciona con python3, ya que el de virustotal funciona con python2, por lo que usaremos este mejor:
![[Pasted image 20230331123948.png]]
Si lo ejecutamos vemos las instrucciones:
![[Pasted image 20230331121619.png]]
Ejecutamos esta herramienta con las instrucciones anteriores y utilizando el diccionario kaonashi.txt de esta forma, ya que con rockyou es posible que nos de algún problema:
![[Pasted image 20230331124006.png]]
Y nos encuentra unas credenciales:
![[Pasted image 20230331124046.png]]
Aunque con hydra simplemente con tener el usuario también nos habría funcionado (aunque poniendo el puerto 2222 que en esta máquina el protocolo ssh no funciona por el 22, sino por este otro puerto):
![[Pasted image 20230331124609.png]]
![[Pasted image 20230331124638.png]]
Vamos a conectarnos por SSH con estas credenciales y especificando el puerto y ya estamos dentro:
![[Pasted image 20230331124809.png]]
![[Pasted image 20230331124833.png]]
Si hacemos un sudo -l vemos que podemos ejecutar vim como root [[14 - Escalada de Privilegios con VIM]]:
![[Pasted image 20230331124952.png]]
En este punto se nos dice que podemos ejecutar vim como root, por lo que podemos asignar permisos de root a la bash y luego invocarla para ser usuario root. Por lo que ejecutamos el comando sudo vim hola:
![[Pasted image 20230401005157.png]]
Y asinamos permisos a la bash:
![[Pasted image 20230401005240.png]]
Damos  ENTER y ahora escribimos esto otro:
![[Pasted image 20230401005310.png]]
Y ya somos usuarios root:
![[Pasted image 20230401005330.png]]
