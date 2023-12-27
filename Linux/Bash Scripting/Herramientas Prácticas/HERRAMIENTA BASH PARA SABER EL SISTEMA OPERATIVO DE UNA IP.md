También podemos saber en cuántas líneas se repite un patrón utilizando la opción -c, donde en este caso nos dice que la palabra ttl=64 dentro del fichero ping.log se repite una vez, por tanto podemos saber que es un linux:
![[Pasted image 20230101174501.png]]
![[Pasted image 20230101174506.png]]
Ahora vamos a expresar esto con distintos colores para que quede muy bien; y de una forma mejorada:
```bash
#!/bin/bash

rojo_luminoso='\033[1;31m'
verde_luminoso='\033[1;32m'
cyan_luminoso='\033[1;36m'
sin_color='\033[0m'

read -p "Introduce la dirección IP: " IP

# Realizamos un ping al host y verificamos si el host es accesible
if ping -c 1 $IP > /dev/null 2>&1; then
  echo "El host es accesible"

  # Obtenemos el valor del TTL
  TTL=$(ping -c 1 $IP | grep "ttl=" | awk '{print $6}' | tr -d "ttl=")

  echo "El TTL detectado es de $TTL"

  # Analizamos el valor del TTL para determinar el sistema operativo
  if [ $TTL -gt 60 ] && [ $TTL -lt 100 ]; then
    echo -e "${verde_luminoso}El sistema operativo es Linux/Unix ${sin_color}" #Aquí restauramos el color original.
  elif [ $TTL -gt 100 ] && [ $TTL -lt 170 ]; then
    echo -e "${cyan_luminoso}El sistema operativo es Windows ${sin_color}"
  else
    echo -e "${rojo_luminoso}No se puede determinar el sistema operativo con certeza ${sin_color}"
  fi
else
  echo -e "${rojo_luminoso}El host no es accesible ${sin_color}"
fi
```
Y este es el resultado:
![[Pasted image 20230211004408.png]]
También podemos convertir este mismo código en Python, utilizando la librería OS:
```python
import os

def detect_os(ip):
    # Ejecutar el comando ping
    response = os.system("ping -c 1 " + ip)
    if response == 0:
        # Obtener el valor del TTL
        ttl = os.popen("ping -c 1 " + ip + " | grep ttl | awk '{print $7}' | tr -d 'ttl='").read()
        ttl = int(ttl)

        # Analizar el valor del TTL para determinar el sistema operativo
        if ttl > 60 and ttl < 100:
            return "El sistema operativo es Linux/Unix"
        elif ttl > 100 and ttl < 170:
            return "El sistema operativo es Windows"
        else:
            return "No se puede determinar el sistema operativo con certeza"
    else:
        return "El host no es accesible"

# Obtener la dirección IP
ip = input("Introduce la dirección IP: ")

# Detectar el sistema operativo
os_name = detect_os(ip)

# Imprimir el resultado
print(os_name)
```
![[Pasted image 20230211005748.png]]