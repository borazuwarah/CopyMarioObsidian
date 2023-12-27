```bash
#!/bin/bash

# Ejecutar el comando netstat para obtener una lista de puertos abiertos
open_ports=$(netstat -tln | awk '{print $4}' | tr -d "Local" | tr -d "(ny" | sed 's/:/ /g' | awk '{print $2}')

# Mostrar los puertos abiertos
echo "Puertos abiertos: $open_ports"
```
Y por ejemplo si corremos dos servidores web con Python, podremos luego comprobar el buen funcionamiento de esta herramienta:
![[Pasted image 20230218142330.png]]
![[Pasted image 20230218142343.png]]
Y ahora si lanzamos el script, nos detecta estos dos puertos que están en uso:
![[Pasted image 20230218142406.png]]
