Hacemos el escaneo de nmap:
![[Pasted image 20231024100218.png]]
Entramos en el puerto 80:
![[Pasted image 20231024100244.png]]
Pero viendo el código fuente vemos un archivo javascript:
![[Pasted image 20231024100305.png]]
El cual tiene el siguiente contenido:
![[Pasted image 20231024100329.png]]
Y si buscamos la ruta del /save-user-agent en el navegador, vemos que algo existe:
![[Pasted image 20231024100455.png]]
Seguimos haciendo reconocimiento realizando fuzzing web, donde vemos un config.php:
![[Pasted image 20231024101008.png]]
No obstante, lo que tenemos que hacer es interceptar la petición del save-user-agent.php y guardarla dentro de un .txt para después usarla con sqlmap, por lo que en firefox interceptamos por un puerto determinado:
![[Pasted image 20231024102615.png]]
Y con netcat lo recibimos:
![[Pasted image 20231024102642.png]]
De tal forma que cambiamos el GET por un POST, ya que en el fichero javascript pudimos ver que se está utilizando un POST, y además debemos añadir el user agent que lo encontraremos en las cabeceras de la url http://192.168.0.25/save-user-agent.php:
![[Pasted image 20231024102804.png]]
Guardamos la respuesta de netcat cambiándola a POST y el user agent en el siguiente formato:
![[Pasted image 20231024102453.png]]
Y ya con esto, lo pasamos con sqlmap:
