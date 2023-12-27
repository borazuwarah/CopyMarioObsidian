Un DataFrame es una estructura de datos tabular que se utiliza para almacenar y manipular datos en filas y columnas. Por ejemplo de esta manera, donde tenemos un diccionario y queremos estructurar la información:
```python
import pandas as pd

data = {'Nombre': ['Manzana', 'Naranja', 'Pera', 'Limón'], 
        'Precio': [25, 30, 22, 27], 
        'Ciudad': ['Madrid', 'Barcelona', 'Valencia', 'Sevilla']}

df = pd.DataFrame(data)

print(df)
```
Y si imprimimos esto la información se mostrará de esta manera:
![[Pasted image 20230407073854.png]]
