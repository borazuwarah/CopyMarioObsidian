RENOMBRAR: Para renombrar un contenedor, escribo docker rename nombre_actual nombre_nuevo:

![[Pasted image 20221217105215.png]]

DETENER SIN ELIMINAR: Escribimos docker stop id_contenedor:

![[Pasted image 20221217105221.png]]

PARA REANUDAR EL CONTENEDOR: Ponemos docker start id_contenedor:

![[Pasted image 20221217105228.png]]

PARA REINICIAR EL CONTENEDOR: Ponemos docker restart id_contenedor:

![[Pasted image 20221217105234.png]]

## **COMO INGRESAR A UN CONTENEDOR**

Ponemos docker exec -ti nombre_contenedor bash (para entrar dentro del contenedor usando una terminal bash):

![[Pasted image 20221217105246.png]]

Para acceder con permisos root:

![[Pasted image 20221217105253.png]]

Para eliminar todos los contenedores de docker a la vez, ejecutaremos este comando:

![[Pasted image 20230108200952.png]]
