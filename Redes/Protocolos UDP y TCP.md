## PROTOCOLO TCP

Protocolo para comunicarse por internet. 

CARACTERÍSTICAS: 
- Proporciona verificación de errores.
- Garantiza la entrega de datos.
- Los paquetes se entregan en el orden en que fueron enviados.

### THREE-WAY Handshake

Esto es el proceso de tres pasos que se sigue para establecer una conexión TCP con el siguiente proceso:

1 - SYN --> El dispositivo que inicia la conexión envía un paquete SYN (Synchronize) al dispositivo receptor.

2 - SYN-ACK --> El dispositivo receptor responde con un paquete SYN-ACK (Synchronize-Acknowledgment). Este paquete indica que el dispositivo receptor está disponible para establecer una conexión.

3 - ACK --> El dispositivo que inició la conexión responde con un paquete ACK (Acknowledgment). Este paquete confirma que se ha establecido una conexión entre los dos dispositivos.

Por ejemplo, si nos establecemos una conexión con netcat a nosotros mismos, a nuestra dirección loopback, veremos que se establece una conexión Three-way handshake. Por lo que primero abrimos wireshark poniendonos en escucha por dicha dirección loopback:

![[Pasted image 20230411205254.png]]
Ahora nos quedará todo en blanco, lo cual es normal:
![[Pasted image 20230411205334.png]]
Establecemos una conexión TCP dentro de nuestra propia máquina por el puerto 443:
![[Pasted image 20230411205506.png]]
Y en wireshark podemos ver este mismo proceso como comienza con un SYN, luego un SYN-ACK y luego por último un ACK:
![[Pasted image 20230411205702.png]]

## PROTOCOLO UDP

Se trata de un protocolo sin conexión, lo que significa que no establece una conexión antes de enviar datos. UDP no establece un canal de comunicación confiable antes de enviar datos. 

Esto hace que el protocolo UDP sea más rápido y eficiente que otros protocolos de comunicación que requieren una conexión confiable, como TCP.

-------------------------------------------------------

### SERVICIOS QUE SUELEN CORRER POR CADA UNO DE ESTOS DOS PROTOCOLOS

**TCP**

21 -> FTP
22 -> SSH
23 -> Telnet
25 -> SMTP
53 -> DNS
80 -> HTTP
443 -> HTTPS
110 -> POP3
139, 445 -> SMB
143 -> IMAP

**UDP**

53 -> DNS
67/68 -> DHCP
69 -> TFTP
123 -> NTP (Para sincronizar los relojes de los dispositivos en una red)
161 -> SNMP (Para administrar y supervisar dispositivos en una red)

