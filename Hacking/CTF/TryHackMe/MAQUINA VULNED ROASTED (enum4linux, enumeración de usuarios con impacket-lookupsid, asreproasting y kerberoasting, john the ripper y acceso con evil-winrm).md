Hacemos el reconocimiento con nmap y nos encontramos con todos estos puertos abiertos:
![[Pasted image 20230701103021.png]]
Nos encontramos con un entorno de directorio activo, por lo que vemos un dominio que tenemos que añadirlo en el /etc/hosts:
![[Pasted image 20230701103241.png]]
Tenemos el servicio de RPC corriendo sobre el puerto 135, por lo que vamos a conectarnos usando rpcclient para conectarnos y haciendo uso de una null session:
![[Pasted image 20230701103402.png]]
Pero no tenemos permisos para listar ni usuarios ni grupos:
![[Pasted image 20230701103512.png]]
Ahora enumeramos el puerto 445 haciendo uso de una null session usando smbmap:
![[Pasted image 20230701103853.png]]
Nos conectaremos a los dos recursos compartidos donde tendremos permisos y descargaremos todo en nuestra máquina local:
![[Pasted image 20230701104341.png]]
![[Pasted image 20230701104351.png]]
Teniendo dos carpetas en nuestra máquina con los archivos:
![[Pasted image 20230701104413.png]]
Y analizando estas carpetas, tenemos documentos de texto con distintos usuarios, por tanto los vamos a ir guardando en un txt:
![[Pasted image 20230701104728.png]]
![[Pasted image 20230701104811.png]]
Por tanto ahora ejecutamos enum4linux para comprobar estos usuarios, pero vemos que no existen:
![[Pasted image 20230701105934.png]]
Pero hay una herramienta llamada lookupsid que sí nos permite hacer esta enumeración y encontrar usuarios válidos, por lo que vamos a ejecutarla de esta forma y nos encontramos con distintos usuarios bajo la categoría de SidTyperUser:
![[Pasted image 20230701110242.png]]
Ahora vamos a filtrar sólo aquellos que tengan esa categoría para obtener los usuarios:
![[Pasted image 20230701110509.png]]
Ahora todo esto lo guardamos en el archivo de nombres que teníamos anteriormente, aplicando expresiones regulares para guardar los usuarios:
![[Pasted image 20230701110749.png]]
Y ahora aplicamos las regex sobre el archivo convertir:
![[Pasted image 20230701110807.png]]
Ya con estos usuarios, vamos a probar a realizar un [[ASREPRoasting]] de esta forma con impacket-GetNPUsers, y nos encontramos con el hash del usuario t-skid:
![[Pasted image 20230701111335.png]]
Copiamos este hash y le hacemos un ataque de fuerza bruta:
![[Pasted image 20230701111738.png]]
Vemos que tenemos el usuarios skid con la posible contraseña tj072889*, por lo que vamos a probarlos con [[Crackmapexec]] y efectivamente son válidos:
![[Pasted image 20230701111919.png]]
Por tanto ahora vamos a hacer un [[Hacking Kerberos]] usando la herramienta impacket-GetUserSPNs para tratar de obtener un TGS:
```bash
impacket-GetUserSPNs 'vulnnet-rst.local/t-skid:tj072889*' -request
```
![[Pasted image 20230701112719.png]]
Encontramos el TGS del usuario “enterprise-core-vn”. Hacemos lo mismo que con el ataque ASREPRoast, primero cogemos el hash y lo guardamos en un archivo y luego procedemos a crackearlo con John The Ripper; y vemos que nos encuentra la contraseña ry=ibfkfv,s6h, para este usuario:
![[Pasted image 20230701112827.png]]
En este punto podemos intentar acceder a la máquina víctima a través de evil-winrm con el usuario enterprise-core-vn que es el que obtuvimos anteriormente:
```bash
evil-winrm -i vulnnet-rst.local -u enterprise-core-vn -p 'ry=ibfkfv,s6h,'
```
Y obtenemos acceso:
![[Pasted image 20230701115521.png]]
Obtenemos la flag de user:
![[Pasted image 20230701115654.png]]
De todos modos vamos a intentar acceder con este usuario a los recursos compartidos:
![[Pasted image 20230712110609.png]]
Por tanto vamos a entrar en NETLOGON:
![[Pasted image 20230712111502.png]]
Vamos a acceder por smbclient a los recursos compartidos para descargarnos el archivo .vbs:
![[Pasted image 20230712111716.png]]
Nos descargamos el archivo:
![[Pasted image 20230712111744.png]]
Si visualizamos el contenido de este archivo, nos encontramos con unas credenciales:
![[Pasted image 20230712111814.png]]
```php
a-whitehat:bNdKVkjv3RR9ht
```
Por tanto, con estas credenciales, podemos probar en acceder con wmiexec:
```php
evil-winrm -i vulnnet-rst.local -u a-whitehat -p "bNdKVkjv3RR9ht"
```
Y con esto deberíamos estar dentro y poder obtener la flag de user, aunque es posible que la máquina tenga errores y no funcione el acceso:
![[Pasted image 20230712114934.png]]
De todos modos, con estas credenciales podemos usar secretsdump de impacket para listas hashes de usuarios del sistema, y vemos que podemos listar varios de ellos, entre los que se encuentra el del administrador:
![[Pasted image 20230712112250.png]]
Y este es el hash del administrador que podemos usarlo en evil-winrm:
```php
c2597747aa5e43022a3a3049a3c3b09d
```
![[Pasted image 20230712112501.png]]
Por último, accedemos por evil-winrem o wmiexec con estas credenciales y ya deberíamos estar dentro como el usuario administrador y acceder a la flag de system (pass the hash):
```php
impacket-wmiexec vulnnet-rst.local/Administrator@10.10.203.119 -hashes aad3b435b51404eeaad3b435b51404ee:c2597747aa5e43022a3a3049a3c3b09d
```
![[Pasted image 20230712113833.png]]
Navegamos por los directorios y nos ubicamos en el escritorio para acceder a la flag de system:
![[Pasted image 20230712114605.png]]
