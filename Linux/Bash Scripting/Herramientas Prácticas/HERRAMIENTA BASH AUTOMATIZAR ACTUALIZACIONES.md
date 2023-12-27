```bash
#!/bin/bash

read -p "Escribe 'actualizar' si quieres actualizar, y 'salir' si quieres salir: " decision

if [ "$decision" = "actualizar" ]; then
    apt update
    apt upgrade -y
    apt autoremove -y
    apt autoclean
    echo "¡Actualización completada!"
elif [ "$decision" = "salir" ]; then
    exit
else
    echo "Opción no válida"
fi
```