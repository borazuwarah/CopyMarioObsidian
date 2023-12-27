**Puerto 25 ->** Se trata del puerto SMPT, el cual sirve para la transferencia de correo electrónico. 

**Puerto 53 ->** Es el puerto estándar utilizado por el protocolo de Sistema de Nombres de Dominio. El protocolo DNS se utiliza para traducir los nombres de dominio que las personas utilizan para acceder a sitios web, como [www.ejemplo.com](http://www.ejemplo.com/), en direcciones IP numéricas que los dispositivos de red utilizan para comunicarse entre sí.

**Puerto 443 ->** Es el puerto estándar utilizado por el protocolo HTTPS para enviar y recibir datos cifrados a través de Internet. HTTPS es una variante segura del protocolo HTTP que utiliza cifrado de datos SSL/TLS para proteger la información transmitida entre el servidor web y el cliente.

**Puerto 139 ->** Es utilizado por el protocolo NetBIOS (Network Basic Input/Output System) sobre TCP/IP. Este protocolo es utilizado para compartir archivos e impresoras en redes locales (LAN) y también puede ser utilizado para la administración remota de sistemas.

Es importante tener en cuenta que el puerto 139 también puede ser utilizado por malware y virus para propagarse en una red.

**Puerto 445 ->** En este puerto se utiliza el servicio "microsoft-ds" que se refiere al servicio de Microsoft Directory Service para proporcionar la funcionalidad del protocolo SMB (Server Message Block). Este protocolo permite compartir archivos, impresoras y otros recursos en una red, y es utilizado en sistemas operativos Windows para la gestión de recursos compartidos y en la implementación de Active Directory.

En resumen, el puerto 139 es más antiguo y menos seguro en comparación con el puerto 445, que es una versión más moderna y segura para el protocolo SMB. En la práctica, se recomienda deshabilitar el puerto 139 en favor del puerto 445 para mejorar la seguridad de la red.
# SERVICIOS / PROTOCOLOS

**Microsoft RPC ->** Microsoft RPC (Remote Procedure Call) es un protocolo utilizado para la comunicación entre procesos y servicios en el sistema operativo, lo que permite que diferentes componentes del sistema interactúen entre sí.

Por ejemplo, Microsoft RPC se utiliza en la implementación de Active Directory, permitiendo que los servidores de dominio y los clientes de dominio se comuniquen entre sí.

