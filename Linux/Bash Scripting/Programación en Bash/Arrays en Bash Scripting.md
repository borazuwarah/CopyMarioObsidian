Es como si fuera una lista en python, pero su similar en bash.

Un array en Bash es una colección ordenada de elementos, donde cada elemento puede ser accedido mediante un índice:

```bash
# Declaración de un array
frutas=("manzana" "naranja" "plátano" "uva")
# Accediendo a elementos del array  
echo "La primera fruta es ${frutas[0]}"  
echo "La tercera fruta es ${frutas[2]}"
```
![[Pasted image 20231122202453.png]]
O podemos hacer todo el proceso completo:
```bash
#!/bin/bash

# Declaración de un array
frutas=("manzana" "naranja" "plátano" "uva")

# Accediendo a elementos del array
echo "La primera fruta es ${frutas[0]}"
echo "La tercera fruta es ${frutas[2]}"

# Modificando un elemento del array
frutas[1]="limón"
echo "La segunda fruta es ${frutas[1]}"

# Añadiendo elementos al array
frutas+=("mango" "papaya")
echo "Las frutas disponibles son: ${frutas[@]}"

# Recorriendo el array con un bucle for
echo "Recorriendo el array con un bucle for:"
for fruta in "${frutas[@]}"; do
    echo "- $fruta"
done
```
Y este sería el resultado:
![[Pasted image 20230414120544.png]]
Otro ejemplo de array sería el siguiente, donde guardamos en una lista todos los archivos del actual directorio, siendo el arroba un elemento para referirnos a todos los archivos en total:
```bash
#!/bin/bash

archivos=($(ls))

for archivo in "${archivos[@]}"; do
    echo "$archivo"
done
```
Y este sería el resultado:
![[Pasted image 20231122202714.png]]

## AÑADIR ELEMENTOS A UNA LISTA A PARTIR DE UN BUCLE FOR
También podemos añadir elementos a una lista vacía a partir de un bucle for de la siguiente forma:
```bash
#!/bin/bash

cosas=("hola" "que tal" 5)
lista=("${cosas[@]}")

for i in "${lista[@]}"
do
  echo "$i"
done
```
Y este es el resultado:
![[Pasted image 20230613220036.png]]

