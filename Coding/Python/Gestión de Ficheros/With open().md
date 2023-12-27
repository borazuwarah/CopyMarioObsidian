Con esta función podremos abrir ficheros y trabajar sobre ellos; así sería su funcionamiento por ejemplo para leer el contenido de un documento de texto con Python:

```python
with open('documento.txt') as file:

   leer = file.read()
   print(leer)
```

![[Pasted image 20230108162759.png]]

Para escribir dentro de un fichero lo haríamos de esta forma, aunque esto eliminará todo el contenido del documento:

```python
with open('documento.txt', 'w') as file:

    file.write('Escribimos este texto en el documento')
```

![[Pasted image 20230108163037.png]]

Para escribir varias líneas en un documento y que haga un salto a cada línea:
```python
with open('documento.txt', 'w') as file:

    file.write('Escribimos este texto en el documento\n')
    file.write('Hola que tal\n')
    file.write('Otro texto\n')
    file.write('Final\n')
```

![[Pasted image 20230108163320.png]]
**ABRIR UN ARCHIVO EN BINARIO CON PYTHON**
Así es el código para abrir y leer un archivo en binario con python:
```python
# Abre un archivo en modo binario
with open("direcciones_ip.txt", "rb") as f:
    # Lee el contenido del archivo
    contenido = f.read()
    
    # Imprime el contenido del archivo
    print(contenido)
```
Y vemos que lee el contenido:
![[Pasted image 20230421040212.png]]

