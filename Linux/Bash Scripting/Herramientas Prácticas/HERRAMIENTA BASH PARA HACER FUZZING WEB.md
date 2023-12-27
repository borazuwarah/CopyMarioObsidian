Podemos crear un script en bash para hacer fuzzing web de la siguiente forma:
```bash
#!/bin/bash

# Verifica que se proporcionen dos argumentos: el diccionario y la URL
if [ $# -ne 2 ]; then
    echo "Uso: $0 <diccionario> <URL>"
    exit 1
fi

diccionario="$1"
url="$2"

total_lines=$(wc -l < "$diccionario")
current_line=0

# Verifica si el diccionario existe
if [ ! -f "$diccionario" ]; then
    echo "El diccionario '$diccionario' no existe."
    exit 1
fi

# Itera sobre cada línea en el diccionario
while read -r linea; do

    current_line=$((current_line + 1))
    echo -ne "Progreso: $current_line/$total_lines\r"

    respuesta=$(curl -s -o /dev/null -w "%{http_code}" "$url$linea/")

    # Verifica si el código de respuesta es 200 (OK)
    if [ "$respuesta" -eq 200 ]; then
        echo "URL accesible: $url$linea/"
    fi
done < "$diccionario"
```
E incluso podemos añadir un indicador de progreso en el script:
![[Pasted image 20231206105021.png]]
