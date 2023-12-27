Para detectar equipos o hosts conectados a mi red privada lo haría de la siguiente forma:
```bash
#!/bin/bash

for i in {1..255}; do
    timeout 1 bash -c "ping -c 1 192.168.0.$i" >/dev/null
    if [ $? -eq 0 ]; then
        echo "El host 192.168.0.$i está activo"
    fi
done
```
Y este sería el resultado:
![[Pasted image 20230414123535.png]]
También podemos hacer que si el usuario presiona control c el script se detenga:
```bash
#!/bin/bash

# Función para manejar la señal SIGINT (Ctrl+C)
function control_c() {
    echo "¡Detenido por el usuario!"
    exit 1
}

# Asociar la función control_c() al manejo de la señal SIGINT
trap control_c SIGINT

for i in {1..255}; do
    timeout 1 bash -c "ping -c 1 192.168.0.$i" >/dev/null
    if [ $? -eq 0 ]; then
        echo "El host 192.168.0.$i está activo"
    fi
done
```
![[Pasted image 20231103161704.png]]