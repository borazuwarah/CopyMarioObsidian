Esta librería sirve para poder crear y gestionar hojas de excel con Python. Así sería su funcionamiento básico:
### PARA LEER UN EXCEL DESDE PYTHON
```python
import openpyxl

# Abrir archivo de Excel
workbook = openpyxl.load_workbook('ejemplo.xlsx')

# Seleccionar la hoja de trabajo por nombre
worksheet = workbook['Hoja1']

# Leer valor de celda A1
valor = worksheet['A1'].value

# Imprimir valor de celda en la consola
print(valor)
```
Por ejemplo, tenemos un documento de excel que dentro de la columna y fila A1 se contiene la palabra hola:
![[Pasted image 20230216222959.png]]
Si ejecutamos este código, al estar indicando de leer la fila y columna A1, vamos a extraer esta palabra:
![[Pasted image 20230216223033.png]]
## PARA INSERTAR DATOS EN UN EXCEL DESDE PYTHON
Para insertar datos en un excel desde Python, lo haremos con el siguiente código:
```python
import openpyxl

# Cargar el archivo de Excel
workbook = openpyxl.load_workbook('ejemplo.xlsx')

# Seleccionar la hoja de trabajo por nombre
worksheet = workbook['Hoja1']

# Escribir datos en una celda
worksheet['A1'] = 'Ejemplo de texto'
worksheet['B1'] = 1234

# Guardar los cambios en el archivo de Excel
workbook.save('ejemplo.xlsx')
```
Y si lo ejecutamos funciona perfecto. Tenemos la información añadida en las columnas A1 y B1:
![[Pasted image 20230216223419.png]]
