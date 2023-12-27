Los sockets en Python permiten la comunicación entre procesos en una red utilizando el protocolo TCP/IP o UDP. Con los sockets, se pueden crear programas que envían y reciben datos en tiempo real a través de una red.

## SERVIDOR - El que Recibe y Ataca

Lado del servidor donde permanecerá esperando la conexión y el mensaje del cliente. Pondremos la IP del equipo que va a recibir el archivo o el mensaje, tanto en el cliente como en el servidor:
```python
import socket

server_socket = socket.socket()
server_socket.bind(('192.168.0.17', 5000))
server_socket.listen(1)

client_socket, client_address = server_socket.accept()
data = client_socket.recv(1024)
print(f"Received: {data.decode()}")

client_socket.close()
server_socket.close()
```

Ahora ejecutamos este código en el servidor y permanecerá en la escucha:
![[Pasted image 20230307130821.png]]

## CLIENTE - El que Envía

Lado del cliente:

```python
import socket

client_socket = socket.socket()
client_socket.connect(('192.168.0.17', 5000))

message = "Hola Servidor! Mensaje enviado desde el cliente"
client_socket.send(message.encode())

client_socket.close()
```
Ejecutamos este código:
![[Pasted image 20230307130839.png]]
Y el mensaje se habrá enviado al servidor:
![[Pasted image 20230307130904.png]]

---------------------------------------

# ENVIAR ARCHIVOS CON SOCKETS DE PYTHON
Vamos a enviar un archivo por ejemplo con extensión .php, desde el cliente al servidor que estará en la máquina Kali Linux:
## CLIENTE

```python
import socket

HOST = '192.168.0.17'  # dirección IP del servidor
PORT = 5000        # número de puerto del servidor
FILE = 'pwned.php' # archivo que se desea enviar

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))
    with open(FILE, 'rb') as f:
        while True:
            data = f.read(1024)
            if not data:
                break
            s.sendall(data)
    print('Archivo enviado')
```
Vamos a explicar ciertos puntos del código:
- **socket.AF_INET:** indica que se va a utilizar el protocolo de red IPv4.
- **socket.SOCK_STREAM:** indica que se va a utilizar el protocolo de transporte TCP.
- **with open(FILE, 'rb') as f:** Esto abre un archivo en modo lectura binaria y lo asocia con un objeto de archivo en Python llamado "f".
- **f.read(1024):** Se utiliza para leer los datos del archivo asociado al objeto de archivo "f" en bloques de 1024 bytes.

## SERVIDOR
Y este será el código del servidor desde Kali Linux que estará esperando el archivo:
```python
import socket

HOST = '192.168.0.17'  # dirección IP del servidor
PORT = 5000        # número de puerto del servidor

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.bind((HOST, PORT))
    s.listen()
    conn, addr = s.accept()
    with conn:
        print('Conectado por', addr)
        with open('pwned.php', 'wb') as f:
            while True:
                data = conn.recv(1024)
                if not data:
                    break
                f.write(data)
        print('Archivo recibido')
```
Y si ejecutamos este código, recibimos el archivo:
![[Pasted image 20230307132326.png]]
![[Pasted image 20230307132356.png]]
