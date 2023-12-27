Hacemos el escaneo de nmap:
![[Pasted image 20231216145058.png]]
Y vemos que esto es lo que corre sobre el puerto 80:
![[Pasted image 20231216145144.png]]
Y si hacemos fuzzing a esto vemos que nos encuentra el directorio /docs, donde nos pone algo de que no tenemos acceso, así como el directorio /api que nos muestra como la versión de la API:
![[Pasted image 20231216145626.png]]
![[Pasted image 20231216145606.png]]
![[Pasted image 20231216145556.png]]
En este punto, es fácil pensar que si ponemos /v1 en la dirección, nos va a acceder a un endpoint:
![[Pasted image 20231216145720.png]]
Y al poner admin, vemos que existe pero que no tenemos acceso:
![[Pasted image 20231216145753.png]]
Y probamos en hacer fuzzing de la dirección de /user y nos encuentra muchos usuarios con valores numéricos, pero nos fijamos en el tamaño de las respuestas, donde para el usuario 01 vemos algo raro:
```bash
dirb http://10.10.11.161/api/v1/user/
```
![[Pasted image 20231216150137.png]]
Lo pegamos en el navegador y vemos algo raro:
![[Pasted image 20231216150209.png]]
En este punto, también podemos hacer peticiones dentro de esta url pero usando POST, por lo que podemos hacerlo con wfuzz de la siguiente forma:
```bash
wfuzz -c -X POST -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt http://10.10.11.161/api/v1/user/FUZZ
```
Aunque con esto nos saca muchas respuestas:
![[Pasted image 20231216150724.png]]
Quitamos las respuestas 405 y nos encuentra estos dos directorios:
```bash
wfuzz -c -X POST --hc=405 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt http://10.10.11.161/api/v1/user/FUZZ
```
![[Pasted image 20231216150816.png]]
Este es el directorio /login:
![[Pasted image 20231216151015.png]]
Y este el /signup:
![[Pasted image 20231216151028.png]]
Una vez descubiertos estos directorios, podemos intentar autenticarnos con curl de la siguiente forma:
```bash
curl -X POST http://10.10.11.161/api/v1/user/login -d 'username=admin&password=admin'
```
![[Pasted image 20231216151310.png]]
Y si intentamos registrarnos con el directorio signup, nos encontramos con un error .dict:
![[Pasted image 20231216151430.png]]
Por lo que lmandamos la data en formato json pero nos sigue saliendo otro error, donde falta el campo email:
```bash
curl -X POST http://10.10.11.161/api/v1/user/signup -H "Content-Type: application/json" -d '{"username": "pinguino", "password": "pinguino"}'
```
![[Pasted image 20231216151739.png]]
Por lo que vamos a hacerlo igual mandando un correo:
```bash
curl -X POST http://10.10.11.161/api/v1/user/signup -H "Content-Type: application/json" -d '{"username": "pinguino", "password": "pinguino", "email": "pinguino@example.com"}'
```
Y ahora sí funciona:
![[Pasted image 20231216151803.png]]
Y al enviar otra vez esta petición vemos que ya hemos registrado el usuario en el sistema:
![[Pasted image 20231216151901.png]]
Y ahora si probamos en iniciar sesión con estas credenciales, nos encontramos con un token de acceso de sesión:
```bash
curl -X POST http://10.10.11.161/api/v1/user/login -d 'username=pinguino@example.com&password=pinguino'
```
![[Pasted image 20231216152236.png]]
Ahora que tenemos forma de autenticarnos, podemos intentar acceder al directorio /docs, que como estamos viendo, tenemos acceso a él:
![[Pasted image 20231217185613.png]]
![[Pasted image 20231217185641.png]]
Por lo que accedemos a este directorio de forma autenticada, pero vemos que tampoco nos muestra nada:
```bash
curl -X POST http://10.10.11.161/docs/ -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNzAzNTQxOTE2LCJpYXQiOjE3MDI4NTA3MTYsInN1YiI6IjIiLCJpc19zdXBlcnVzZXIiOmZhbHNlLCJndWlkIjoiNTczYmUwOTItM2YwZS00MGM4LThkNTMtOWI2OGU0YjRkNDNlIn0.ClkgxdsukuTsyrFkX-UzZh43v_JBij4PzWMbCrVaWwI"
```
![[Pasted image 20231217185846.png]]
Y si usamos el parámetro -I podemos ver las cabeceras de esta respuesta, donde podemos ver que se está aplicando un redirect:
![[Pasted image 20231217185916.png]]
Viendo esto, vamos a interceptar la petición con burp suite:
![[Pasted image 20231217190025.png]]
![[Pasted image 20231217190036.png]]
Y debemos de ir a esta parte para hacer que en el redirect se esté aplicando en todo momento el web token que tenemos y así poder acceder al directorio /docs/:
![[Pasted image 20231217190200.png]]
Y aquí pegamos lo mismo que habíamos enviado antes con curl:
```bash
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0eXBlIjoiYWNjZXNzX3Rva2VuIiwiZXhwIjoxNzAzNTQxOTE2LCJpYXQiOjE3MDI4NTA3MTYsInN1YiI6IjIiLCJpc19zdXBlcnVzZXIiOmZhbHNlLCJndWlkIjoiNTczYmUwOTItM2YwZS00MGM4LThkNTMtOWI2OGU0YjRkNDNlIn0.ClkgxdsukuTsyrFkX-UzZh43v_JBij4PzWMbCrVaWwI
```
![[Pasted image 20231217190609.png]]
![[Pasted image 20231217190618.png]]
Y ahora, al recargar, vemos que burpsuite añade la cabecera de autenticación:
![[Pasted image 20231217190739.png]]
Y accedemos:
![[Pasted image 20231217190757.png]]
Y hay un apartado donde vemos que nos están dando la flag de user:
![[Pasted image 20231217190940.png]]
Vemos que si tramitamos esto por terminal, nos devuelve también la flag:
![[Pasted image 20231217191058.png]]
Si seguimos enumerando esta API, veremos que no tenemos permisos para hacer gran cosa, pero hay un apartado de updatepass, donde nos piden el guid del usuario que queramos cambiarle la contraseña, por ejemplo de admin:
![[Pasted image 20231217191420.png]]
Y en esta parte podemos detectar el guid del usuario admin al pasarle su id, que ya sabemos que es el 1:
![[Pasted image 20231217191538.png]]
Y se la cambiamos:
![[Pasted image 20231217191705.png]]
Vemos que funciona correctamente:
![[Pasted image 20231217191942.png]]
Y ahora en la parte de arriba a la derecha tenemos un panel de autenticación, que podemos probar a entrar como el usuario admin:
![[Pasted image 20231217191826.png]]
![[Pasted image 20231217191908.png]]
Y estamos dentro:
![[Pasted image 20231217191919.png]]
Y ahora sí podemos listar archivos internos de la máquina:
![[Pasted image 20231217192204.png]]
![[Pasted image 20231217192212.png]]
Y esto mismo podemos listarlo desde consola también, ya que la web nos muestra la petición:
![[Pasted image 20231217192337.png]]
![[Pasted image 20231217192320.png]]
Y también podemos visualizar la flag, que está dentro del usuario htb que vemos que ya existe dentro de la máquina:
![[Pasted image 20231217192816.png]]
