IRQ -> Se trata de una señal que se envía a la CPU para que gestione una petición de hardware (teclado, ratón, etc...). Se muestra en /proc/interrupts.

DMA -> El acceso directo a memoria permite a cierto tipo de componentes de una computadora acceder a la memoria del sistema para leer o escribir independientemente de la CPU. 

Direcciones E/S -> Trozos de memoria para que los dispositivos se comuniquen con la CPU. El fichero es /proc/ioports.

Coldplug/hotplug -> Son dispositivos que se tienen que conectar en frío (cuando la máquina está apagada, por ejemplo un disco duro) o en caliente (mientras está en funcionamiento, por ejemplo un pendrive USB).

![[Pasted image 20230111211026.png]]
Sysfs -> Sistema virtual de ficheros que se encuentra en /sys/, donde se puede acceder a la información sobre los dispositivos.

D-Bus -> Para que los procesos se comuniquen y se notifiquen eventos. Permite que otros procesos se comuniquen entre ellos.

Udev -> Mantiene el directorio virtual de /dev (de device), cuando se detecta una nueva conexión de algún dispositivos, se crea el fichero y luego al desconectar el device vuelve a desaparecer dicho fichero porque ya no podremos acceder físicamente al dispositivo.
![[Pasted image 20230112003205.png]]
**DIRECTORIO PROC**
Por ejemplo, visualizando el fichero /proc/interrupts, podremos visualizar las señales asignadas del software al hardware:
![[Pasted image 20230112003657.png]]
**DIRECTORIO SYS**
En este directorio encontramos mucha información relacionada con el hardware, tal como estamos viendo:
![[Pasted image 20230112023221.png]]
Dentro del directorio /class podremos ver los dispositivos por clases:
![[Pasted image 20230112023318.png]]
Por ejemplo podemos listar los archivos de la carpeta net y veremos las interfaces de red existentes:
![[Pasted image 20230112023524.png]]
**DETECTAR DISPOSITIVOS O PENDRIVES CONECTADOS EN LINUX**
Para detectar los dispositivos conectados en Linux, podemos ejecutar el comando ```ls -l sd*``` y de esta manera visualizamos todos los dispositivos, por ejemplo veremos el caso de antes de insertar un pendrive y después insertándolo:
![[Pasted image 20230112024252.png]]
**PARA VER PARTICIONES DE LOS DISCOS**
Para ello tenemos que acceder al directorio /dev/disk; y aquí tendremos la carpeta by-label/ donde se encontrarán las particiones:
![[Pasted image 20230112025210.png]]
