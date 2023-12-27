Para poder eliminar archivos duplicados en linux, tenemos una herramienta llamada fdupes, mediante la cual podemos detectar archivos que sean iguales, por ejemplo de la siguiente forma:
![[Pasted image 20231103153809.png]]
Y ahora podemos usar la herramienta fdupes dentro del script:
```bash
#!/bin/bash

# Comprobar si fdupes está instalado
if ! command -v fdupes &> /dev/null
then
    echo "fdupes no está instalado. Por favor, instálalo para usar este script."
    exit 1
fi

# Buscar y mostrar los archivos duplicados en el directorio actual
archivos=$(fdupes -r . | sed 's/^..//')

# Eliminar los archivos duplicados de manera interactiva
echo "$archivos" | while read -r line; do
    rm "$line"
done
```
Por ejemplo tenemos estos archivos repetidos:
![[Pasted image 20231103154614.png]]
Y ahora ya no lo hay:
![[Pasted image 20231103160043.png]]
