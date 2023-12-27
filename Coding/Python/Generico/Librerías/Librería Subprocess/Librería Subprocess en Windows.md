Para ejecutar un comando utilizando la biblioteca `subprocess` de Python en Windows, se puede utilizar la siguiente sintaxis:
### EJECUTAR UN COMANDO
```python
import subprocess

# Ejecutar el comando "dir" en la consola de Windows
subprocess.run("dir", shell=True)
```
### EJECUTAR UN COMANDO CON PARÁMETROS
Si queremos ejecutar un comando con parámetros, lo haríamos de la siguiente manera (por ejemplo el comando ipconfig all):
```python
import subprocess

# Ejecutar el comando "ipconfig /all" en la consola de Windows
subprocess.run(["ipconfig", "/all"], shell=True)
```
### CAPTURAR EN UNA VARIABLE EL OUTPUT DE UN COMANDO
También es posible capturar la salida del comando utilizando el parámetro `capture_output`. Por ejemplo:
```python
import subprocess

# Ejecutar el comando "dir" en la consola de Windows y capturar la salida
result = subprocess.run("dir", shell=True, capture_output=True, text=True)

# Obtener la salida del comando
output = result.stdout

# Imprimir la salida del comando
print(output)
```
