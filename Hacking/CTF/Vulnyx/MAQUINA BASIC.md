Hacemos el reconocimiento con nmap:
![[Pasted image 20231102101526.png]]
Y esto corre por el puerto 80:
![[Pasted image 20231102101618.png]]
Y esto por el puerto 631:
![[Pasted image 20231102101641.png]]
Y en la parte de las impresoras encontramos al usuario dimitri:
![[Pasted image 20231102101727.png]]
Por lo que hacemos un ataque de fuerza bruta por el puerto 22 con el usuario dimitri:
```bash
hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://192.168.0.47
```
La encuentra:
![[Pasted image 20231223122258.png]]
Y accedemos vía ssh:
![[Pasted image 20231102102124.png]]
Hacemos una búsqueda de binarios y vemos el de /env:
![[Pasted image 20231102102328.png]]
Por tanto miramos en gtfobins y tenemos unas instrucciones:
![[Pasted image 20231102102352.png]]
Lo ejecutamos y ya somos root:
![[Pasted image 20231102102405.png]]
