Iterar por líneas en un bucle while:
```bash
while read -r linea; do
    echo $linea
done < "archivo.txt"
```
![[Pasted image 20230927125812.png]]
También podemos recibir un documento como parámetro y leer línea por línea su contenido:
```bash
#!/bin/bash

documento="$1"

while read -r linea; do
    echo $linea
done < "$documento"
```
![[Pasted image 20230927125759.png]]
También podemos automatizar muchas cosas usando un bucle while, como por ejemplo filtrar por códigos de estado de distintas peticiones http con curl:
```bash
while read -r url; do
    respuesta=$(curl -s -o /dev/null -w "%{http_code}" "$url")
 
    if [ "$respuesta" == "200" ]; then
        echo "En $url hay respuesta con código de estado $respuesta"
    fi
done < "texto.txt"

```
![[Pasted image 20230927131003.png]]
