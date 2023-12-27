
##  (Recursos compartidos SMBMAP, monturas con showmount, vulnerabilidad proFTPd para copiar el id_rsa a la montura en máquina loca, escalada privilegios manipular path)
Hacemos el reconocimiento con nmap:
![[Pasted image 20230617091312.png]]
Y vemos que dentro del puerto 80 tenemos un robos.txt, además de utilizar nfs, por lo que probablemente haya monturas. NFS (Network File System) está relacionado con las monturas en sistemas operativos Unix y Linux.

NFS es un protocolo de sistema de archivos distribuido que permite compartir y acceder a sistemas de archivos en red de manera transparente. Permite que un sistema operativo monte un sistema de archivos remoto como si fuera un sistema de archivos local:
![[Pasted image 20230617091413.png]]
Si inspeccionamos los recursos compartidos, podemos ver lo siguiente con smbmap:
![[Pasted image 20230617091731.png]]
Con el parámetro -r accedemos al recurso de anonymous:
![[Pasted image 20230617091847.png]]
No obstante, si queremos acceder a la cli y poder descargar el archivo, podemos hacerlo con smbclient:
![[Pasted image 20230617092311.png]]
Si hacemos un cat de este archivo vemos que se trata de un id_rsa junto a mucha más información:
![[Pasted image 20230617092529.png]]
En este punto, vamos a inspeccionar el puerto 21 FTP, donde vemos que tiene un servicio que quizá sea vulnerable:
![[Pasted image 20230617093308.png]]
Buscamos en google y encontramos exploits:
![[Pasted image 20230617093417.png]]
En este exploit nos dicen que tenemos que tener previamente subido algún archivo malicioso en la máquina víctima para después poder ejecutarlo con el exploit:
![[Pasted image 20230617174545.png]]
Podemos usar el comando showmount para inspeccionar monturas:
![[Pasted image 20230617174447.png]]
Aunque también podríamos comprobar esto mismo con nmap de la siguiente forma:
![[Pasted image 20230617174749.png]]
Entonces vamos a montarnos todo lo del directorio /var de la máquina víctima a nuestro equipo:
![[Pasted image 20230617175407.png]]
![[Pasted image 20230617175416.png]]
Si vamos al directorio /tmp vemos que no tenemos la clave id_rsa:
![[Pasted image 20230617175651.png]]
Pero ahora podemos ver que gracias a que esta versión de proFTP es vulnerable, podemos copiar y pegar archivos dentro de la máquina, de tal forma que podemos obtener el id_rsa y copiarlo al directorio /var/tmp/id_rsa:
![[Pasted image 20230617180817.png]]
Por tanto ahora tenemos que crear una montura dentro del directorio /var/tmp/id_rsa, donde habíamos copiado anteriormente el archivo id_rsa y también donde teníamos permisos de montura, y ahora todo esto lo montamos en nuestra máquina local y podemos acceder al id_rsa:
![[Pasted image 20230617181229.png]]
```bash
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA4PeD0e0522UEj7xlrLmN68R6iSG3HMK/aTI812CTtzM9gnXs
qpweZL+GJBB59bSG3RTPtirC3M9YNTDsuTvxw9Y/+NuUGJIq5laQZS5e2RaqI1nv
U7fXEQlJrrlWfCy9VDTlgB/KRxKerqc42aU+/BrSyYqImpN6AgoNm/s/753DEPJt
dwsr45KFJOhtaIPA4EoZAq8pKovdSFteeUHikosUQzgqvSCv1RH8ZYBTwslxSorW
y3fXs5GwjitvRnQEVTO/GZomGV8UhjrT3TKbPhiwOy5YA484Lp3ES0uxKJEnKdSt
otHFT4i1hXq6T0CvYoaEpL7zCq7udl7KcZ0zfwIDAQABAoIBAEDl5nc28kviVnCI
ruQnG1P6eEb7HPIFFGbqgTa4u6RL+eCa2E1XgEUcIzxgLG6/R3CbwlgQ+entPssJ
dCDztAkE06uc3JpCAHI2Yq1ttRr3ONm95hbGoBpgDYuEF/j2hx+1qsdNZHMgYfqM
bxAKZaMgsdJGTqYZCUdxUv++eXFMDTTw/h2SCAuPE2Nb1f1537w/UQbB5HwZfVry
tRHknh1hfcjh4ZD5x5Bta/THjjsZo1kb/UuX41TKDFE/6+Eq+G9AvWNC2LJ6My36
YfeRs89A1Pc2XD08LoglPxzR7Hox36VOGD+95STWsBViMlk2lJ5IzU9XVIt3EnCl
bUI7DNECgYEA8ZymxvRV7yvDHHLjw5Vj/puVIQnKtadmE9H9UtfGV8gI/NddE66e
t8uIhiydcxE/u8DZd+mPt1RMU9GeUT5WxZ8MpO0UPVPIRiSBHnyu+0tolZSLqVul
rwT/nMDCJGQNaSOb2kq+Y3DJBHhlOeTsxAi2YEwrK9hPFQ5btlQichMCgYEA7l0c
dd1mwrjZ51lWWXvQzOH0PZH/diqXiTgwD6F1sUYPAc4qZ79blloeIhrVIj+isvtq
mgG2GD0TWueNnddGafwIp3USIxZOcw+e5hHmxy0KHpqstbPZc99IUQ5UBQHZYCvl
SR+ANdNuWpRTD6gWeVqNVni9wXjKhiKM17p3RmUCgYEAp6dwAvZg+wl+5irC6WCs
dmw3WymUQ+DY8D/ybJ3Vv+vKcMhwicvNzvOo1JH433PEqd/0B0VGuIwCOtdl6DI9
u/vVpkvsk3Gjsyh5gFI8iZuWAtWE5Av4OC5bwMXw8ZeLxr0y1JKw8ge9NSDl/Pph
YNY61y+DdXUvywifkzFmhYkCgYB6TeZbh9XBVg3gyhMnaQNzDQFAUlhM7n/Alcb7
TjJQWo06tOlHQIWi+Ox7PV9c6l/2DFDfYr9nYnc67pLYiWwE16AtJEHBJSHtofc7
P7Y1PqPxnhW+SeDqtoepp3tu8kryMLO+OF6Vv73g1jhkUS/u5oqc8ukSi4MHHlU8
H94xjQKBgExhzreYXCjK9FswXhUU9avijJkoAsSbIybRzq1YnX0gSewY/SB2xPjF
S40wzYviRHr/h0TOOzXzX8VMAQx5XnhZ5C/WMhb0cMErK8z+jvDavEpkMUlR+dWf
Py/CLlDCU4e+49XBAPKEmY4DuN+J2Em/tCz7dzfCNS/mpsSEn0jo
-----END RSA PRIVATE KEY-----
```
Y ahora tan solo tenemos que copiar el contenido de este id_rsa, otorgarle permisos chmod 600 y acceder a la máquina de esta forma:
![[Pasted image 20230617181447.png]]
Una vez dentro vamos a enumerar binarios SUID; y nos encontramos con uno extraño que es el de /usr/bin/menu:
![[Pasted image 20230617181936.png]]
Si lo ejecutamos vemos que se trata de una especie de menú que interactúa con nosotros y nos proporciona información del sistema utilizando el comando curl:
![[Pasted image 20230617182142.png]]
Por tanto, podemos modificar la variable de entorno en curl y añadirle que ejecute el comando /bin/bash cada vez que la máquina ejecute el comando curl, por tanto insertamos el comando /bin/bash dentro del archivo curl, y lo añadimos como variable de entorno, para que el menú lo ejecute y nos devuelva esa bash como root:
![[Pasted image 20230617182500.png]]
De esta forma, como vemos, lo que hemos enviado se ejecutará como variable de entorno cada vez que la máquina ejecute algún comando:
![[Pasted image 20230617184345.png]]
De hecho con el comando ifconfig también podríamos haber escalado privilegios de la misma forma, ya que la opción 3 ejecuta el ifconfig:
![[Pasted image 20230617191739.png]]