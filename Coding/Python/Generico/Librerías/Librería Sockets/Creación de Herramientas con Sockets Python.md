# ESCANER DE PUERTOS CON SOCKET

Un escaner de los puertos internos de mi máquina:

```python
import socket

ip = '192.168.0.3'
puertos_lista = [21,22,80,443]
  

for puerto in puertos_lista:

    sock = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    resultado = sock.connect_ex((ip, puerto))

    print(puerto, ":", resultado)
    sock.close()
```
