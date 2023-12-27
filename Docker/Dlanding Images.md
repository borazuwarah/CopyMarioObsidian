Significa imagen huérfana, son aquellas imágenes que se muestran como "none" dentro de nuestra lista de docker images. Estas imágenes se crean cuando modificamos una de nuestras imágenes, hacemos un docker build y la versión antigua queda huérfana, ahí es donde se originan estas imágenes huérfanas. Esto ocurre porque cada capa del Dockerfile es de sólo lectura, entonces si modificamos una capa se crea una nueva imagen y queda huérfana la anterior:

![[Pasted image 20221217110311.png]]

Estas imágenes huérfanas se crean por ejemplo al modificar el Dockerfile y luego crear una imagen, por ejemplo:

![[Pasted image 20221217110342.png]]
Para evitar que ocurra lo anterior, lo arreglamos al definir tags en el momento de construir la imagen, por ejemplo, ejecutamos el docker build dándole un nombre

![[Pasted image 20221217110350.png]]

Y después, al construir esta imagen veremos que esta ya no se llama "none":
![[Pasted image 20221217110403.png]]
Ahora vamos a eliminar todas las imágenes huérfanas de docker:

![[Pasted image 20221217110411.png]]

Si quiero eliminar todas las imágenes huérfanas, debo primero filtrar por el ID estas imágenes; y después pasarle un comando que es xargs docker rmi:

![[Pasted image 20221217110418.png]]





