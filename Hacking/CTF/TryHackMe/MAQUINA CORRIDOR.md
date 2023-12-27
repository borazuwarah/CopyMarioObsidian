Hacemos el escaneo de nmap y vemos que sólo tenemos abierto el puerto 80:
![[Pasted image 20231024105332.png]]
Si vemos el código fuente de la web, nos encontramos con lo siguiente:
![[Pasted image 20231024105355.png]]
Y esto en el código fuente, donde tenemos varios hashes:
![[Pasted image 20231024105413.png]]
Si miramos en crackstation vemos que cada uno de los hashes tiene un valor numérico:
![[Pasted image 20231024105606.png]]
Por lo que vamos a obtener todos los hashes mediante regex de bash:
```bash
curl http://10.10.97.162/ | tr '"' ' ' | awk '{print $4}'
```
![[Pasted image 20231024105740.png]]
Y obtenemos resultados:
![[Pasted image 20231024105909.png]]
Si miramos cada uno de estos hashes en hash-identifier para ver el tipo de hash, y vemos que se trata de MD5:
![[Pasted image 20231024110917.png]]
