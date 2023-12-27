En este ejemplo, se define una función llamada "mi_funcion" que simplemente muestra un mensaje por pantalla. Luego, se llama a la función:
```bash
#!/bin/bash

# Definición de la función
mi_funcion() {
    echo "¡Hola desde la función!"
}

# Llamada a la función
mi_funcion
```
En este ejemplo, la función "saludar" recibe un parámetro (en este caso, el nombre de una persona) y muestra un saludo personalizado. Se realiza una llamada a la función con diferentes argumentos:
```bash
#!/bin/bash

# Definición de la función con parámetros
saludar() {
    nombre=$1
    echo "¡Hola, $nombre!"
}

# Llamada a la función con un argumento
saludar "Juan"

# Llamada a la función con otro argumento
saludar "María"
```
En este ejemplo, la función "sumar" recibe dos parámetros, realiza la suma y almacena el resultado en una variable local. Luego, se muestra el resultado por pantalla fuera de la función:
```bash
#!/bin/bash

# Definición de la función con valor de retorno
sumar() {
    local resultado=$(( $1 + $2 ))
    echo $resultado
}

# Llamada a la función y almacenamiento del valor de retorno
resultado_suma=$(sumar 5 3)

echo "El resultado de la suma es: $resultado_suma"
```
# VARIABLES LOCALES Y GLOBALES EN FUNCIONES / EXTRAER CONTENIDO DE UNA FUNCIÓN EN BASH
Podemos declarar variables locales o globales dentro de una función para luego poder utilizarlas fuera de la función. Por lo que hay que diferenciar entre funciones locales y globales:
```bash
#!/bin/bash

my_function() {
  local my_variable="Hola, soy una variable local"
  global_variable="¡Hola, soy una variable global!"
}

# Llamamos a la función para que se ejecute y declare las variables
my_function

# Imprimimos el valor de la variable local (solo dentro de la función, por lo que no se imprimirá)
echo "Valor de la variable local: $my_variable"

# Imprimimos el valor de la variable global (accesible fuera de la función)
echo "Valor de la variable global: $global_variable"
```
Y este es el resultado:
![[Pasted image 20230613214759.png]]
# ENVÍO DE PARÁMETROS EN LAS FUNCIONES
Podemos hacerlo de la siguiente forma, donde tenemos una función con parámetros que reciba la información desde fuera de la función:
```bash
#!/bin/bash

# Definir una función que toma dos parámetros
function saludar {
  nombre=$1
  edad=$2
  echo "Hola, $nombre. Tienes $edad años."
}

# Llamar a la función y pasarle dos parámetros
saludar "Juan" 30
```
Vamos a hacer otro ejemplo donde una función reciba un archivo como parámetro y vea el tamaño que ocupa:
```bash
#!/bin/bash

# Función que verifica el tamaño de un archivo dado como parámetro
verificar_tamaño_archivo() {
    archivo="$1"

    # Verificar si el archivo existe
    if [ -e "$archivo" ]; then
        # Utilizar du para obtener el tamaño del archivo en formato legible
        espacio=$(du -h "$archivo" | awk '{print $1}')
        echo "El tamaño del archivo $archivo es: $espacio"
    else
        echo "El archivo $archivo no existe."
    fi
}

# Llamar a la función y pasar el nombre del archivo como parámetro
verificar_tamaño_archivo "$1"
```
![[Pasted image 20230922133620.png]]