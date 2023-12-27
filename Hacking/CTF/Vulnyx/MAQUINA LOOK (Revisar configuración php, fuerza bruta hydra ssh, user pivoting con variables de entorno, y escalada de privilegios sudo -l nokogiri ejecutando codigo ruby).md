Hacemos el reconocimiento con nmap:
![[Pasted image 20230720154118.png]]
Y nos encontramos con una plantilla de apache:
![[Pasted image 20230720154147.png]]
Y hacemos fuzzing, donde si lo hacemos en búsqueda de directorios no vemos nada interesante, pero si buscamos archivos, vemos dos archivos con extensión .php:
![[Pasted image 20230720154255.png]]
Y dentro del archivo /info.php vemos un usuario llamado axel:
![[Pasted image 20230720154559.png]]
Por tanto, vamos a probar en hacer un ataque de fuerza bruta con hydra vía ssh usando este usuario:
```bash
hydra -t 64 -l axel -P rockyou.txt ssh://192.168.0.39 -F -I
```
![[Pasted image 20230720160034.png]]
Y ya estamos dentro:
![[Pasted image 20230720160131.png]]
Y nos encontramos con el usuario dylan también:
![[Pasted image 20230722165532.png]]
Podemos escalar privilegios revisando las variables de entorno del sistema, ya que es posible que por aquí se guarden contraseñas, por lo que nos encontramos con la contraseña del usuario dylan:
![[Pasted image 20230722165747.png]]
```bash
dylan:bl4bl4Dyl4N
```
Y nos hemos convertido en Dylan:
![[Pasted image 20230722165823.png]]
Y si ejecutamos el comando sudo -l podemos ver que como el usuario dylan podemos ejecutar algo que se llama nokogiri como root:
![[Pasted image 20230722165913.png]]
Si lo visualizamos, vemos que se trata de un archivo escrito en ruby:
![[Pasted image 20230722170006.png]]
Si investigamos, vemos que este script sirve para hacer web scrapping:
![[Pasted image 20230722170628.png]]
Y aquí vemos las opciones, donde vemos además que tiene un parámetro para ejecutar comandos:
![[Pasted image 20230722170105.png]]
Por tanto vamos a intentar entrar en el modo interactivo, ya que si investigamos en esta web, vemos que existe un modo interactivo al pasarle un documento html:
```bash
https://robm.me.uk/2014/01/nokogiri-command-line/
```
![[Pasted image 20230722170847.png]]
Por tanto, en la máquina Kali nos creamos un archivo html y accedemos al modo interactivo:
![[Pasted image 20230722170910.png]]
Ahora, en este punto, si revisamos las paginas man de nokogiri, vemos que también permite ejecutar comandos de ruby:
```bash
man nokogiri
```
![[Pasted image 20230722171508.png]]
Por tanto, si miramos en gtfobins como escalar privilegios con ruby, nos encontramos el siguiente comando:
![[Pasted image 20230722171538.png]]
Por tanto, ejecutamos el comando y deberíamos ser root:
![[Pasted image 20230722171614.png]]


