Esta sentencia establece una acción que se debe tomar cuando el script recibe la señal SIGINT, que se produce cuando el usuario presiona la combinación de teclas "Ctrl + C" en la terminal:
```bash
#!/bin/bash

function finish {
    echo "Finalizando el script"
    exit
}

trap finish SIGINT

echo "Presiona Ctrl + C para interrumpir el script"

while true; do
    sleep 1
done
```
Y este sería el resultado:
![[Pasted image 20230414120320.png]]
