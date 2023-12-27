La librería pickle de Python es una herramienta muy útil para la serialización y deserialización de objetos Python. La serialización se refiere al proceso de convertir un objeto Python en un flujo de bytes, que se puede guardar en un archivo o transmitir a través de una red (como haremos con sockets más adelante), mientras que la deserialización es el proceso inverso, que consiste en reconstruir el objeto Python a partir de los datos serializados.

```python
import pickle

# Serialización
data = {"nombre": "Juan", "edad": 30}
serialized_data = pickle.dumps(data)
print(f"Serialized data: {serialized_data}")

# Deserialización
deserialized_data = pickle.loads(serialized_data)
print(f"Deserialized data: {deserialized_data}")
```

## SOCKETS + PICKLE

Es posible enviar datos a través de un socket utilizando la librería pickle de Python. La serialización y deserialización de los datos se realiza automáticamente mediante pickle, lo que simplifica mucho el proceso de enviar y recibir objetos complejos a través de un socket. 

#### EL QUE ENVÍA EL MENSAJE:

```python
import socket
import pickle

client_socket = socket.socket()
client_socket.connect(('192.168.0.19', 5000))

data = {"nombre": "Juan", "edad": 30}
mensaje_serializado = pickle.dumps(data)
client_socket.send(mensaje_serializado)

client_socket.close()
```

#### EL QUE RECIBE EL MENSAJE

```python
import socket
import pickle

server_socket = socket.socket()
server_socket.bind(('192.168.0.19', 5000))
server_socket.listen(1)

client_socket, client_address = server_socket.accept()
message = client_socket.recv(1024)

mensaje_desserializado = pickle.loads(message)
print(f"Received: {mensaje_desserializado}")

client_socket.close()
server_socket.close()
```
Y vemos que se envió correctamente el mensaje:
![[Pasted image 20230429212105.png]]
