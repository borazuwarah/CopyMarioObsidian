**MBR**

Contiene una tabla de 4 particiones dentro de cada disco; y una de estas particiones se puede dedicar a una extendida para poder guardar varias particiones lógicas; y de esta forma que acaba con la restricción de las 4 particiones.
Tampoco puede manejar particiones de más de 2 TB.
![[Pasted image 20230125154923.png]]
**GPT**

- En primer lugar es compatible con MBR. Pero no funciona con BIOS, sino que necesita EFI o UEFI.
- Soporta tamaños de disco de hasta 9,4 zettabytes.
- Puede gestionar aproximadamente 128 particiones.
- Gestiona mejor el arranque del S.O.

## COMANDO PARTED

Si queremos comprobar la tabla de particiones de nuestro sistema, podemos ejecutar el comando parted -l:
![[Pasted image 20230125155330.png]]
Por ejemplo si fuese MBR, nos lo mostraría de esta ofra forma, donde vemos que nos pone msdos, que es lo mismo que MBR:
![[Pasted image 20230125155621.png]]
