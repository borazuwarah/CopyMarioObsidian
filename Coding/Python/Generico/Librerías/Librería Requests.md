Con la librería requests puedo hacer peticiones HTTP desde python:
```python
import requests

# Realizar una petición GET a una URL
response = requests.get('http://192.168.0.18')

print(response)
```
Y vemos que la conexión fue exitosa:
![[Pasted image 20230417135417.png]]
## DESCARGAR ARCHIVOS CON REQUESTS PYTHON
Para descargar un archivo con requests de un servidor http, podemos hacerlo de esta forma:
```python
import requests

url = 'http://192.168.0.18/numeros.sh'

response = requests.get(url)

with open('numeros.sh', 'wb') as f:
    f.write(response.content)
```
Y si lo ejecutamos, vemos que el archivo se descargó:
![[Pasted image 20230417140017.png]]
