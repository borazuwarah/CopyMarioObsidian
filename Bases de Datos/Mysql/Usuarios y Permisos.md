Para crear un usuario lo hago de esta manera:
![[Pasted image 20221217102443.png]]
Para darle permisos para listar las bases de datos lo hacemos de esta forma:
![[Pasted image 20221217102451.png]]
Para eliminar todos los permisos lo hacemos de esta forma:
![[Pasted image 20221217102458.png]]
Ahora para conectarnos como este usuario, vamos a la parte de arriba donde pone database:
![[Pasted image 20221217102506.png]]
Y aquí ponemos el nombre del usuario que hemos creado antes:
![[Pasted image 20221217102513.png]]
Ahora nos pide la contraseña, por tanto en mi caso para este usuario es 123123:
![[Pasted image 20221217102519.png]]
Y ahora vemos que este usuario no tiene permisos y ni siquiera puede visualizar la base de datos hr:
![[Pasted image 20221217102527.png]]
Si queremos otorgar por ejemplo el permiso para que pueda ver la tabla employees de la base de datos hr, lo hacemos de esta forma:
![[Pasted image 20221217102535.png]]
Ahora vamos a comprobar si desde este usuario podemos realizar la consulta de la tabla employees; y

vemos que es la única tabla sobre la que tenemos permisos:
![[Pasted image 20221217102542.png]]
Si hago la consulta, funciona correctamente:
![[Pasted image 20221217102549.png]]
Si eliminamos el privilegio que acabamos de otorgar, lo podemos hacer de la siguiente manera:
![[Pasted image 20221217102559.png]]
Y ahora, el usuario ya no tiene estos permisos:
![[Pasted image 20221217102607.png]]
