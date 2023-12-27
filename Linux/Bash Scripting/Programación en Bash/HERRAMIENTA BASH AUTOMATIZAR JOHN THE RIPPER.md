Podemos automatizar ataques con john the ripper desde un script de bash:
```bash
#!/bin/bash

# Comprobar si se proporciona el archivo como argumento
if [ $# -ne 2 ]; then
    echo "Uso: $0 <diccionario> <archivo>"
    exit 1
fi

diccionario=$1
archivo=$2

# Comprobar si el archivo es un .rar
if [[ $archivo == *.rar ]]; then
    echo "Auditoría de archivo .rar con John the Ripper"
    rar2john $archivo > hash
    john --wordlist=$diccionario hash

# Comprobar si el archivo es un .zip
elif [[ $archivo == *.zip ]]; then
    zip2john $archivo > hash
    john --wordlist=$diccionario hash

# Archivo no compatible
else
    echo "El formato del archivo no es compatible"
    exit 1
fi

# Mostrar contraseñas encontradas
echo "Contraseñas encontradas:"
john --show $archivo

rm hash
```
![[Pasted image 20231206113007.png]]