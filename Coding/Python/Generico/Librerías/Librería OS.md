## OBTENER RUTA ABSOLUTA DE UN ARCHIVO
```python
import os

print(os.path.abspath('pruebas.txt'))
```
Y este sería el resultado:
![[Pasted image 20230524150643.png]]
## OBTENER LA EXTENSIÓN DE UN ARCHIVO
Podemos filtrar por un determinado patrón en un archivo, por ejemplo para obtener la extensión de un archivo:
```python
import os

nombre_base = os.path.splitext("video.avi")[1]
print(nombre_base)
```
![[Pasted image 20230928135240.png]]
