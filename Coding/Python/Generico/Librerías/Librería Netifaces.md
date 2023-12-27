La biblioteca `netifaces` en Python proporciona una interfaz para acceder a la información de red del sistema operativo, como direcciones IP, máscaras de subred, puertas de enlace y otras propiedades de las interfaces de red. Aquí tienes algunos ejemplos básicos de uso de `netifaces`:

Obtener una lista de todas las interfaces de red disponibles:

```python
import netifaces

interfaces = netifaces.interfaces()
print(interfaces)
```
Y este sería el resultado:
![[Pasted image 20230617103232.png]]
## Obtener la dirección IP de una interfaz:

```python
import netifaces

interfaces = netifaces.interfaces()

print(netifaces.ifaddresses(interfaces[4])) # Ponemos 4 para acceder a la interfaz tun0 que está en posición 4 dentro de la lista.
```
Y este sería el resultado:
![[Pasted image 20230617104204.png]]
Por lo que teniendo todo esto, ya podemos acceder al valor de IP y guardarlo en una variable:
```python
import netifaces

interfaces = netifaces.interfaces()

tun0 = netifaces.ifaddresses(interfaces[4])

ip_address = tun0[2][0]['addr']
print(ip_address)
```
Y dentro de la variable ip_address ya tenemos guardada la IP:
![[Pasted image 20230617104342.png]]