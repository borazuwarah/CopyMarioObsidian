También podemos crear un diccionario en bash con su clave-valor, de la siguiente forma:
```bash
#!/bin/bash

# Declarar un diccionario
declare -A diccionario

# Agregar elementos al diccionario
diccionario["clave1"]="valor1"
diccionario["clave2"]="valor2"
diccionario["clave3"]="valor3"

# Acceder a un valor usando la clave
echo "El valor de clave2 es: ${diccionario["clave2"]}"
```
Y ahora si imprimimos esto, obtendremos el siguiente mensaje por pantalla:
![[Pasted image 20230528212611.png]]
