```bash
#!/bin/bash

# Obtener la lista de archivos en el directorio actual
archivos=$(ls)

# Recorrer la lista de archivos y mostrar su tamaño en bytes
for archivo in $archivos
do
  # Verificar si es un archivo (no un directorio)
  if [ -f "$archivo" ]; then
    espacio=$(du -b "$archivo" | awk '{print $1}')
    echo "El archivo $archivo ocupa $espacio bytes"
  fi
done
```
![[Pasted image 20230924184218.png]]
Podemos adaptar este script para que se encargue de elimnar aquellos archivos que ocupen más de 1 mb:
```bash
#!/bin/bash

# Obtener la lista de archivos en el directorio actual
archivos=$(ls)

# Recorrer la lista de archivos y mostrar su tamaño en bytes
for archivo in $archivos
do
  # Verificar si es un archivo (no un directorio)
  if [ -f "$archivo" ]; then
    espacio=$(du -b "$archivo" | awk '{print $1}')
    if [ $espacio -gt 1000000 ]; then
        echo "Eliminamos el archivo $archivo, ocupa más de 1 MB"
        rm "$archivo"
    fi
  fi
done
```