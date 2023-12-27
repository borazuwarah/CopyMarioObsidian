Son comandos para obtener información del hardware de nuestra máquina.

**iscpi ->** Muestra información sobre los buses PCI y los dispositivos que tienen conectados. Podemos utilizar estas dos opciones: -v para más información, y -s para información sólo del dispositivo:
![[Pasted image 20230112214257.png]]
Si ejecutamos el comando lspci veremos la información de los buses:
![[Pasted image 20230112214525.png]]
Ahora vamos a aplicar los detalles con la opción -v:
![[Pasted image 20230112214723.png]]
Por último, si quiero seleccionar un controlador determinado, puedo usar también la opción -s:
![[Pasted image 20230112215030.png]]
**Lsusb ->** Muestra información sobre los buses y dispositivos usb conectados. Tenemos la opción -t para mostrar información en árbol y velocidad del puerto USB:
![[Pasted image 20230112214355.png]]
Si ejecutamos este comando nos encontramos los buses USB de la máquina:
![[Pasted image 20230112215118.png]]
Y con la opción -v puedo obtener más información; y al igual que antes, con -s también puedo obtener la información sólo de uno de ellos:
![[Pasted image 20230112215345.png]]
Y por último con la opción -t obtengo más información e incluso la capacidad de almacenamiento:
![[Pasted image 20230112215455.png]]

