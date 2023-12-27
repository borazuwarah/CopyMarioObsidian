Las variables de entorno son aquellas variables que podemos acceder desde cualquier parte del contenedor, (por ejemplo la variable prueba sería una variable de entorno:

![[Pasted image 20221217110647.png]]

Después, si construyo la imagen y entro a ella, existirá una variable de entorno que será $prueba, que contendrá 1234, ya que lo he definido dentro del Dockerfile:

![[Pasted image 20221217110654.png]]

También puedo crear una variable de entorno directamente cuando ejecuto el contenedor, sin necesidad de modificar el Dockerfile, lo haría de esta manera:

![[Pasted image 20221217110701.png]]

Y si entro dentro de este contenedor y miro el valor de la variable2 que acabamos de crear, veremos que existe y con el valor que le hemos asignado:

![[Pasted image 20221217110707.png]]
