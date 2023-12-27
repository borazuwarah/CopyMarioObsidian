Lo hacemos con el siguiente script:
```bash
#!/bin/bash

arp-scan -I eth0 --localnet | grep -v "ip" |  grep -v "Starting" | grep -v "pa" | grep -v "Interface" | grep -v "Ending" | awk '{print $1}' | tr -d ' ' > ip.txt

while read -r linea; do
    echo -e "\e[38;5;11mEscaneando con nmap la dirección $linea\e[0m"

    nmap -p- -sS -sC -sV --min-rate=5000 -n -Pn $linea -oN "escaneo_$linea.txt"

done < ips.txt
rm ips.txt
```
