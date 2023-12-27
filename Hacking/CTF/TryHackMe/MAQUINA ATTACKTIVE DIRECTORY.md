## (enum4linux, kerbrute enumerar usuarios, ASREPRoasting obtener TGT, john the ripper, smbclient para listar recursos compartidos, secredump dumpear hashes y pass the hash)
Haremos los reconocimientos de nmap y tenemos todos estos puertos abiertos:
![[Pasted image 20230425145638.png]]
![[Pasted image 20230425150244.png]]
Vemos que el nombre del dominio es spookysec.local, por lo que tendremos que añadir esto al fichero /etc/hosts:
![[Pasted image 20230425150428.png]]
Lo primero inspeccionamos el puerto 80 y vemos que está corriendo un servicio de IIS:
![[Pasted image 20230425145740.png]]
Y en cuanto al puerto 445, vamos a inspeccionar el servicio SMB con enum4linux, la cual es una herramienta utilizada para extraer información de los hosts del dominio y de samba:
![[Pasted image 20230425150510.png]]
Y encontramos el nombre del dominio, el cual es THM-AD:
![[Pasted image 20230425151049.png]]
Ahora el siguiente paso será enumerar usuarios válidos del dominio; y para ello usaremos [[Kerbrute]]. Kerbrute es una herramienta para realizar un ataque de fuerza bruta sobre Kerberos en una máquina víctima. Por lo que este es su repositorio:
![[Pasted image 20230425150757.png]]
Y procedemos con su instalación, por lo que en primer lugar lo clonamos y lo instalamos de esta forma, simplemente ejecutando el fichero requirements para que queden instaladas todas las dependencias:
![[Pasted image 20230425150924.png]]
Y con el comando kerbrute ya vemos las instrucciones de cómo utilizarlo:
![[Pasted image 20230425151011.png]]
Ahora tendremos que tener un diccionario de usuarios y otro de contraseñas (que podemos usar los que nos proporcionan en la propia web de tryhackme):
![[Pasted image 20230425154502.png]]
![[Pasted image 20230425154603.png]]
## BÚSQUEDA DE USUARIOS DOMINIO
Y este es el parámetro que debemos usar para hacer una búsqueda de usuarios válidos del dominio:
```bash
python3 kerbrute.py -users userlist.txt -passwords passwordlist.txt -domain spookysec.local -t 100
```
Y en el resultado vemos varios usuarios, aunque hay uno de ellos que no requiere autenticación:
![[Pasted image 20230425154919.png]]
Ahora en este punto podemos hacer uso de [[ASREPRoasting]] para enviar una petición como este usuario y recibir el TGT con la contraseña hasheada del mismo:
![[Pasted image 20230425155219.png]]
Y ahora este hash que obtenemos podemos crackearlo con john the ripper:
![[Pasted image 20230425155713.png]]
Y ya tenemos las credenciales del usuario svc-admin:
```python
usuario --> svc-admin
contraseña --> management2005
```
Ahora que ya tenemos las credenciales de este usuario, podemos usar [[SMBclient]] para listar los recursos compartidos (también poniendo la IP funcionaría):
![[Pasted image 20230425155910.png]]
![[Pasted image 20230425160001.png]]
Y con el comando get ya tenemos el archivo:
![[Pasted image 20230425160051.png]]
Y la credencial que está dentro de este fichero tenemos que convertirla porque viene en base64:
**![[Pasted image 20230425160525.png]]**
```bash
backup@spookysec.local:backup2517860
```
Ahora que hemos visto que en el recurso compartido del usuario backup pudimos descargar un hash, vamos a utilizar una herramienta que se llama secretdump de impacket para dumpear los hashes de todos los usuarios del dominio [[Impacket-secretdumps]]:
![[Pasted image 20230425162401.png]]
Y ahora que tenemos el hash del usuario administrador, podemos loguearnos directamente y obtener una reverse shell proporiconando simplemente este hash, lo cual se conoce comopass the hash, [[Pass The Hash, impacket psexec y wmiexec]]:
![[Pasted image 20230425163009.png]]
Y ya somos el usuario administrador y máquina completa:
![[Pasted image 20230425163218.png]]
