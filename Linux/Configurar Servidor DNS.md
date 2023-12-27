El primer paso será configurar una IP estática añadiendo la siguiente configuración al fichero de interfaces:
![[Pasted image 20231023155831.png]]
Y ya tenemos correctamente configurada nuestra IP estática:
![[Pasted image 20231023155922.png]]
Y procedemos a cambiar el nombre de nuestro equipo:
![[Pasted image 20231023155944.png]]
Añadimos las zonas que crearemos más adelante dentro del named.conf.default-zones:
```bash
# Nuevas zonas

zone "alvarez.local" {
        type master;
        file "/etc/bind/db.alvarez.local";
};

zone "0.168.192.in-addr.arpa" {
        type master;
        file "/etc/bind/db.0.168.192";
};
```
![[Pasted image 20231023160220.png]]
Ahora el siguiente paso será abrir el archivo named.conf.options y añadir las siguientes líneas:
```bash
options {
	directory "/var/cache/bind";
	forwarders {
		1.1.1.1;
		9.9.9.9;
	};
	dnssec-validation auto;
	auth-nxdomain no;
	listen-on { any; };
	listen-on-v6 { any; };
	allow-query{
		192.168.0.0/24;
		localhost;
	};
};
```
![[Pasted image 20231023160326.png]]
Ahora abrimos el archivo named.conf.options y añadimos las siguientes líneas:
```bash
options {
	directory "/var/cache/bind";
	forwarders {
		1.1.1.1;
		9.9.9.9;
	};
	dnssec-validation auto;
	auth-nxdomain no;
	listen-on { any; };
	listen-on-v6 { any; };
	allow-query{
		192.168.0.0/24;
		localhost;
	};
};
```
En la zona de búsqueda directa, añade un registro A llamado servidor con la IP 192.168.0.100, dos registros de tipo A: pc1 y pc2 que tendrán las direcciones IP 192.168.0.30 para el pc1 y la 192.168.0.31 para el pc2, además de un registro de tipo CNAME ‘ftp’ apuntando a servidor, otro registro de tipo NS apuntando al equipo servidor, y un intercambiador de correo MX con prioridad 10 apuntando a servidor. Por lo que editamos el archivo db.alvarez.local:
```bash
; Zona para el dominio alvarez.local

$TTL 1D
@       IN      SOA     alvarez.local. root.alvarez.local. (
                        1         ; número de serie
                        604800    ; tiempo de refresco (1 semana)
                        86400     ; tiempo de reintento (1 día)
                        2419200   ; tiempo de expiración (4 semanas)
                        86400 )   ; tiempo de vida en caché (1 día)

@       IN      NS      servidor.alvarez.local.
@       IN      MX      10 servidor.alvarez.local.
servidor IN      A       192.168.0.100
pc1      IN      A       192.168.0.30
pc2      IN      A       192.168.0.31
dns      IN      CNAME   servidor.alvarez.local.
www      IN      CNAME   servidor.alvarez.local.
mail     IN      CNAME   servidor.alvarez.local.
```
![[Pasted image 20231023163114.png]]
Y ahora abrimos el fichero db.0.168.192 que hemos creado anteriormente y le añadimos lo siguiente:
```bash
; Zona inversa para la red 192.168.0.0/24

$TTL 86400
@       IN      SOA     alvarez.local. root.alvarez.local. (
                        1         ; número de serie
                        604800    ; tiempo de refresco (1 semana)
                        86400     ; tiempo de reintento (1 día)
                        2419200   ; tiempo de expiración (4 semanas)
                        86400 )   ; tiempo de vida en caché (1 día)

@       IN      NS      servidor.alvarez.local.
100     IN      PTR     servidor.alvarez.local.
30      IN      PTR     pc1.alvarez.local.
31      IN      PTR     pc2.alvarez.local.
```
![[Pasted image 20231023163251.png]]
Por último, si ejecutamos el comando named-checkconf, vemos que no nos muestra ningún error:
```bash
named-checkconf
named-checkzone alvarez.local db.alvarez.local
named-checkzone 0.168.192.in-addr.arpa db.0.168.192
```
![[Pasted image 20231023161629.png]]
Reiniciamos el servicio y comprobamos que se esté ejecutando:
![[Pasted image 20231023161659.png]]
Una vez hecho todo lo anterior, verificamos con el comando nmcli dev show | grep DNS los servidores DNS existentes, donde podemos ver el que acabamos de crear:
![[Pasted image 20231023161804.png]]
Por último, editamos el fichero resolv.conf para añadir nuestro servidor DNS:
![[Pasted image 20231023164611.png]]
Vamos a verificar el correcto funcionamiento con nslookup de la siguiente forma:
![[Pasted image 20231023163850.png]]
![[Pasted image 20231023164648.png]]
![[Pasted image 20231023165016.png]]
