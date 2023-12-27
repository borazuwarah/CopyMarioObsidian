En Bash, puedes usar parámetros para recoger datos al momento de ejecutar un script. Los parámetros se pasan al script como argumentos de línea de comandos y se pueden recoger dentro del script utilizando las variables especiales "$1", "$2", "$3", etc., donde "$1" hace referencia al primer parámetro, $2" al segundo y así sucesivamente. También puedes utilizar $@ para hacer referencia a todos los parámetros pasados:
```bash
#!/bin/bash

# Recoger el primer parámetro en una variable
parametro1="$1"

# Recoger el segundo parámetro en una variable
parametro2="$2"

# Imprimir los valores de los parámetros
echo "El primer parámetro es: $parametro1"
echo "El segundo parámetro es: $parametro2"
```
Otro ejemplo:
```bash
#!/bin/bash

# Recoger los dos números como parámetros
numero1="$1"
numero2="$2"

# Realizar la suma
suma=$((numero1 + numero2))

# Imprimir el resultado
echo "La suma de $numero1 y $numero2 es: $suma"

```