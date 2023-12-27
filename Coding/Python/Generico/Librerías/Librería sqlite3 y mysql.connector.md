**Se va a crear un fichero en el mismo directorio que será la base de datos**
![[Pasted image 20230104141021.png]]
![[Pasted image 20230104141033.png]]
**Cómo ver el contenido de la base de datos que estamos creando**
Debemos descargar DB Browser for SQLITE y arrastrar en su interior la base de datos que creamos.
![[Pasted image 20230104141102.png]]
![[Pasted image 20230104141114.png]]
![[Pasted image 20230104141125.png]]
![[Pasted image 20230104141136.png]]
![[Pasted image 20230104141212.png]]
![[Pasted image 20230104141222.png]]
![[Pasted image 20230104141233.png]]
![[Pasted image 20230104141242.png]]
**CREANDO OTRA TABLA QUE TENGA PRIMARY KEY**
![[Pasted image 20230104141256.png]]
![[Pasted image 20230104141306.png]]
![[Pasted image 20230104141316.png]]
![[Pasted image 20230104141325.png]]
# CONECTAR Y USAR PYTHON CON MYSQL
pip install mysql-connector-python
![[Pasted image 20230104145252.png]]
![[Pasted image 20230104145303.png]]
![[Pasted image 20230104145317.png]]
![[Pasted image 20230104145333.png]]
![[Pasted image 20230104145344.png]]
![[Pasted image 20230104145356.png]]
![[Pasted image 20230104145410.png]]
También podemos automatizar la insercción de muchos datos a la vez utilizando un bucle for, por ejemplo vamos a crear un secuenciador que vaya insertando el mismo ordenador muchas veces pero cambiando el código de cada ordenador desde el 51 al 100:
![[Pasted image 20230104151211.png]]
Y ahora dentro de MySQL tenemos estos nuevos registros creados:
![[Pasted image 20230104151240.png]]
Ahora vamos a hacer esto mismo pero de una forma más compleja e insertando muchos datos dentro de la base de datos desde Python:
```python
import mysql.connector
import time

while(True):

    numero = int(input("Introduce un número: "))
    if numero <= 1000:
        print("Genial, número introducido entre 0 y 1000, está correcto")
        break
    else:
        print("Número incorrecto, vuelve a intentarlo")

conexion = mysql.connector.connect(user="root", password="123123",

                                    host="localhost",
                                    database="tienda",
                                    port="3306",)

  
miCursor = conexion.cursor()
miCursor.execute(f"SELECT ganador FROM cesta WHERE posicion={numero}")
consulta = miCursor.fetchall()
conversion_lista = []

  

for i in consulta: # Sacamos los datos de la tupla y lo pasamos a una lista.
    for a in i:
        conversion_lista.append(a)


time.sleep(3)


numero_ganador = conversion_lista[0] # Sacamos el dato de la lista para que no tenga el corchete.

print(f"El número ganador es: {numero_ganador}")
  
conexion.close()
```

![[Pasted image 20230105140106.png]]
Incluso también podemos utilizar random para insertar una serie de datos de manera automática en la base de datos, lo haríamos con este código:
```python
import mysql.connector
import random

conexion = mysql.connector.connect(user="root", password="123123",

                                    host="localhost",
                                    database="tienda",
                                    port="3306",)

miCursor = conexion.cursor()

posibles_modelos = ["ASUS", "HP", "LENOVO"]
posibles_colores = ["amarillo", "azul", "rojo"]


for numero in range(0,100):

    modelos = random.choice(posibles_modelos)
    colores = random.choice(posibles_colores)
    miCursor.execute(f"INSERT INTO ordenadores(id_ordenador, modelo, color) VALUES ('{numero}', '{modelos}', '{colores}')")

  

conexion.commit()

conexion.close()
```

![[Pasted image 20230106151728.png]]


