Hacemos el reconocimiento con nmap:
![[Pasted image 20230819154133.png]]
![[Pasted image 20230819154219.png]]
![[Pasted image 20230819154231.png]]
Añadimos el dominio windcorp.thm al /etc/hosts:
![[Pasted image 20230819154347.png]]
Miramos lo que corre por el puerto 80:
![[Pasted image 20230819154203.png]]
Si hacemos clic en restablecer contraseña nos encontramos con más virtual hosting:
![[Pasted image 20230819154437.png]]
Por lo que lo añadimos también al /etc/hosts:
![[Pasted image 20230819155221.png]]
Y ahora ya vemos para cambiar la password:
![[Pasted image 20230819154613.png]]
Y aquí vemos que tenemos que responder a alguna de estas preguntas para poder cambiar la contraseña:
![[Pasted image 20230819154705.png]]
Pero casualmente en una de las imágenes hay un perro donde podamos obtener su nombre (Posiblemente sea sparky):
![[Pasted image 20230819154751.png]]
![[Pasted image 20230819154910.png]]
Y hemos cambiado la contraseña de uno del usuario lilyle:
```
lilyle:ChangeMe#1234
```
![[Pasted image 20230819154940.png]]
Vemos que las credenciales son correctas por smb:
![[Pasted image 20230819155456.png]]
