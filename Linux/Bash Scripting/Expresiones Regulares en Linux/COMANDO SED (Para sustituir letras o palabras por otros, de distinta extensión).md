Por ejemplo vamos a sustituir la palabra Hola por la palabra Mario con el comando sed:
![[Pasted image 20230128181636.png]]
Incluso también podemos sustituir un espacio por lo que queramos, por ejemplo donde había un espacio ponemos un guión:
![[Pasted image 20230128181721.png]]
Lo que pasa que sólo nos sustituye con la primera coincidencia, por lo que si queremos hacer una sustitución en todo el documento debemos poner una 'g' al final:
![[Pasted image 20230128181813.png]]
También puedo eliminar o modificar el último elemento de una cadena de texto, escribiendo sed ‘s/.$//’
![[Pasted image 20230515145932.png]]
**Eliminar filas con sed**
Podemos también eliminar filas con el comando sed, donde tenemos el siguiente texto:
![[Pasted image 20231125134422.png]]
Y ahora podemos eliminar la tercera file con sed '3d':
![[Pasted image 20231125134502.png]]
Aunque también podemos eliminar más de dos filas a la vez de la siguiente forma:
![[Pasted image 20231125134603.png]]
**Añadir texto al final de cada línea**
También podemos añadir texto al final de cada línea de un archivo, por ejemplo vamos a suponer que al final de cada línea quiero añadir la frase 'El gato es feliz', pues lo haría de la siguiente manera:
```bash
cat texto.txt | sed -i 's/$/ El gato es feliz/' texto.txt
```
![[Pasted image 20231127175130.png]]
