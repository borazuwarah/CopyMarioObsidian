Tenemos la librería shodan para realizar búsquedas desde Python utilizando su API. Por ejemplo si queremos hacer una búsqueda de equipos con windows XP, lo haríamos de esta forma:
```python
import shodan

# introduce tu clave de API de Shodan
API_KEY = "INSERTAR API"

# crea una instancia del objeto API de Shodan
api = shodan.Shodan(API_KEY)

# realiza una búsqueda en Shodan por equipos con Windows XP instalado
query = "Windows XP"
results = api.search(query)

# imprime información básica sobre los resultados de la búsqueda
print(f"Se encontraron {results['total']} resultados:")
for result in results['matches']:
    print(f"IP: {result['ip_str']}\nPuerto: {result['port']}\nOrganización: {result.get('org', 'N/A')}\n")
```
Y este sería el output:
![[Pasted image 20230413105044.png]]
Ahora por ejemplo vamos a buscar por vídeo cámaras:
```python
import shodan

# introduce tu clave de API de Shodan
API_KEY = "INSERTAR API"

# crea una instancia del objeto API de Shodan
api = shodan.Shodan(API_KEY)

# realiza una búsqueda en Shodan por equipos por cámaras de vídeo
query = "camera"
results = api.search(query)

# imprime información básica sobre los resultados de la búsqueda
print(f"Se encontraron {results['total']} resultados:")
for result in results['matches']:
    print(f"IP: {result['ip_str']}\nPuerto: {result['port']}\nOrganización: {result.get('org', 'N/A')}\n")
```
Y este es el resultado:
![[Pasted image 20230413105438.png]]
Incluso también podríamos exportar toda esta información a una hoja de excel usando la librería openpyxl:
```python
import shodan
import openpyxl

# introduce tu clave de API de Shodan
API_KEY = "INSERTAR API"

# crea una instancia del objeto API de Shodan
api = shodan.Shodan(API_KEY)

# realiza una búsqueda en Shodan por cámaras de vídeo
query = "camera"
results = api.search(query)

# imprime información básica sobre los resultados de la búsqueda
print(f"Se encontraron {results['total']} resultados:")
for result in results['matches']:
    print(f"IP: {result['ip_str']}\nPuerto: {result['port']}\nOrganización: {result.get('org', 'N/A')}\n")

wb = openpyxl.Workbook()
sheet = wb.active

# escribe las cabeceras de las columnas en la primera fila de la hoja de trabajo
sheet.cell(row=1, column=1).value = "IP"
sheet.cell(row=1, column=2).value = "Puerto"
sheet.cell(row=1, column=3).value = "Organización"

# escribe los resultados de la búsqueda en las filas siguientes
for i, result in enumerate(results['matches']):
    sheet.cell(row=i+2, column=1).value = result['ip_str']
    sheet.cell(row=i+2, column=2).value = result['port']
    sheet.cell(row=i+2, column=3).value = result.get('org', 'N/A')

# guarda el archivo de Excel
wb.save("resultados_shodan.xlsx")
wb.close()
```
Y este es el resultado:
![[Pasted image 20230413105706.png]]
También podríamos sacar sólo las direcciones IP, simplemente extrayendo eso del diccionario:
```python
import shodan

# introduce tu clave de API de Shodan
API_KEY = "INSERTAR API"

# crea una instancia del objeto API de Shodan
api = shodan.Shodan(API_KEY)

# realiza una búsqueda en Shodan por equipos con Windows XP instalado
query = "camera"
results = api.search(query)

# imprime información básica sobre los resultados de la búsqueda
print(f"Se encontraron {results['total']} resultados:")
for result in results['matches']:
    print(result['ip_str'])
```
Y este es el resultado:
![[Pasted image 20230413111212.png]]
Y estas direcciones IP también las podemos guardar en una hoja de excel:
```python
import shodan
import openpyxl

# introduce tu clave de API de Shodan
API_KEY = "INSERTAR API"

# crea una instancia del objeto API de Shodan
api = shodan.Shodan(API_KEY)

# realiza una búsqueda en Shodan por equipos con Windows XP instalado
query = "camera"
results = api.search(query)

# imprime información básica sobre los resultados de la búsqueda
print(f"Se encontraron {results['total']} resultados:")
for result in results['matches']:
    print(result['ip_str'])

wb = openpyxl.Workbook()
sheet = wb.active

# escribe las cabeceras de las columnas en la primera fila de la hoja de trabajo
sheet.cell(row=1, column=1).value = "IP"

# escribe los resultados de la búsqueda en las filas siguientes
for i, result in enumerate(results['matches']):
    sheet.cell(row=i+2, column=1).value = result['ip_str']

# guarda el archivo de Excel
wb.save("resultados_shodan_IP.xlsx")
wb.close()
```
Y lo tenemos todo en un excel:
![[Pasted image 20230413111815.png]]