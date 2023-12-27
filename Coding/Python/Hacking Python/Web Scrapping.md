pip install lxml // pip install bs4 // html5lib

![[Pasted image 20221217090221.png]]

![[Pasted image 20221217090235.png]]

![[Pasted image 20221217090241.png]]

![[Pasted image 20221217090246.png]]

![[Pasted image 20221217090257.png]]
![[Pasted image 20221217090314.png]]

## SCRIPT PARA EXTRAER EQUIPOS DE LA CLASIFICACIÓN DE FUTBOL
Vamos a crear un bot que extraiga todos los equipos de esta web:
![[Pasted image 20230117213029.png]]
Por tanto en Python lo haríamos de esta forma:
```import requests
from bs4 import BeautifulSoup


website = "https://resultados.as.com/resultados/futbol/primera/clasificacion/"
result = requests.get(website)
content = result.text

soup = BeautifulSoup(content, "html5lib")
print(soup.prettify())


equipos = soup.find_all('span', class_= "nombre-equipo") # Aquí filtramos solo los párrafos.

print(equipos)
```

Y este sería el resultado:
![[Pasted image 20230117213830.png]]
## WEB SCRAPING WEB DE VULNHUB
Vamos a crear un script que haga scraping a la web de vulnhub y me saque sólamente los nombres de cada máquina:
```
import requests
import re

  
website = "https://www.vulnhub.com/"
result = requests.get(website)
content = result.text


patron = r"/entry/[\w-]+"
maquinas = re.findall(patron, str(content))

  

for i in maquinas:
	nombre_articulo = i.replace("/entry/", "")
	print(nombre_articulo)
	
```
![[Pasted image 20230201040813.png]]

No obstante, si queremos eliminar elementos repetidos dentro de una lista de python utilizaremos 
```
import requests
import re

  

website = "https://www.vulnhub.com/"
result = requests.get(website)
content = result.text
  

patron = r"/entry/[\w-]+"
maquinas_repetidas = re.findall(patron, str(content))
sin_duplicados = list(set(maquinas_repetidas)) # Quitamos máquinas repetidas.

  
for i in sin_duplicados:
	nombre_maquinas = i.replace("/entry/", "")
	print(nombre_maquinas)
```
Y ahora ya no tendremos máquinas repetidas:
![[Pasted image 20230202124431.png]]
Incluso podemos añadir y comprobador para ver si hay máquinas nuevas dentro de la plataforma vulnhub, evaluando si la máquina noob ha pasado a la segunda página o no; y en función de ello comprobamos si hay máquinas nuevas o no:
```
import requests
import re

website = "https://www.vulnhub.com/"
result = requests.get(website)
content = result.text

  

patron = r"/entry/[\w-]+"
maquinas_repetidas = re.findall(patron, str(content))
sin_duplicados = list(set(maquinas_repetidas))


maquinas_final = []
  

for i in sin_duplicados:
	nombre_maquinas = i.replace("/entry/", "")
	maquinas_final.append(nombre_maquinas)

print(nombre_maquinas)

# Comprobar si la máquina noob sigue estando y así sabemos si hay máquinas nuevas o no:

  

maquina_noob = "noob-1"

existe_noob = False

  

for a in maquinas_final:
	if a == maquina_noob:
	existe_noob = True
break


if existe_noob:
	print("No hay máquinas nuevas")
else:
	print("Hay máquinas nuevas!!!!")
```
Y este sería el resultado:
![[Pasted image 20230202131225.png]]
Y ahora por último le vamos a añadir colores dentro de las sentencias condicionales:
```python
import requests
import re
from colorama import Fore
  
website = "https://www.vulnhub.com/"
result = requests.get(website)
content = result.text

patron = r"/entry/[\w-]+"
maquinas_repetidas = re.findall(patron, str(content))
sin_duplicados = list(set(maquinas_repetidas))

maquinas_final = []

for i in sin_duplicados:
    nombre_maquinas = i.replace("/entry/", "")
    maquinas_final.append(nombre_maquinas)
    print(nombre_maquinas)
	
# Comprobar si la máquina noob sigue estando y así sabemos si hay máquinas nuevas o no:

maquina_noob = "noob-1"
existe_noob = False

for a in maquinas_final:
    if a == maquina_noob:
        existe_noob = True
        break

color_rojo = Fore.RED
color_verde = Fore.GREEN

if existe_noob:
    print(color_verde + "No hay máquinas nuevas")
else:
    print(color_rojo + "Hay máquinas nuevas!!!!")
```
Y así se vería:
![[Pasted image 20230202131852.png]]