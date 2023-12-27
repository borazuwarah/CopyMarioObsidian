En esta etapa de stage debemos primero abrir visual studio code desde el terminal en el mismo directorio, lo cual lo expresamos con el punto:
![[Pasted image 20230108122625.png]]
Ahora se nos abrirá el editor de código, donde crearé un archivo python de prueba:
![[Pasted image 20230108122631.png]]
Y ahora si vuelvo a la consola y pongo git status, podremos ver que el archivo prueba.py que he creado anteriormente no se encuentra seguido por git, ya que por defecto no lo hace:
![[Pasted image 20230108122638.png]]
Y ahora si quiero seleccionar el archivo, debo hacerlo con el comando git add prueba.py:
![[Pasted image 20230108122645.png]]
Y ahora si pongo un git status veremos como el archivo ya se encuentra seleccionado y de color verde:
![[Pasted image 20230108122652.png]]
Ahora si yo por ejemplo creo otro archivo, este por defecto no va a estar seguido por git como podemos ver aquí:
![[Pasted image 20230108122700.png]]
Ahora voy a añadir este archivo también con el comando git add:
![[Pasted image 20230108122707.png]]
También, si quisiera añadir varios archivos a la vez, lo haría de esta manera:
![[Pasted image 20230108122716.png]]
Qué ocurre si modifico un archivo, por ejemplo prueba.py y luego escribo un git status, veremos que git me marca que hubo una modificación:
![[Pasted image 20230108122727.png]]
Y ahora si pongo un git status ocurrirá lo siguiente:
![[Pasted image 20230108122737.png]]
Para actualizar estos cambios y añadirlos a git, debo volver a añadir el archivo:
![[Pasted image 20230108122744.png]]
# Fase de Commit (para comprometer nuestro trabajo)
Para ello utilizo el comando commit -m “Primer commit” → Aquí utilizo un título de este commit, para indicar las modificaciones que hice:
![[Pasted image 20230108122835.png]]
Ahora si usamos un git status veremos como no tenemos nada disponible para hacer un commit:
![[Pasted image 20230108122842.png]]
# ELIMINAR UN ARCHIVO Y ACTUALIZAR CAMBIOS EN ETAPA STAGE Y COMMIT

Si eliminamos un archivo, deberemos marcar este cambio dentro de la etapa de stage, ya que ahora si miramos los cambios con git status veremos que nos marca que eliminamos un archivo:
![[Pasted image 20230108123000.png]]
Y ahora para actualizar este cambio escribiremos git add archivo2.py, y así ya tendremos actualizado en nuestra etapa de stage esta modificación:
![[Pasted image 20230108123013.png]]
Y ahora el siguiente paso será también hacer un commit para actualizar este cambio también:
![[Pasted image 20230108123023.png]]
