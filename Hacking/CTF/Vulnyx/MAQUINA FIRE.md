Hacemos el reconocimento con nmap:
![[Pasted image 20231002124609.png]]
Podemos iniciar sesión por FTP como anonymous, y nos encontramos con un archivo llamado backup.zip:
![[Pasted image 20231002124707.png]]
Y esto es lo que hay dentro (carpeta llamada firefox y con más archivos dentro):
![[Pasted image 20231002124758.png]]
Utilizamos el comando tree . y vemos un archivo llamado logins.json:
![[Pasted image 20231002125120.png]]
Y esto es lo que tenemos dentro de este archivo logins.json:
![[Pasted image 20231002125156.png]]
Pasamos todos estos archivos al directorio de firefox de mi máquina atacante:
```bash
mv archivos.zip /home/kali/.mozilla/firefox/
```
![[Pasted image 20231002130814.png]]
Borramos todos estos archivos y descomprimimos la carpeta:
