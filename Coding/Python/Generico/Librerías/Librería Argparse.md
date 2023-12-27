Con argparse podemos establecer parámetros en nuestros scripts de python y recibirlos con instrucciones incluidas; por ejemplo de la siguiente forma:
```python
import argparse

# Crea un objeto ArgumentParser
parser = argparse.ArgumentParser(description='Script de prueba')

# Agrega argumentos
parser.add_argument('-nombre', help='Introduce tu nombre')  # Argumento posicional
parser.add_argument('-edad', type=int, help='Introduce tu edad')  # Argumento opcional

# Analiza los argumentos de la línea de comandos
args = parser.parse_args()

# Accede a los valores de los argumentos
nombre = args.nombre
edad = args.edad

# Realiza alguna operación con los argumentos
saludo = f'Hola {nombre}'

if edad:
    saludo += f', tienes {edad} años'

print(saludo)
```
Y este sería el resultado:
![[Pasted image 20230702210450.png]]
