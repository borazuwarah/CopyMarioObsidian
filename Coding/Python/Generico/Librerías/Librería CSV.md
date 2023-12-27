Así podemos escribir de forma simple en un CSV:
```python
import csv

# 2. Escritura en un archivo CSV
datos = [
    ['Nombre', 'Apellido', 'Edad'],
    ['John', 'Doe', 25],
    ['Jane', 'Smith', 30]
]

with open('nuevos_datos.csv', 'w', newline='') as archivo:
    escritor = csv.writer(archivo)
    escritor.writerows(datos)
```
Y este es el resultado, donde vemos que las 3 líneas se han escrito por orden:
![[Pasted image 20230531165412.png]]