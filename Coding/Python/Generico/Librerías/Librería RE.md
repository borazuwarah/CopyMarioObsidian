Antes de nada hay que entender que las expresiones regulares son casi siempre iguales en todos los lenguajes de programación:
![[Pasted image 20230606140847.png]]

### Expresión regular re.find y re.find.all:
Para utilizar las expresiones regulares, podemos utilizar la librería re, por ejemplo en primer lugar vamos a buscar un dato dentro de un string con re.search:
```python
import re

texto = "Hola que tal"
texto_buscar = "Hola"

if re.search(texto_buscar, texto) is not None:
	print("He encontrado el texto: ", texto_buscar)
else:
	print("No lo encontré")
```
Y vemos que nos lo encuentra correctamente:
![[Pasted image 20230119100925.png]]
Pero esto sólo busca una vez, pero qué ocurre si queremos encontrar todas las veces que se repite un patrón; pues para ello hacemos uso de find.all:
```python
import re

texto = "Hola que tal. Hola a todos"
texto_buscar = "Hola"
lista = re.findall(texto_buscar, texto)

print(lista)
```
Y nos guarda en una lista las veces que se repite una palabra:
![[Pasted image 20230119101437.png]]
O también con .len podemos saber cuantas veces se repite una palabra:
```python
import re

texto = "Hola que tal. Hola a todos"
texto_buscar = "Hola"
print(len(re.findall(texto_buscar, texto)))
```
Donde vemos que en este caso se repite dos veces: 
![[Pasted image 20230119101754.png]]
## PARA ELIMINAR TODO EL TEXTO QUE VAYA DESPUÉS DE UN ESPACIO U OTRO PATRÓN
El patrón específico, r"(\w+)\s._", coincide con una o más palabras (\w+) seguidas de un espacio (\s) y cualquier carácter (_) después de ese espacio. La parte de la expresión regular en paréntesis se utiliza para capturar la primera parte, es decir, las palabras que están antes del espacio.

Un ejemplo de uso de esta expresión regular sería:
```python
import re

texto = "Hola mundo 123"
patron = r"(\w+)\s.*"

resultado = re.sub(patron, r'\1', texto)
print(resultado)
```
Y este sería el resultado:
![[Pasted image 20230201045712.png]]
## RE.COMPILE
La función `re.compile()` se utiliza para compilar una expresión regular en un objeto de patrón, el cual se puede reutilizar para buscar coincidencias en cadenas de texto:
```python
import re

patron = re.compile(r'apple')
texto = 'Me encanta comer una apple roja.'

resultado = patron.search(texto)

if resultado:
    print("Se encontró la palabra 'apple' en el texto.")
else:
    print("No se encontró la palabra 'apple' en el texto.")
```
Y por ejemplo, como la variable patrón se encuentra dentro del texto, imprimirá el print de que encontró la palabra apple:
![[Pasted image 20230530165607.png]]
