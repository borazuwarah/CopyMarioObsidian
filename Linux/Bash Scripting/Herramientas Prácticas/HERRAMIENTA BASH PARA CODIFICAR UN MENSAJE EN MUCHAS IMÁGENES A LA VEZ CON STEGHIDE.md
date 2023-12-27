Este código lo que va a hacer es automatizar la inserción de un mensaje en varias imágenes a la vez utilizando la herramienta steghide:[[Esteganografía con Steghide, Foremost y BinWalk]]
```bash
#!/bin/bash

amarillo_luminoso='\033[1;33m'
sin_color='\033[0m'

archivos=$(ls)
contador=0

mkdir imagenes_con_mensaje

for variable in $archivos
do
  tipo=$(file -bi "$variable")
  if [[ $tipo == image/* ]]
  then
  contador=$((contador+1))
  steghide embed -ef clave -cf "$variable" -sf imagen_con_secreto"$contador".jpg -p '123123'
  fi
done

mv imagen_con_secreto* imagenes_con_mensaje/

echo -e "${amarillo_luminoso}Se ha creado una carpeta llamada imagenes_con_mensaje donde se guardaron todas las imágenes${sin_color}"
echo -e "${amarillo_luminoso}La contraseña para extraer el archivo de cada una de las imágenes es 123123${sin_color}"
```
Y funciona correctamente:
![[Pasted image 20230307072049.png]]
# SCRIPT PARA COMPROBAR QUÉ IMAGEN TIENE UN MENSAJE CODIFICADO
También se puede dar el caso de tener muchas imágenes y sólo 1 o 2 tienen un mensaje, por lo que podemos crear un script que haga esta comprobación:
![[Pasted image 20230613144903.png]]
Introducimos el mensaje secreto en una de las imágenes y le ponemos contraseña:
![[Pasted image 20230613145112.png]]
Haríamos así el código:
