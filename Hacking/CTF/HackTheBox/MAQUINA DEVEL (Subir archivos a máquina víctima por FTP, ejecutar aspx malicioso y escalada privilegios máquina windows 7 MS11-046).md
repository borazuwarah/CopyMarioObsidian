Haremos los reconocimientos de siempre con nmap:
![[Pasted image 20230312124826.png]]
Tiene el puerto ftp abierto con el login como anonymous habilitado, por tanto vamos a entrar:
![[Pasted image 20230312124837.png]]
Y esta es la web con su whatweb:
![[Pasted image 20230312124844.png]]
![[Pasted image 20230312124848.png]]
Lo interesante es que podemos cargar archivos desde ftp ya que tenemos acceso, por ejemplo vamos a cargar un html:
![[Pasted image 20230312124858.png]]
![[Pasted image 20230312124901.png]]
Y vemos que podemos acceder a él desde la máquina:
![[Pasted image 20230312124908.png]]
Si investigamos sobre IIS server, podemos ver que puede procesar archivos con formato .aspx, por tanto podemos subir un archivo malicioso aspx, el cual lo podemos crear con msfvenom:
![[Pasted image 20230312124919.png]]
Lo subimos al servidor ftp:
![[Pasted image 20230312124926.png]]
Ahora visitamos esta ruta desde el navegador y ya habremos recibido la shell reversa:
![[Pasted image 20230312124933.png]]
![[Pasted image 20230312124937.png]]
Podemos ejecutar el comando systeminfo para conocer características de la máquina víctima poniendo este comando:
```powershell
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type”
```
![[Pasted image 20230312124950.png]]
Investigando, vemos que esta máquina es vulnerable también a MS11-046, donde vemos que los windows antiguos son vulnerables:
![[Pasted image 20230312125001.png]]
![[Pasted image 20230312125005.png]]
Y lo podemos descargar desde aquí:
![[Pasted image 20230312125014.png]]
Una vez descargado este archivo nos lo compartimos con la máquina víctima a través de un recurso compartido:
![[Pasted image 20230312125025.png]]
Y ahora desde la máquina víctima tenemos que ubicarnos en una ruta donde tengamos permisos de escritura para poder descargar el archivo. Lo descargamos y ejecutamos:
![[Pasted image 20230312125048.png]]
Ahora simplemente lo ejecutamos y ya somos usuarios root:
![[Pasted image 20230312125059.png]]
