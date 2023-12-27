Hacemos el escaneo con nmap:
![[Pasted image 20230910110754.png]]
Y esta es la web:
![[Pasted image 20231103153039.png]]
Y en la parte de abajo de esta web vemos que hay unas credenciales por defecto dentro del directorio /fuel:
![[Pasted image 20231103153356.png]]
Y tenemos un panel de login, donde probamos con admin:admin:
![[Pasted image 20231103153459.png]]
Donde tenemos el siguiente entorno:
![[Pasted image 20231103153532.png]]
Hacemos fuzzing web y encontramos el directorio assets:

