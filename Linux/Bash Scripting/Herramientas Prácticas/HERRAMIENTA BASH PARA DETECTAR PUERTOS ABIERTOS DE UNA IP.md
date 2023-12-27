Podemos crear un script para detectar puertos abiertos de un hos, utilizando la herramienta de netcat para ir haciendo ping a un determinado puerto:

```bash
#!/bin/bash

finish() {
    echo -e "\n[*] Cerrando el script..."
    exit 0
}

trap finish SIGINT

read -p "Introduce la IP: " ip_address

if [ "$ip_address" ]; then
    for port in $(seq 1 65535); do
        (echo > /dev/tcp/$ip_address/$port) 2>/dev/null && echo "[*] Puerto $port - Abierto" &
    done
    wait
fi
```
Y este sería el resultado:
![[Pasted image 20230414120042.png]]

