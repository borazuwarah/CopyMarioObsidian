## PROCESO DE CREACIÓN DEL CTF
Lo primero será descargar un Ubuntu Server:
![[Pasted image 20231116115623.png]]
Y esto lo instalamos en VirtualBox, teniendo en cuenta que la contraseña de la máquina para acceder vía ssh va a ser la siguiente:
```
communitypenguin:pinguino123
```
![[Pasted image 20231116120255.png]]
Ahora vamos a crear otro usuario que será el que se va a utilizar para hacer la intrusión a la máquina, por lo que usamos el comando useradd para crearlo. Se va a llamar capybarauser y la contraseña será cabybaramolon:
```
capybarauser:ie168
```
![[Pasted image 20231116123307.png]]
Ahora vamos a comprobar que podemos iniciar sesión como este usuario por ssh:
![[Pasted image 20231116123424.png]]
Ahora desde el usuario communitypenguin hacemos la instalación de la base de datos mysql-server:
```
su communitypenguin
apt update
apt install mysql-server
```
Y ahora hacemos que la base de datos siempre se inicie al ejecutar el sistema, además de comprobar que el servicio se encuentre en ejecución:
```
sudo su
systemctl enable mysql
systemctl status mysql
```
![[Pasted image 20231116123814.png]]
A continuación tenemos que hacer visible la base de datos por el puerto 3306 para que nmap pueda encontrarla, por lo que ponemos como bind-address en 0.0.0.0 la siguiente línea del siguiente archivo de configuración:
```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
![[Pasted image 20231116124219.png]]
Y ahora reiniciamos el servicio de la base de datos y además aplicamos una regla para que se permita el tráfico por el puerto 3306 hacia la base de datos, además de comprobar que el firewall se encuentre disponible y funcionando correctamente:
```
sudo systemctl restart mysql
sudo ufw allow 3306
sudo ufw enable
```
![[Pasted image 20231116124408.png]]
Ahora tenemos que habilitar el acceso al usuario capybarauser y con su contraseña para poder acceder a la base de datos, por lo que accedemos a la base de datos:
```
mysql
```
![[Pasted image 20231116124704.png]]
Y ejecutamos los siguientes comandos, primero para registrar el usuario capybarauser dentro de la base de datos, y luego para poder establecer el permiso de acceso para dicho usuario:
```
CREATE USER 'capybarauser'@'localhost' IDENTIFIED BY 'ie168';
GRANT ALL PRIVILEGES ON *.* TO 'capybarauser'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
![[Pasted image 20231116125329.png]]
Y ahora lo mismo para que podamos entrar desde otros equipos:
```
CREATE USER 'capybarauser'@'%' IDENTIFIED BY 'ie168';
GRANT ALL PRIVILEGES ON *.* TO 'capybarauser'@'%' WITH GRANT OPTION;
```
![[Pasted image 20231116132519.png]]
Y ahora reiniciamos el servicio y probamos su correcto funcionamiento:
```
mysql -u capybarauser -pie168
```
![[Pasted image 20231116125513.png]]
Y lo mismo desde otra máquina:
![[Pasted image 20231116135412.png]]
Para finalizar con esta parte, tenemos que permitir que no haya límite máximo de intentos de inicio de sesión editando el fichero conf.d y añadiendo la siguiente configuración:
![[Pasted image 20231116152056.png]]
Y lo mismo dentro del mysqld:
![[Pasted image 20231116154146.png]]
Y ahora vemos que tenemos el firewall limitando los escaneos de nmap debido a la regla que hemos creado:
![[Pasted image 20231116125730.png]]
Por lo que habilitamos también el acceso al puerto 22:
```
sudo ufw allow 22
```
![[Pasted image 20231116125840.png]]
![[Pasted image 20231116125913.png]]
Y ahora vamos a insertar información dentro de la base de datos para que se pueda encontrar el usuario mario con la contraseña pinguinomolon123:
```
mysql
CREATE DATABASE IF NOT EXISTS pinguinasio_db;
USE pinguinasio_db;

CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL
);

INSERT INTO users (user, password) VALUES ('mario', 'pinguinomolon123');

SELECT * FROM users;
```
![[Pasted image 20231116130649.png]]
Una vez hecho esto, es momento de crear dicho usuario:
```
adduser mario
```
![[Pasted image 20231116131019.png]]
Ahora lo que debemos hacer es configurar el sistema para que sólo el usuario mario pueda entrar vía ssh, de tal forma que para entrar a la base de datos haya que ser el usuario capybarauser y para entrar al sistema el usuario mario:
```
sudo nano /etc/ssh/sshd_config
```
![[Pasted image 20231116131313.png]]
Y vemos que ahora no podemos entrar como el usuario capybarauser:
![[Pasted image 20231116131432.png]]
Pero sí con el usuario mario:
```
mario:pinguinomolon123
```
![[Pasted image 20231116131515.png]]
EL siguiente paso será instalar un servidor web de apache para que el usuario pueda detectar el nombre de capybarauser y hacer fuerza bruta contra la base de datos:
```
apt install apache2
systemctl enable apache2
```
![[Pasted image 20231116133118.png]]
Y ahora tenemos que añadir la regla en el firewall para que permita ver desde fuera el puerto 80:
![[Pasted image 20231116133226.png]]
Y vemos que ahora el firewall sí permite la conexión:
![[Pasted image 20231116133318.png]]
Ahora vamos a crear un html que contenga la pista del nombre de usuario para acceder a la base de datos y la imagen de un capybara:
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web de Capybaras</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
            color: #333;
            margin: 0;
            padding: 0;
            text-align: center;
        }

        header {
            background-color: #4CAF50;
            padding: 20px;
            color: white;
            font-size: 24px;
        }

        main {
            padding: 20px;
        }

        footer {
            background-color: #4CAF50;
            padding: 10px;
            position: fixed;
            bottom: 0;
            width: 100%;
        }

        footer img {
            max-width: 100%;
            height: auto;
        }
    </style>
