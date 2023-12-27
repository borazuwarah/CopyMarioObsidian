```python
# Podemos usar un bucle for para recorrer toda una cadena de texto:

for x in "Hola amigos":
    print(x)
```
Ejemplo de bucle for con un correo electrónico; si el correo tiene @ es True, si no lo tiene es False:
Va a haber un momento del bucle donde i va a ser arroba; y es ahí cuando email va a pasar a ser True:
```python
email=False # En un principio el email es False

for i in input():
    if i=="@":
        email=True  # Pero si el email tiene un @, pasará a ser True

if email==True:
    print("El email es correcto") # Si es True (teniendo el @), imprimirá este mensaje.
else:
    print("El email no es correcto") # Si es False, imprimirá este otro.
    
# El bucle for va recorriendo cada uno de los caracteres uno a uno de la lista.
```
Ejemplo de un programa que verifique si una contraseña tiene determinados caracteres o no:
```python
contraseña=False

for i in input("Introduce una contraseña que tenga un arroba, un asterisco y una almohadilla: "):
    if i== "#" and "@" and "*":
        contraseña=True

if contraseña==True:
    print("Contraseña correcta")
else:
    print("Contraseña incorrecta")
```

![[Pasted image 20230104152624.png]]
Ahora por ejemplo voy a hacer un DOBLE BUCLE FOR para recorrer al mismo tiempo dos carpetas y hacer comparaciones, por ejemplo para eliminar los archivos duplicados:
```python
import os

# Guardamos en listas los archivos de estos directorios:
archivosprueba = os.listdir("/Users/mario/Desktop/prueba/")
archivoscarpetadestino = os.listdir("/Users/mario/Desktop/carpetadestino/")

# Guardamos las rutas de las dos carpeta, para luego hacer un os.chdir() sobre la carpeta donde queramos borrar los archivos duplicados.

rutaprueba = ("/Users/mario/Desktop/prueba/")
rutacarpetadestino = ("/Users/mario/Desktop/carpetadestino/")

for i in archivosprueba:
   for a in archivoscarpetadestino:
        if i==a:
           print("Se repiten los archivos: ",a,i)
           os.chdir(rutacarpetadestino)
           os.remove(a) # Se borrarán los archivos duplicados que aparecerán abajo.
```

![[Pasted image 20230104152833.png]]
## Guardar resultado de un bucle for en una variable
Una forma más eficiente de usar un bucle for, donde todos los resultados del bucle en total se guardan en una variable:
```python
import os

archivos = [_ for _ in os.listdir()]
print(archivos)
```
![[Pasted image 20231012153638.png]]
Otro ejemplo de lo anterior:
```python
lista = ['naranja', 'platano', 'sandia']
hola = [_ for _ in lista]
print(hola)
```
![[Pasted image 20230819165722.png]]
## Uso de Guión Bajo _ en Python
El uso de "for _ in" es común en Python cuando no necesitas usar el valor de la variable de iteración en un bucle. En lugar de crear una variable de iteración con un nombre, puedes usar el guión bajo para indicar que no te importa el valor. Aquí tienes algunos ejemplos de cómo se utiliza:
```python
numbers = [1, 2, 3, 4, 5]
for _ in numbers:
    print("Mensaje sin mostrar el contenido de la variable")
```
![[Pasted image 20231012153418.png]]
