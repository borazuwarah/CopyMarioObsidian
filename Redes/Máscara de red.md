El Subnetting es una técnica utilizada para dividir una red IP en subredes más pequeñas y manejables. Esto se logra mediante el uso de máscaras de red, que permiten definir qué bits de la dirección IP corresponden a la red y cuáles a los hosts.

Por ejemplo podemos acceder a ella utilizando simplemente el comando ifconfig:
![[Pasted image 20230411212702.png]]

El origen de los 255 sirve para representar la totalidad de dispositivos que se pueden conectar a la red; y se consigue con el siguiente cálculo:

![[Pasted image 20230411213301.png]]

### Hosts máximos según la máscara de subred al hacer subnetting

255.255.255.128 -> 126 hosts

255.255.254.0 -> 510 hosts

255.255.128.0 -> 32.766 hosts

255.224.0.0 -> 2.097.150 hosts

255.128.0.0 -> 8.388.606 hosts

0.0.0.0 -> 4.294.967.294 hosts











