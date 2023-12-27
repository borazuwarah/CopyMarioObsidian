Haremos los reconocimientos con nmap y encontramos estos puertos abiertos:
![[Pasted image 20230327070945.png]]
Si entramos a la web por el puerto 80, nos encontramos con que tenemos esta dirección, por tanto lo añadimos dentro del /etc/hosts:
![[Pasted image 20230327071102.png]]
![[Pasted image 20230327071152.png]]
Y ahora ya nos carga la web:
![[Pasted image 20230327071212.png]]
Si hacemos fuzzing nos encontramos con estos directorios, donde nos encontramos con el código 302, lo cual significa redirección:
![[Pasted image 20230327071650.png]]
Si entramos en el directorio web de admin, vemos que nos aplica un redirect a login:
![[Pasted image 20230327071830.png]]
![[Pasted image 20230327071842.png]]
Y en este caso si probamos en poner una comilla o algo para probar si se acontecede una SQL injection, vemos que se queda bloqueada, por lo podemos intuir que hay algo de esto:
![[Pasted image 20230327073650.png]]
Por tanto vamos a buscar payloads válidos para esta inyección SQL dentro del repositorio de payloadsallthethings:
![[Pasted image 20230327073817.png]]
Y tenemos un apartado de NoSQL Injection:
![[Pasted image 20230327073917.png]]
Y nos muestran estas instrucciones, donde la data puede ser tramitada por json o no:
![[Pasted image 20230327074555.png]]
Por tanto vamos a enviar una petición enviando la data por JSON, donde tenemos que especificar JSON en el content type; y vemos que a priori no funciona:
![[Pasted image 20230327075949.png]]
Pero lo interesante es que si en el valor de username o password le colamos alguna comilla, estamos forzando la ejecución de un comportamiento diferente:
![[Pasted image 20230327080033.png]]
E incluso podemos ver un usuario:
![[Pasted image 20230327080056.png]]
Y tras varios intentos, encontramos esta inyección como válida, sin necesidad de tramitar la petición por JSON, en las instrucciones que encontramos por internet:
![[Pasted image 20230327082344.png]]
![[Pasted image 20230327082402.png]]
Y probando esta inyección con el usuario admin ya estamos dentro:
![[Pasted image 20230327081944.png]]
![[Pasted image 20230327082441.png]]
Dentro de esta web, tenemos un buscador de usuarios, donde si ponemos un usuario válido (como el usuario admin), vemos que nos genera un reporte:
![[Pasted image 20230327083249.png]]
Y este es el reporte que nos genera:
![[Pasted image 20230327083310.png]]
No obstante, dentro de este panel también podemos probar la misma noSQL injection de antes y vemos que también funciona:
![[Pasted image 20230327083432.png]]
E incluso accedemos a otro usuario llamado josh:
![[Pasted image 20230327083455.png]]
Vamos a ver de que tipo de hash se trata utilizando la herramienta hash-identifier, donde vemos que se trata de md5:
![[Pasted image 20230327083650.png]]
Y ahora en este punto podríamos usar hashcat u otra herramienta, pero también podemos hacer el crackeo de forma online en una web que se llama crackstation, donde vemos que funciona correctamente, donde la contraseña es remembermethisway:
![[Pasted image 20230327083955.png]]
Y ahora en este punto podemos quedarnos estancados, por tanto tenemos que seguir con la enumeración; y esta vez para encontrar subdominios en la página principal de shoppy.htb, por lo que probamos en hacer una búsqueda con gobuster pero no encontramos nada:
![[Pasted image 20230327085440.png]]
Por tanto, tras probar varios diccionarios, nos encontramos con que con el diccionadio de bitquark funciona, el cual podemos encontrar en este repositorio:
```javascript
https://github.com/danielmiessler/SecLists/blob/master/Discovery/DNS/bitquark-subdomains-top100000.txt
```
Por tanto nos lo descargamos y probamos este:
![[Pasted image 20230327090544.png]]
Probamos en hacer la búsqueda de subdominios con gobuster:



