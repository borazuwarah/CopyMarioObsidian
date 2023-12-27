Esta librería de Python permite poder ejecutar una mista tarea de forma simultánea, lo que nos permitirá agilizar el funcionamiento de nuestros script:
```python
import threading

# Función que será ejecutada por cada hilo
def mi_funcion():
    for i in range(5):
        print(f"Hilo actual: {threading.current_thread().name}, Valor: {i}")

# Crear dos hilos
thread1 = threading.Thread(target=mi_funcion)
thread2 = threading.Thread(target=mi_funcion)

# Iniciar los hilos
thread1.start()
thread2.start()

# Esperar a que los hilos terminen
thread1.join()
thread2.join()

print("Hilos finalizados.")
```
Y este sería el resultado, donde podremos ver como la función se fue ejecutando de forma simultánea:
![[Pasted image 20231012152743.png]]
También podemos verlo con otro ejemplo donde se imprimirán mensajes por pantalla, de tal forma que dichos mensajes al momento de imprimirse se mostrarán mezclados unos con otros:
```python
import threading
import time

# Función que imprimirá un mensaje simple
def imprimir_mensaje(mensaje):
    for _ in range(3):
        print(mensaje)
        time.sleep(0.1)  # Agregar un pequeño retraso para mostrar la mezcla

# Crear dos hilos que ejecutarán la función imprimir_mensaje
thread1 = threading.Thread(target=imprimir_mensaje, args=("Hola desde Hilo 1",))
thread2 = threading.Thread(target=imprimir_mensaje, args=("Hola desde Hilo 2",))

# Iniciar los hilos
thread1.start()
thread2.start()

# Esperar a que los hilos terminen
thread1.join()
thread2.join()

print("Ejecución de hilos completada.")
```
![[Pasted image 20231012153144.png]]
