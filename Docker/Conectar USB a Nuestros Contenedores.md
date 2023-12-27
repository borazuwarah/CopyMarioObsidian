**docker run -t -i  –privileged -v /dev/bus/usb:/dev/bus/usb [nombre_contenedor] bash**

Con este comando, estamos indicando que queremos tener en el contenedor los mismos dispositivos que detecta el sistema y poder interactuar con ellos.