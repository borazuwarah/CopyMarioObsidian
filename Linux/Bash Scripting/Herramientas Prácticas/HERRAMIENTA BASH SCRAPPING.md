Vamos a crear una herramienta para detectar si se subieron nuevas máquinas en la web de vulnhub. Por ejemplo, así se pueden filtrar las máquinas que hay en la primera portada de la web de vulnhub:
```bash
cat vulnhub | grep 'href="/entry/' | tr -d '#' | sed 's/entry/ /' | awk '{print $3}' | tr -d '/' | sed 's/">/ /' | sed 's/download/ /' | sed 's/"/ /' | tr -d " " | uniq
```
Ahora vamos a evaluar si hay máquinas nuevas o no en función del número de ellas que encuentre, dentro de un script:
```bash
#!/bin/bash

curl https://vulnhub.com > log.log 2>/dev/null

contar=$(cat log.log | grep 'href="/entry/' | tr -d '#' | sed 's/entry/ /' | awk '{print $3}' | tr -d '/' | sed 's/">/ /' | sed 's/download/ /' | sed 's/"/ /' | tr -d " " | uniq | wc -l)

echo "Estas son las últimas máquinas disponibles: "

cat log.log | grep 'href="/entry/' | tr -d '#' | sed 's/entry/ /' | awk '{print $3}' | tr -d '/' | sed 's/">/ /' | sed 's/download/ /' | sed 's/"/ /' | tr -d " " | uniq

echo $maquinas

if [ $contar -lt "13" ]; then

        echo "Sigue habiendo 12 máquinas o menos publicadas como siempre"

else

        echo "HAY MÁQUINAS NUEVAS!!!"

fi

rm log.log
```
Y este sería el resultado:
![[Pasted image 20230128135815.png]]
Y ahora adaptamos el script a la web donde de verdad nos dirá si hay máquinas nuevas o no, dependiendo de si deja de salir la última máquina porque pasa a la página 2 en caso de añadir alguna nueva:
```bash
#!/bin/bash

curl https://vulnhub.com > log.log 2>/dev/null

contar=$(cat log.log | grep 'href="/entry/' | tr -d '#' | sed 's/entry/ /' | awk '{print $3}' | tr -d '/' | sed 's/">/ /' | sed 's/download/ /' | sed 's/"/ /' | tr -d " " | uniq | wc -l)

echo "Estas son las últimas máquinas disponibles: "

cat log.log | grep 'href="/entry/' | tr -d '#' | sed 's/entry/ /' | awk '{print $3}' | tr -d '/' | sed 's/">/ /' | sed 's/download/ /' | sed 's/"/ /' | tr -d " " | uniq

echo $maquinas

cat log.log | grep noob >/dev/null # Aquí vemos si la máquina pasa a otra página, lo que implica que hay una máquina nueva.

if [ "$(echo $?)" == "0" ]; then

        echo "No hay máquinas nuevas"

else

        echo "HAY MÁQUINAS NUEVAS!!!"

fi

rm log.log
```
