Con el comando $? podemos comprobar si el comando anterior se ejecutó sin errores o no en Linux, donde si imprimimos la variable ? obtendremos un número que nos indicará si el comando anterior se ejecutó correctamente, la salida 0 es que el comando se ejecutó bien, y la salida 127 es que hubo un error:
![[Pasted image 20230526123556.png]]
Podemos crear también un script que compruebe mediante sentencias condicionales si un comando se ejecutó correctamente o no, por ejemplo que nos compruebe si el comando ls se ejecutó bien:
```bash
#!/bin/bash

# Ejemplo 1: Comprobar el éxito de la ejecución de un comando
ls
if [ $? -eq 0 ]; then
    echo "El comando ls se ejecutó correctamente."
else
    echo "Ocurrió un error al ejecutar el comando ls."
fi
```
Y el resultado es este, donde vemos que se ejecuta el comando ls de forma correcta y si al ejecutar el $? el resultado es un 0, es que el comando se ejecutó bien:
![[Pasted image 20230526123804.png]]
