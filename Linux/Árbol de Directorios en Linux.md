Así es el árbol de directorios de Linux:
![[Pasted image 20230528212311.png]]
EXPLICACIÓN DE CADA UNO DE LOS DIRECTORIOS:

- /: El directorio raíz, representado por una barra (/), es el punto de partida del sistema de archivos. Contiene todos los demás directorios y archivos del sistema.
- /etc: Aquí se almacenan los archivos de configuración del sistema. Los archivos en este directorio controlan el comportamiento de los programas y servicios en el sistema, como la configuración de red, los usuarios y grupos, los servicios de inicio y más. (por ejemplo el archivo de configuración de crontab).
- /bin: Aquí se encuentran los archivos ejecutables (binarios) que son esenciales para el funcionamiento del sistema. Estos archivos incluyen comandos básicos utilizados por los usuarios y programas del sistema.
- /boot: Contiene los archivos necesarios para el arranque del sistema, como el núcleo del sistema operativo (kernel), los archivos de configuración del gestor de arranque (como GRUB) y otros archivos relacionados con el proceso de inicio.
- /dev: Este directorio contiene archivos especiales que representan y brindan acceso a dispositivos de hardware. Por ejemplo, los dispositivos de almacenamiento (discos duros, unidades USB) se representan como archivos en /dev.
- /etc: Aquí se almacenan los archivos de configuración del sistema. Los archivos en este directorio controlan el comportamiento de los programas y servicios en el sistema, como la configuración de red, los usuarios y grupos, los servicios de inicio y más. (por ejemplo el archivo de configuración de crontab).
- /home: Cada usuario tiene su propio directorio personal en /home, donde pueden almacenar sus archivos personales y configuraciones. Por ejemplo, si el nombre de usuario es "usuario", su directorio personal se encuentra en /home/usuario.
- /lib y /lib64: Estos directorios contienen las bibliotecas compartidas (archivos .so) necesarias para que los programas en el sistema funcionen correctamente. /lib es para sistemas de 32 bits, mientras que /lib64 es para sistemas de 64 bits.
- /media: Cuando se montan dispositivos extraíbles, como CD-ROM, unidades USB o discos externos, se montan en este directorio.
- /opt: Este directorio se utiliza para instalar aplicaciones de terceros (opcionales). Los programas instalados en /opt suelen tener su propia estructura de directorios interna.
- /tmp: Es un directorio utilizado para almacenar archivos temporales. Los programas y el sistema pueden utilizarlo para almacenar datos temporales que no necesitan persistir a largo plazo.
- /usr: Contiene archivos y directorios relacionados con programas y datos del usuario. Aquí se encuentran los binarios, bibliotecas, archivos de encabezado y documentación relacionados con las aplicaciones instaladas en el sistema.
- /var: Este directorio almacena archivos y datos variables, como registros del sistema (log files), correos electrónicos, archivos de bases de datos, archivos spool de impresión, entre otros.