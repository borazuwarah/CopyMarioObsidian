Esto es una forma de enlazar una carpeta de mi equipo anfitrión con otra carpeta del equipo contenedor, así se crea una copia de seguridad en mi equipo anfitrión de los datos.

Primero levanto un contenedor por ejemplo de ubuntu donde vinculo mi directorio con el directorio /home dentro del contenedor:
![[Pasted image 20221231200259.png]]
Ahora mismo todo lo que cree dentro del contenedor en la carpeta home se me copiará también al directorio de mi ordenador que es mario/Escritorio/bindmounts/ejemplo:
![[Pasted image 20221231200313.png]]
Lo que cree en el contenedor se duplica al ordenador anfitrión:
![[Pasted image 20221231200332.png]]
