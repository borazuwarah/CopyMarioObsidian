Es un estandar para los protocolos de red que describe cómo los dispositivos de red se comunican entre sí a través de una red.

### CAPAS DEL MODELO OSI

- **Capa física** -->  Esta capa se encarga de la transmisión de señales físicas a través de medios de transmisión, como cables y fibra óptica. Esta capa idenficia los dispositivos como hoops y los medios de transmisión como los cables de red. 

  Algunos de los protocolos que funcionan en esta capa son los siguientes -> Ethernet, USB, Bluetooth y Wi-Fi.
  
- **Capa de enlace de datos -->** Esta capa inspecciona que los paquetes transmitidos anteriormente están correctos, además de controlar el flujo sobre el que se envían los paquetes. 

- **Capa de red -->** Esta capa se encarga de la dirección lógica y el enrutamiento de los datos a través de la red. Aquí tenemos las IP de origen y destino. Además de decidir qué ruta seguir para hacer el envío de dichos paquetes.
  

![[Pasted image 20230411212148.png]]

Protocolos que funcionan en esta capa --> IP, ICMP, OSPF, BGP o RIP.

- **Capa de transporte -->** Esta capa proporciona servicios de transporte de extremo a extremo para el intercambio de datos entre dispositivos finales. 

   Esta capa es la que garantiza el envío y recepción de los paquetes. Los protocolos UDP y TCP son los que se encuentran en esta capa.

![[Pasted image 20230411212206.png]]

Protocolos que funcionan en esta capa --> TCP, UDP, SCTP O DCCP.

- **Capa de sesión -->** Esta capa establece, gestiona y termina las sesiones entre los distintos hosts.
Protocolos que funcionan en esta capa --> NetBIOS, PPTP o SMB.

- **Capa de presentación -->** Esta capa se encarga de la presentación de datos para que puedan ser entendidos por las aplicaciones receptoras.
Protocolos que funcionan en esta capa --> JPEG, MPEG, SSL/TLS o ASCII.

- **Capa de aplicación-->** Esta capa proporciona servicios de red a las aplicaciones del usuario final, como correo electrónico y navegación web.

   En esta capa es donde actual los protocolos como FTP, HTTP, SMTP, DNS o SNMP.