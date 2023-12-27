Esta biblioteca facilita la manipulación y validación de direcciones IP, así como la realización de operaciones relacionadas con subredes, como la obtención de hosts válidos en una subred, la verificación de pertenencia de una dirección IP a una subred y más:
```python
import ipaddress

# Crear una instancia de dirección IP
ip = ipaddress.ip_address('192.168.0.1')
print(f"La dirección es {ip}")  
print(f"La versión de la IP anterior es: {ip.version}")
```
Y este sería el resultado:
![[Pasted image 20230531111047.png]]
## EXTRAER DATOS DE UNA RED/SUBRED
Podemos extraer más información de una red de esta forma:
```python
import ipaddress

# Crear una instancia de red/subred
red = ipaddress.ip_network('192.168.0.0/24')
print(red)  # 192.168.0.0/24
print(red.network_address)  # 192.168.0.0
print(red.broadcast_address)  # 192.168.0.255
```
Y nos lo extrae:
![[Pasted image 20230531111804.png]]
## VERIFICAR SI UNA IP PERTENECE A UNA SUBRED
Podemos comprobar si una IP pertenece a una subred o no de esta forma:
```python
import ipaddress

red = ipaddress.ip_network('192.168.0.0/24')

# Verificar si una dirección IP pertenece a la subred
direccion = ipaddress.ip_address('192.168.0.100')
print(direccion in red)
```
![[Pasted image 20230531111924.png]]
Si por ejemplo ponemos la IP en otro rango, nos dará false, porque ya no pertenece a la /24:
```python
import ipaddress

red = ipaddress.ip_network('192.168.0.0/24')

# Verificar si una dirección IP pertenece a la subred
direccion = ipaddress.ip_address('192.168.1.100')
print(direccion in red)
```
![[Pasted image 20230531112003.png]]
## OBTENER HOSTS VÁLIDOS DE UNA SUBRED
Por ejemplo, podemos obtener uno a uno cada IP que exista dentro de una subred, por ejemplo de una /24:
```python
import ipaddress

red = ipaddress.ip_network('192.168.0.0/24')

hosts = list(red.hosts())
print(hosts)
```
![[Pasted image 20230531112137.png]]
