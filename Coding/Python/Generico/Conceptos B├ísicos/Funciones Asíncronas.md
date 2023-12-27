Funciona de esta forma:
```python
import asyncio

async def tarea1():
    print("Tarea 1 comenzando...")
    await asyncio.sleep(1) # Espera 1 segundo
    print("Tarea 1 terminada!")

async def tarea2():
    print("Tarea 2 comenzando...")
    await asyncio.sleep(2) # Espera 2 segundos
    print("Tarea 2 terminada!")


# Llamamos ambas tareas de forma asíncrona
tarea_1 = asyncio.create_task(tarea1())
tarea_2 = asyncio.create_task(tarea2())
```
Si lo ejecutamos ocurre lo siguiente:
![[Pasted image 20230529131100.png]]
