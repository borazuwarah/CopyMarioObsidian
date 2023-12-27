Estos son los puertos abiertos y toda la información:
![[Pasted image 20230418211225.png]]
Y este sería el whatweb:
![[Pasted image 20230418211234.png]]
Y esta es la página web:
![[Pasted image 20230418211240.png]]
Una vez aquí vamos a probar en fuzzear, y nos encontramos con esto:
![[Pasted image 20230418211249.png]]
Vamos a probar con el directorio de dev y nos encontramos con esto:
![[Pasted image 20230418211256.png]]
Y si probamos con encode nos encontramos con un lugar donde podemos subir archivos (y lo mismo funciona con decode)
![[Pasted image 20230418211304.png]]
Y en el fichero hype_key tenemos toda esta información representada en hexadecimal:
![[Pasted image 20230418211314.png]]
Podemos descifrar esta información; y para ello podemos hacerle un curl y hacemos una tubería para descifrar la información, de esta manera, y nos dará una contraseña protegida:
![[Pasted image 20230418211321.png]]
Por tanto ahora debemos pasar esta información a un fichero para después pasarla a contraseña hasheada a través de un script de python llamado ssh2john.py, por tanto pasamos la información a un fichero llamado id_rsa, la cual será la clave privada de la máquina por ssh:
![[Pasted image 20230418211329.png]]
Y ahora ejecutamos este script sobre el fichero que contiene esta información:
![[Pasted image 20230418211337.png]]
Y nos sale la contraseña en formato hash, que podremos lanzarle ataques de fuerza bruta más tarde para descifrarla:
![[Pasted image 20230418211349.png]]
Ahora lo guardamos como hash:
![[Pasted image 20230418211357.png]]
Y ya podremos lanzarle un ataque de fuerza bruta con un diccionario llamado rockyou.txt, aunque vemos que no nos encuentra nada:
![[Pasted image 20230418211404.png]]
Entonces ahora vamos a lanzar un script de nmap para detectar vulnerabilidades sobre el puerto 443 de la máquina con el script interno de nmap de vuln and safe:
![[Pasted image 20230418211411.png]]
Y vemos que tiene varias vulnerabilidades:
![[Pasted image 20230418211424.png]]
Y tenemos esta vulnerabilidad de heartbleed que nos da una pista en relación a la imagen del principio que aparecía en la web [[Vulnerabilidad Heartbleed]]:
![[Pasted image 20230418211431.png]]
Entonces ahora vamos a buscar algún script de python para explotar esta vulnerabilidad, que va a consistir en sacar información privada de la máquina, la cual igual encontramos una contraseña para utilizarla con ssh:
![[Pasted image 20230418211439.png]]
Entramos en este repositorio y lo clonamos:
![[Pasted image 20230418211447.png]]
Y si ejecutamos el exploit de heartbleed (que en este caso debe ser con python 2) nos dará unas instrucciones:
![[Pasted image 20230418211456.png]]
Lo ejecutamos y nos dice que recibió mucha información de la respuesta de la máquina; y nos dice que toda la información nos la exporta en un out.txt:
![[Pasted image 20230418211508.png]]
Si abrimos ese archivo nos encontramos con esto:
![[Pasted image 20230418211515.png]]
Entonces vamos a filtrar con grep para que todas las líneas de 0 que no nos salgan con un grep -v:
![[Pasted image 20230418211522.png]]
Y ahora estas filas ya no nos salen:
![[Pasted image 20230418211530.png]]
Y vemos que no nos aparece nada, pero si volvemos a ejecutar otra vez el exploit, veremos que ahora sí nos da más información:
![[Pasted image 20230418211538.png]]
![[Pasted image 20230418211542.png]]
Y aquí está la parte interesada porque es un dato en base64, ya que acaba con unas líneas y por eso está en base64:
![[Pasted image 20230418211554.png]]
Y ahora si decodificamos en base64 esta contraseña la veremos:
![[Pasted image 20230418211601.png]]
Aho‐ ra para entrar con ssh sabemos la contraseña pero no los posibles usuarios, por tanto vamos a ir probando con cada una de las palabra que vemos ahí, y vemos que hype es un usuario válido; por tanto vamos a probar en conectarnos a la máquina con el id_rsa y la contraseña que acabamos de obtener, pero vemos que nos da un error:
![[Pasted image 20230418211609.png]]
Por tanto, si nos da un error de esta manera, tendremos que conectarnos de esta otra forma:
![[Pasted image 20230418211619.png]]
Y con esta forma ya habremos obtenido acceso a la máquina:
![[Pasted image 20230418211627.png]]
Y ya tenemos la flag:
![[Pasted image 20230418211636.png]]