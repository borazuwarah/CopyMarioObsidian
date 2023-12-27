Es útil para hacer que los cambios que hacemos dentro de un contenedor persistan.

En primer lugar, voy a hacer una modificación a este contenedor, por ejemplo creando un documento de texto:
![[Pasted image 20221217105437.png]]

Puedo capturar el estado actual del contenedor y guardarlo tal como está en una imagen. De esta manera los cambios se hacen permanentes. Debo escribir docker commit nombre_contenedor nombre_nuevo_imagen:

![[Pasted image 20221217105444.png]]

Ahora si ejecuto la imagen-resultante podré ver que el documento se mantiene y ya habremos convertido un contenedor en una imagen:

![[Pasted image 20221217105450.png]]

AVISO: Todo lo que esté dentro de un VOLUME cuando hagamos el Dockerfile sí se perderá.
