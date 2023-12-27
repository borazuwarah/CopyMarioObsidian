Hacemos el reconocimiento de puertos con nmap:
![[Pasted image 20231222111447.png]]
Esto corre por el puerto 80:
![[Pasted image 20231222111550.png]]
Hacemos fuzzing web y nos encontramos con un directorio robots.txt:
![[Pasted image 20231222111729.png]]
Donde si accedemos a él nos encontramos con un subdominio:
![[Pasted image 20231222111745.png]]
Modificamos el /etc/hosts:
![[Pasted image 20231222111821.png]]
Y podemos hacer un ataque de transferencia de zona de la siguiente forma para detectar subdominios, ya que en el reconocimiento de nmap veíamos que el puerto 53 (protocolo DNS) estaba habilitado:
```bash
dig axfr hunterzone.nyx @192.168.0.40
```
![[Pasted image 20231222112441.png]]
Pero tras realizar varias pruebas, el dominio que nos interesa es el de devhunter.nyx:
![[Pasted image 20231222112759.png]]
Por lo que lo añadimos al /etc/hosts:
![[Pasted image 20231222112818.png]]
Y ahora si hacemos un ataque en busca de subdominios con wfuzz nos encontramos con el subdominio files:
```bash
wfuzz -c --hc=404 --hl=367 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.devhunter.nyx" -u 192.168.0.40
```
![[Pasted image 20231222112932.png]]
Lo añadimos al /etc/hosts:
![[Pasted image 20231222112955.png]]
Y dentro de este nuevo subdominio nos encontramos con un lugar para subir archivos:
![[Pasted image 20231222113028.png]]
Si tratamos de subir un archivo en php vemos que no lo permite:
![[Pasted image 20231222113233.png]]
Por lo que si queremos conocer qué extensiones de archivo acepta, debemos de utilizar el intruder de burpsuite e ir probando; por lo que primero interceptamos la petición:
![[Pasted image 20231222113415.png]]
Seleccionamos la extensión del archivo y utilizamos el siguiente diccionario de extensiones para ir probando cual es válida:
```bash
https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/File-Extensions-Wordlist.txt
```
![[Pasted image 20231224111803.png]]
Establecemos dónde queremos que vaya aplicando la sustitución:
![[Pasted image 20231224111840.png]]
Cargamos dentro del intruder, en la parte de payload el diccionario de las extensiones:
![[Pasted image 20231224111920.png]]
Y en grep extract capturamos el mensaje de error:
![[Pasted image 20231224112223.png]]
Vemos que podemos subir tanto archivos .png como .htaccess al servidor; y los ficheros htaccess sirven para controlar algunas configuraciones en servidores apache, entre las cuales podremos controlar las extensiones permitidas, por lo que vamos a hacer que nos permita subir archivos .png:
![[Pasted image 20231224114526.png]]
```bash
AddType application/x-httpd-php .png
```
![[Pasted image 20231224113559.png]]
Y lo mismo con la webshell de extensión .png:
![[Pasted image 20231224113324.png]]
Subimos ambos archivos y lo logramos:
![[Pasted image 20231224113124.png]]
Una vez subido, verificamos que existe el directorio de /uploads:
![[Pasted image 20231224113159.png]]
Y ahora llamamos a la webshell de .png y vemos que podemos ejecutar comandos:
![[Pasted image 20231224113653.png]]
Url encodeamos la reverse shell con burp suite:
![[Pasted image 20231224113853.png]]
Y lo pegamos en el navegador para obtener la intrusión:
![[Pasted image 20231224113921.png]]
Una vez dentro, podemos ver que podemos ejecutar/usr/bin/bsh como root; pero cuando ejecutamos esto vemos que realmente se llama BeanShell:
![[Pasted image 20231224114143.png]]
Y en esta web vemos cómo podemos ejecutar comandos en eso:
```bash
http://www.beanshell.org/manual/quickstart.html
```
![[Pasted image 20231224114301.png]]
Lo ejecutamos y cambiamos de permisos a la bash:
![[Pasted image 20231224114421.png]]
Y ahora al ejecutar el comando bash -p podremos escalar a root:
![[Pasted image 20231224114452.png]]
