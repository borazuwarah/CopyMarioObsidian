Con esta librería podemos conectarnos por FTP con Python. Por ejemplo para una conexión simple lo haríamos de esta forma:
```python
from ftplib import FTP

ftp = FTP('192.168.0.18')
ftp.login(user='mario', passwd='123123')
```
Y vemos que la conexión fue exitosa:
![[Pasted image 20230417134751.png]]
## CAMBIAR DIRECTORIO DE TRABAJO
Ahora vamos a cambiar el directorio de trabajo:
```python
from ftplib import FTP

ftp = FTP('192.168.0.18')
ftp.login(user='mario', passwd='123123')

# Cambiar el directorio de trabajo
ftp.cwd('/home/mario/Escritorio')

# Listar el contenido del directorio actual
ftp.retrlines('LIST')

# Cerrar la conexión
ftp.quit()
```
Y vemos que ahora nos lista los archivos del escritorio:
![[Pasted image 20230417134913.png]]
## DESCARGAR UN ARCHIVO FTP PYTHON
Para descargar un archivo desde el servidor FTP utilizando python lo haremos así:
```python
from ftplib import FTP

ftp = FTP('192.168.0.18')
ftp.login(user='mario', passwd='123123')

ftp.cwd('/home/mario/Escritorio')

# Descargar un archivo
with open('numeros.sh', 'wb') as f:
    ftp.retrbinary('RETR numeros.sh', f.write)

ftp.quit()
```
Y se descarga:
![[Pasted image 20230417135116.png]]
