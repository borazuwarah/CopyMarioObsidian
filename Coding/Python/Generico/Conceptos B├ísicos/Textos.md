# Los Números
Así es como se realizan operaciones matemáticas sencillas con Python:
```python
# Suma
a = 10
b = 5
resultado = a + b
print(resultado) # Imprime 15

# Resta
a = 10
b = 5
resultado = a - b
print(resultado) # Imprime 5

# Multiplicación
a = 10
b = 5
resultado = a * b
print(resultado) # Imprime 50

# División
a = 10
b = 5
resultado = a / b
print(resultado) # Imprime 2.0

# Potencia
a = 10
b = 2
resultado = a ** b
print(resultado) # Imprime 100

# Módulo (resto de la división)
a = 10
b = 3
resultado = a % b
print(resultado) # Imprime 1

```
# JUNTAR STR + STR, JOIN y SPLIT
Podemos juntar un string con otro en Python de la siguiente forma:
```python
# Utilizando el operador +
nombre = "Juan"
apellido = "Pérez"
nombre_completo = nombre + " " + apellido
print(nombre_completo) # Imprime "Juan Pérez"

# Utilizando la función join()
palabras = ["Hola", "mundo", "!"]
mensaje = " ".join(palabras)
print(mensaje) # Imprime "Hola mundo !"
```
Y obtenemos esta respuesta:
![[Pasted image 20230529150758.png]]
Aunque también podemos hacer que al juntar todos los elementos de la lista, en vez de poner un espacio que ponga cualquier otro símbolo, por ejemplo un guión:
```python
palabras = ["Hola", "mundo", "!"]
mensaje = "-".join(palabras)
print(mensaje) 
```
![[Pasted image 20230529150921.png]]
También podemos incluso filtrar por columnas en un texto de python, donde podríamos convertir unos strings en una lista y luego aplicar un filtro, de tal forma que sea similar a [[COMANDO AWK (Para filtrar por columnas en bash)]] de linux, utilizando el método split():
```python
texto = "Hola Que tal Mario"
lista_palabras = texto.split()

print(lista_palabras)
```
Y ahora cada palabra estará separada:
![[Pasted image 20230529151400.png]]
Y fácilmente podemos filtrar por columnas en python:
```python
texto = "Hola Que tal Mario"
lista_palabras = texto.split()

print(lista_palabras[1])
```
![[Pasted image 20230529151505.png]]