</head>
<body>

    <header>
        <h1>Bienvenido a la Web de Capybaras</h1>
    </header>

    <main>
        <p>Hola <strong>capybarauser</strong>, esta es una web de capybaras.</p>
    </main>

    <footer>
        <img src="capybara.jpg" alt="Capybara">
    </footer>

</body>
</html>
```
![[Pasted image 20231116133943.png]]
Lo enviamos a la máquina community penguin dentro del directorio html:
![[Pasted image 20231116134110.png]]
Reiniciamos el servicio de apache y la web es accesible:
![[Pasted image 20231116134544.png]]
Ahora para escalada de privilegios, vamos a hacer que el binario nmap tenga el bit de setuid, por lo que primero instalamos nmap y luego otorgamos el permiso:
```
apt install nmap
sudo chmod u+s $(which nmap)
```
![[Pasted image 20231116135512.png]]
Y ahora probamos la escalada de privilegios, donde vemos correctamente el binario de nmap:
![[Pasted image 20231116135848.png]]
Y también podemos contemplar que se pueda escalar privilegios usando el comando sudo -l con nano, por lo que ejecutamos el comando sudo visudo:
```
sudo visudo
```
Y añadimos la siguiente línea:
![[Pasted image 20231116141459.png]]
Y vemos que funciona bien:
![[Pasted image 20231116141553.png]]
Y de esta forma presionando control + r y control + x podemos inyectar el siguiente payload:
![[Pasted image 20231116141642.png]]
![[Pasted image 20231116141711.png]]
# RESOLUCIÓN
Hacemos el escaneo de nmap:
![[Pasted image 20231116155137.png]]
Esto es lo que corre por el puerto 80:
![[Pasted image 20231116155322.png]]
Vemos que tiene abierto el puerto 3306 donde corre una base de datos, y posiblemente el usuario sea capybarauser, por lo que hacemos un ataque de fuerza bruta con metasploit, pero su contraseña se encuentra al final del rockyou, por lo que tendremos que invertirle el orden:
![[Pasted image 20231116155453.png]]
```
tac /usr/share/wordlists/rockyou.txt > test.txt
```
Pero vemos que no lo ordenó bien:
![[Pasted image 20231116160321.png]]
Y ahora aplicamos la siguiente regex:
![[Pasted image 20231116160521.png]]
Por lo que ahora vemos que está casi corregido, donde solo quitamos los símbolos de las primeras líneas y listo:
![[Pasted image 20231116160606.png]]
Y ahora encuentra bien la contraseña:
![[Pasted image 20231116160648.png]]
Y ahora usaremos el módulo de mysql_login:
![[Pasted image 20231116155620.png]]
![[Pasted image 20231116154331.png]]
## PROCESO DE SECURIZACIÓN
Lo primero será impedir que se pueda ejecutar nano como root, por lo que lo eliminamos de visudo:
![[Pasted image 20231129092641.png]]
Y lo borramos:
![[Pasted image 20231129092659.png]]
Y ahora si entramos como mario a la máquina, ya no podemos ejecutar sudo -l:
![[Pasted image 20231129093149.png]]
También desactivamos el binario de nmap al ejecutar el comando find / -perm -4000 2>/dev/null:
```bash
sudo chmod u-s $(which nmap)
```
![[Pasted image 20231129095301.png]]
Y ahora vamos a cambiar la contraseña del usuario mario por una segura:
```
mario:dfsdfg?''2""-.,,.:_jfyHG78_
```
![[Pasted image 20231129093555.png]]
Vemos que vía ssh ya no podemos acceder con la antigua contraseña:
![[Pasted image 20231129095531.png]]
Pero sí con la nueva:
![[Pasted image 20231129095549.png]]
A continuación, cambiamos también la contraseña del usuario communitypenguin, ya que ahora es pinguino123 y no es segura:
```bash
communitypenguin:dfsd97834rn_:,,d,d'¡?¿¿iokfns
```

Y ahora vamos a actualizar la base de datos con la nueva contraseña:
![[Pasted image 20231129093648.png]]
Pero pasamos a hash la nueva contraseña:
```bash
mario:f26308b5ca39147c5480de881d400e21
```
![[Pasted image 20231129093755.png]]
Y cambiamos la el contenido de la columna password:
```bash
UPDATE users
SET password = 'f26308b5ca39147c5480de881d400e21'
WHERE user = 'mario';
```
![[Pasted image 20231129093957.png]]
Y comprobamos que hayamos cambiado el dato:
![[Pasted image 20231129094048.png]]
Ahora vamos a cambiar la contraseña de acceso para el usuario capybarauser, de tal forma que ahora la contraseña será la siguiente:
```bash
capyubarauser:dfiwer98g7f__.2'¡?¿dfgfJHH
```
Lo hacemos con los siguientes comandos:
![[Pasted image 20231129094933.png]]
```bash
ALTER USER 'capybarauser'@'localhost' IDENTIFIED BY 'dfiwer98g7f__.2\'¡?¿dfgfJHH';

ALTER USER 'capybarauser'@'%' IDENTIFIED BY 'dfiwer98g7f__.2\'¡?¿dfgfJHH';

FLUSH PRIVILEGES;
```
También actualizamos la contraseña de este usuario para el sistema en general:
![[Pasted image 20231129100330.png]]
Y ahora vemos como ya no podemos acceder a la base de datos con la contraseña anterior:
![[Pasted image 20231129095004.png]]
Pero sí con la nueva:
![[Pasted image 20231129095046.png]]
Ahora vamos a quitar el usuario dentro de la web en el puerto 80, por lo que editamos el fichero index.html y ponemos usuario sin más:
![[Pasted image 20231129095411.png]]


