El comando `comm` en Linux se utiliza para comparar dos archivos de texto línea por línea.

Por ejemplo vamos a trabajar sobre estos dos archivos:
![[Pasted image 20230628124737.png]]

---------------------------

Este comando muestra las líneas que están presentes únicamente en `archivo1.txt`, pero no en `archivo2.txt`.
```bash
comm -23 archivo1.txt archivo2.txt
```
Y este sería el resultado:
![[Pasted image 20230628124830.png]]
Este comando muestra las líneas que están presentes tanto en `archivo1.txt` como en `archivo2.txt`.
```bash
comm -12 archivo1.txt archivo2.txt
```
Y este sería el resultado:
![[Pasted image 20230628124916.png]]
Este comando muestra las líneas que están presentes únicamente en `archivo2.txt`, pero no en `archivo1.txt`, y las líneas comunes se omiten.
```bash
comm -13 archivo1.txt archivo2.txt
```
Y este sería el resultado:
![[Pasted image 20230628125111.png]]