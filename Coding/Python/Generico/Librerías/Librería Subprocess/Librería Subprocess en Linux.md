La librería `subprocess` de Python permite interactuar con procesos del sistema operativo, ejecutar comandos en la línea de comandos y obtener su salida. Aquí te dejo algunos ejemplos básicos de cómo usar esta librería:
### Ejecutar un comando en la línea de comandos y obtener su salida:
```python
import subprocess

# Ejecutar el comando "ls" y obtener su salida
output = subprocess.check_output(['ls'])
print(output)
```
### Ejecutar un comando con argumentos:
```python
import subprocess

# Ejecutar el comando "ls -l" y obtener su salida
output = subprocess.check_output(['ls', '-l'])
print(output)
```
### Ejecutar un comando en segundo plano:
```python
import subprocess

# Ejecutar el comando "sleep 10" en segundo plano
subprocess.Popen(['sleep', '10'])
```
