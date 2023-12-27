Ejemplo con números:
```bash
#!/bin/bash

for i in {1..5}
do
  echo -n "$i "
done
echo
```
Ahora ejemplo con cadenas de texto:
```bash
#!/bin/bash

frutas=("manzana" "naranja" "plátano" "pera" "uva")

for fruta in "${frutas[@]}"
do
  echo "Me gusta la $fruta"
done
```
Iterar sobre archivos de un directorio e imprimir sus nombres o hacer cualquier acción sobre cada uno de ellos:
```bash
#!/bin/bash

for archivo in /ruta/a/directorio/*
do
  echo "Nombre de archivo: $archivo"
done
```
Leer líneas de un archivo y mostrarlas en pantalla:
```bash
#!/bin/bash

archivo="ruta/al/archivo.txt"

for linea in $(cat "$archivo")
do
  echo "$linea"
done
```
