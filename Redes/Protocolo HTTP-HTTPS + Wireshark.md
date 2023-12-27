## PROTOCOLO HTTP
Vamos a ver cómo las credenciales de un usuario viajan en forma de texto claro por la web viéndolo con wireshark. Por lo que primero creamos un código python que envíe los datos al servidor:
```python
import http.client

# Definir los datos de inicio de sesión
username = "mario"
password = "123123"

# Definir la dirección IP y el puerto del servidor
ip = "192.168.0.19"
port = 443

# Definir los encabezados HTTP
headers = {'Content-type': 'application/x-www-form-urlencoded'}

# Definir los datos que se enviarán al servidor
data = 'username=' + username + '&password=' + password

# Crear una conexión HTTP con el servidor
conn = http.client.HTTPConnection(ip, port)

# Enviar los datos de inicio de sesión a través de HTTP
conn.request("POST", "/login", body=data, headers=headers)

# Obtener la respuesta del servidor
response = conn.getresponse()

# Verificar si la solicitud se ha completado correctamente
if response.status == 200:
    print("Inicio de sesión exitoso!")
else:
    print("Error en el inicio de sesión:", response.reason)

# Cerrar la conexión HTTP
conn.close()
```

Nos ponemos en escucha con netcat desde el servidor para recibir la petición:
![[Pasted image 20230501204549.png]]
Y ahora con wireshark pondremos que esté filtrando todo el tráfico de la IP 192.168.0.19; y vemos que los datos enviados utilizando HTTP han sido interceptados:
![[Pasted image 20230501204426.png]]
## PROTOCOLO HTTPS
Ahora vamos a ver esto mismo pero con HTTPS, donde veremos que la misma información irá cifrada, por lo que primero usaremos este código de python para enviar la petición:
```python
import http.client

# Definir los datos de inicio de sesión
username = "mario"
password = "123123"

# Definir la dirección IP y el puerto del servidor
ip = "192.168.0.19"
port = 443

# Definir los encabezados HTTP
headers = {'Content-type': 'application/x-www-form-urlencoded'}

# Definir los datos que se enviarán al servidor
data = 'username=' + username + '&password=' + password

# Crear una conexión HTTPS con el servidor
conn = http.client.HTTPSConnection(ip, port)

# Enviar los datos de inicio de sesión a través de HTTPS
conn.request("POST", "/login", body=data, headers=headers)

# Obtener la respuesta del servidor
response = conn.getresponse()

# Verificar si la solicitud se ha completado correctamente
if response.status == 200:
    print("Inicio de sesión exitoso!")
else:
    print("Error en el inicio de sesión:", response.reason)

# Cerrar la conexión HTTPS
conn.close()
```
Nos ponemos en escucha con netcat y vemos que recibimos la información cifrada:
![[Pasted image 20230501204916.png]]
Y con wireshark vemos que tampoco podemos obtener las credenciales en texto plano:
![[Pasted image 20230501204952.png]]
