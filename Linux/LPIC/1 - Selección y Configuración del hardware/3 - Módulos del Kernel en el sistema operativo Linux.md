¿Que son los módulos? Los módulos son partes del kernel que se pueden activar o desactivar para quitar o añadir funcionalidades, los cuales están muy vinculados con los driver. Por ejemplo módulo para la disquetera floppy.
![[Pasted image 20230115200036.png]]
Tenemos estos comandos para gestionar los módulos:
**lsmod ->** Listar los módulos que están funcionando actualmente en memoria.
**modinfo ->** Amplía la información de un módulo que hayamos visto con el comando anterior.
**insmod ->** Inserta un nuevo módulo, los cuales funcionan con archivos con extensión .ko.
**rmmod ->** Quita un módulo del sistema.
**modprobe ->** Carga o borra módulos resolviendo las dependencias entre estos.
![[Pasted image 20230115200303.png]]
![[Pasted image 20230115200455.png]]
## COMANDO LSMOD
Para listar los módulos lo haremos con este comando y se verá de esta forma:
![[Pasted image 20230115200719.png]]
## COMANDO MODINFO
Si queremos obtener más información de un módulo, utilizamos este otro comando seguido del nombre del módulo que queramos consultar:
![[Pasted image 20230115200937.png]]
## COMANDO INSMOD
Si quisiéramos cargar un módulo, debemos tener el fichero con extensión .ko; y con el comando insmod podríamos cargarlo en el sistema:
![[Pasted image 20230115201251.png]]
## COMANDO MODPROBE
Vamos a ver ahora cómo con el comando modprobe eliminamos un módulo y luego si lo volvemos a buscar veremos que no aparece:
![[Pasted image 20230115201356.png]]
Si quisiéramos cargar este módulo otra vez lo podríamos hacer con el comando modprobe igualmente:
![[Pasted image 20230115201656.png]]

