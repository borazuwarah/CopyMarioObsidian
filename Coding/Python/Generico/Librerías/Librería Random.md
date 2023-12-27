Sirve para generar valores aleatorios con Python.
### Generando un número entero aleatorio:

La función `randint` de la librería `random` se utiliza para generar un número entero aleatorio en un rango dado.
```python
import random

# Generando un número entero aleatorio entre 1 y 10
random_number = random.randint(1, 10)

print(random_number)
```

### Generando una lista de números aleatorios:
```python
import random

# Generando una lista de 5 números aleatorios únicos del rango de 1 a 10
random_numbers = random.sample(range(1, 11), 5)

print(random_numbers)
```
### Barajando elementos de una lista (como barajar unas cartas)
Para cambiar el orden de forma aleatoria de números o elementos de una lista en Python, lo hacemos con shuffle:
```python
import random

# Barajando los elementos de una lista
my_list = [1, 2, 3, 4, 5]
random.shuffle(my_list)

print(my_list)
```
![[Pasted image 20230218152403.png]]
## LIBRERÍA RANDOM DE PYTHON CON BASES DE DATOS

Vamos a complementar el funcionamiento de la librería random con las bases de datos, donde vamos a crear un script que introduzca valores aleatorios a mi base de datos utilizando la librería random:

![[Pasted image 20230104152014.png]]
Otro ejemplo donde hacemos lo mismo de antes pero con dos tablas y muchos números, donde insertamos muchos datos a la vez con un bucle for dentro de la base de datos:
```python
import mysql.connector
import random

  

conexion = mysql.connector.connect(user="root", password="123123",

                                    host="localhost",
                                    database="tienda",
                                    port="3306",)


miCursor = conexion.cursor()


for posiciones in range(0,1000):

    ganadores = random.choice(range(0,1000))

    miCursor.execute(f"INSERT INTO cesta(ganador, posicion) VALUES ('{ganadores}', '{posiciones}')")


conexion.commit()

  
conexion.close()
```
![[Pasted image 20230105132949.png]]


