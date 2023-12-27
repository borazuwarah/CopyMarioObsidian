Un conjunto (set) en Python es una estructura de datos que se utiliza para almacenar una colección desordenada de elementos únicos. A diferencia de las listas, los conjuntos no conservan un orden específico ni permiten elementos duplicados.

-----------------
Con la función set() podemos crear un conjunto, donde vemos que ya no hay elementos repetidos:
```python
lista = [1, 2, 3, 3, 4, 4, 5]
conjunto = set(lista)
print(conjunto)
```
Y el resultado:
![[Pasted image 20230530163244.png]]
En resumen, los conjuntos en Python son útiles cuando se necesita almacenar una colección de elementos únicos y realizar operaciones de conjunto eficientes, como verificación de pertenencia, eliminación de duplicados y manipulación matemática de conjuntos. Las listas son más adecuadas cuando se necesita un orden específico y la posibilidad de tener elementos duplicados en la colección.
## INTERSECTION()
En Python, el método `.intersection()` se utiliza para encontrar la intersección, es decir, los elementos comunes, entre dos o más conjuntos (sets):
```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}
set3 = {4, 5, 6, 7}

intersection_set = set1.intersection(set2, set3)
print(intersection_set)
```
Y el resultado sería el 4 porque es el número que se encuentra en común en todos los conjuntos:
![[Pasted image 20230530164000.png]]
Aunque también es posible que nos muestre más números, no solo uno (por ejemplo si hay más de un repetido):
```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 1, 6}
set3 = {4, 5, 1, 7}

intersection_set = set1.intersection(set2, set3)
print(intersection_set)
```
![[Pasted image 20230530164104.png]]


