Un diccionario en Python es un tipo de dato que permite almacenar pares clave-valor. Estos pares clave-valor se pueden acceder, modificar o eliminar de manera eficiente utilizando la clave correspondiente.
```python
# Creación de un diccionario
colors = {'red': 'rojo', 'green': 'verde', 'blue': 'azul'}

# Acceder a un valor utilizando su clave
print(colors['red']) # salida: 'rojo'

# Agregar un nuevo elemento al diccionario
colors['yellow'] = 'amarillo'

# Modificar un valor en el diccionario
colors['red'] = 'Rojo'

# Eliminar un elemento del diccionario
del colors['blue']

# Verificar si una clave está en el diccionario
print('red' in colors) # salida: True
print('blue' in colors) # salida: False

```
De esta forma podemos ejecutar este código y obtener el valor que queramos; por ejemplo vamos a obtener el valor rojo:
![[Pasted image 20230201140835.png]]
Y obtenemos el valor que se encuentra dentro de red:
![[Pasted image 20230201140852.png]]

En este ejemplo, creamos un diccionario llamado `colors` con tres elementos clave-valor. Luego, accedemos a un valor utilizando su clave, agregamos un nuevo elemento, modificamos un valor existente y eliminamos un elemento. Finalmente, verificamos si una clave está en el diccionario utilizando la operación `in`.
![[Pasted image 20230201141002.png]]
## RECORRER ELEMENTOS CLAVE-VALOR DE UN DICCIONARIO
También podemos recorrer todos los elementos del diccionario tanto clave como valor a través de un [[conocimiento/conocimiento/Coding/Python/Generico/Conceptos B├ísicos/Bucle FOR|Bucle FOR]]
```python
diccionario = {"Mario": 27, "Pepe":24, "Luis": 30}

for clave,valor in diccionario.items():
    print(f"{clave} -> {valor}")
```
Y este sería el resultado:
![[Pasted image 20230529130838.png]]
También podemos hacer otro ejemplo:
```python
# Definir un diccionario
frutas = {
    'manzana': 3,
    'banana': 6,
    'naranja': 4,
    'kiwi': 2
}

# Iterar sobre los elementos del diccionario utilizando items()
for fruta, cantidad in frutas.items():
    print(fruta, cantidad)
```
Y obtenemos el mismo resultado:
![[Pasted image 20230529130918.png]]
# CREAR DICCIONARIO RECORRIENDO ELEMENTOS DE UN BUCLE FOR
Podemos crear un diccionario a partir de recorrer un bucle for:
```python
categorias = ["frutas", "verduras", "carnes"]
inventario = {categoria: ["ejemplo1", "ejemplo2"] for categoria in categorias}

print(inventario)
```
Y este es el resultado:
![[Pasted image 20230616131909.png]]
