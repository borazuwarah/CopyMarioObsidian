Por ejemplo tenemos esta data en formato json:
```python
import json

data = {"nombre": "Juan", "edad": 25, "ciudad": "Madrid"}
```
Podemos convertir esta data en un objeto python:
```python
json_data = json.dumps(data)
print(json_data)
```
El resultado sería este:
![[Pasted image 20230513111234.png]]
### ESCRIBIR SOBRE FICHERO JSON
También podemos escribir sobre un fichero json que en un principio está vacío:
![[Pasted image 20230513111525.png]]
Por tanto este sería el código para escribir sobre él:
```python
data = {"nombre": "Juan", "edad": 25, "ciudad": "Madrid"}

# Escribir un archivo JSON
with open("data.json", "w") as f:
    json.dump(data, f)
```
Y si lo ejecutamos habremos escrito el contenido de la variable data dentro de json:
![[Pasted image 20230513111632.png]]
Y por ejemplo podemos leer el contenido de un json de esta forma:
```python
# Leer un archivo JSON
with open("data.json", "r") as f:
    data = json.load(f)

print(data["ciudad"])  # Imprime "Madrid"
```
y leemos el resultado:
![[Pasted image 20230513111736.png]]
## AGRUPAR EN LISTAS AQUELLOS JSON QUE TENGAN UN ELEMENTO EN COMÚN (Por ejemplo la fecha)
```python
data = [
    {"fecha": "07/12/2023", "edad": 25, "ciudad": "Madrid"},
    {"fecha": "01/11/2022", "edad": 25, "ciudad": "Oviedo"},
    {"fecha": "10/11/2023", "edad": 25, "ciudad": "León"},
    {"fecha": "07/12/2023", "edad": 25, "ciudad": "Madrid"},
    {"fecha": "01/11/2022", "edad": 25, "ciudad": "Oviedo"},
    {"fecha": "10/11/2023", "edad": 25, "ciudad": "León"}
]

fechas_dict = {}

for dato in data:
    fecha = dato["fecha"]
    if fecha not in fechas_dict:
        fechas_dict[fecha] = [dato]  # crea una nueva lista para la fecha
    else:
        fechas_dict[fecha].append(dato)  # agrega el dato a la lista existente

fechas_ordenadas = sorted(fechas_dict.items())

for fecha, datos in fechas_ordenadas:
    print(f"Fecha: {fecha}")
    for dato in datos:
        print(dato)
```
Por último, si quisiéramos leer la data de un archivo json, lo haríamos de esta forma:
![[Pasted image 20230513114038.png]]
```python
import json

with open('data.json', 'r') as f:
    data = json.load(f)

fechas_dict = {}

for dato in data:
    fecha = dato["fecha"]
    if fecha not in fechas_dict:
        fechas_dict[fecha] = [dato]  # crea una nueva lista para la fecha
    else:
        fechas_dict[fecha].append(dato)  # agrega el dato a la lista existente

fechas_ordenadas = sorted(fechas_dict.items())

for fecha, datos in fechas_ordenadas:
    print(f"Fecha: {fecha}")
    for dato in datos:
        print(dato)
```
![[Pasted image 20230513114056.png]]
