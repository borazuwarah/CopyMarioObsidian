**Son fragmentos de código que se pueden ejecutar múltiples veces**
```python
def bienvenida():
    print("Bienvenido a mi canal")
    print("Te animo a suscribirte")
    print("Adiós")
    
bienvenida()
```
![[Pasted image 20230106135904.png]]

```python
# Voy a crear la función saludar, de esta manera, si abajo escribo la función con sus dos paréntesis, se va a ejecutar el comando que tengo dentro de la función:

def saludar():
    print("Hola, este print se llama desde la función saludar()")

saludar()

# Con esto llamo a la función.
```

![[Pasted image 20230106135935.png]]

```python
# Otro ejemplo pero con una suma de dos números / Enviar valores a la función:

def numeros(numero1,numero2):
    print(numero1 + numero2)

numeros(5,2)
numeros(2,1)
numeros(9384,39383)

# Los números 5 y 2 son enviados arriba del todo y se guardan en "numeros"
```

![[Pasted image 20230106140000.png]]

```python
# Creo la función test y dentro quiero que me imprima la variable m:

m = 10
def test():
    print(m)
test()

# Si creamos una variable dentro de la función, solo funcionará dentro de esa función.
```

![[Pasted image 20230106140018.png]]

```python
# Esta función va a repetir 5 veces la suma de i + 1

def sumar():
    for i in range(5): # Aquí digo que se repita este bucle 5 veces
        print("Voy a ir sumando 1 más 1 hasta llegar a 5: ",i,"=",i+1) # Aquí digo que i va a ser a i + 1; como esto se repetirá 5 veces, acabará valiendo 5.

sumar()
```

![[Pasted image 20230106140039.png]]

```python
# Juntando la función con la for:

def dibujar_tabla_del_5():
    for i in range(10):
        print("5*",i,"=",i*5)
dibujar_tabla_del_5()
```

![[Pasted image 20230106140058.png]]

Ahora por último, veremos cómo transferir el contenido de una variable de una función a otra:

```python
def saludar():

    nombre = "Mario"
    ultimo(nombre) # Aquí enviamos el contenido de la variable nombre a la siguiente función.

def ultimo(nombre): # Recogemos el contenido de la variable nombre definida dentro de la función anterior para poder utilizarla en esta otra.

    print("El nombre que pasamos de una función a otra es: ",nombre)
saludar()
```

![[Pasted image 20230106140340.png]]

### IMPRIMIR EL VALOR DE UNA VARIABLE QUE HAYA SIDO DECLARADA DENTRO DE UNA FUNCIÓN
```python
def mi_funcion():
  nombre = 'mario'
  return nombre

resultado = mi_funcion()
print(resultado)
```
Y este es el resultado:
![[Pasted image 20230510212548.png]]
### DISTINTAS VARIABLES A LA VEZ
Si hay más de una variable dentro de la función y queremos imprimirlas fuera, lo haríamos de esta forma:
```python
def mi_funcion():
  nombre = "Mario"
  apellido = "Álvarez"
  return nombre, apellido

variable1, variable2 = mi_funcion()
print(variable1)
print(variable2)
print("El valor de x es", variable1, "y el valor de y es", variable2)
```
Y este sería el resultado:
![[Pasted image 20230510213001.png]]
### VARIABLES LOCALES Y GLOBALES EN FUNCIONES
En Python, cuando defines una variable dentro de una función sin utilizar la palabra clave `global`, esa variable se considera local a la función. Esto significa que la variable solo es válida dentro del ámbito de esa función y no es accesible desde fuera de ella. Sin embargo, si deseas que una variable sea global y pueda ser modificada tanto dentro como fuera de la función, puedes utilizar la palabra clave `global`.

Aquí hay un ejemplo para ilustrar cómo funciona una variable global en una función en Python:
```python
# Variable global
variable_global = 10

def funcion_con_variable_global():
    # Se accede a la variable global dentro de la función
    global variable_global
    variable_local = 5
    variable_global += variable_local
    print("Dentro de la función:", variable_global)

# Llamamos a la función
funcion_con_variable_global()

# La variable global también puede ser accedida y modificada fuera de la función
print("Fuera de la función:", variable_global)
```
![[Pasted image 20231221220635.png]]
