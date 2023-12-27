Lo primero que podemos hacer es borrar todas las reglas del firewall para aquellos casos donde hayamos cometido errores o queramos empezar desde cero:
![[Pasted image 20230116081705.png]]
## DENEGAR O ACEPTAR EL TRÁFICO
Podemos denegar todo el tráfico y luego ir añadiendo reglas que permitan el tráfico que queramos, o bien podemos permitir todo el tráfico y luego añadir reglas que rechacen el que queramos. Vamos a elegir alguna de estas dos políticas con el comando iptables -P, por ejemplo que empiece rechazando todo el tráfico:
![[Pasted image 20230116082027.png]]
Ahora podemos listar todas estas reglas del firewall con este comando:
![[Pasted image 20230116082121.png]]
Y si navegamos vemos que no tenemos internet porque el firewall lo está bloqueando:
![[Pasted image 20230116082211.png]]
Y ahora si queremos permitir el tráfico del firewall, ejecutamos estos otros comandos:
![[Pasted image 20230116082318.png]]
Y ya tendremos otra vez conexión a internet y podremos navegar:
![[Pasted image 20230116082359.png]]
## SCRIPT PARA ELIMINAR TRÁFICO E IR PERMITIENDO UNO A UNO
Creamos un script que en principio se encarga de denegar todo el tráfico y luego en dicho script se van contemplando aquel tráfico que queramos permitir:
![[Pasted image 20230116082955.png]]
Si ejecutamos este script volvemos a no tener tráfico:
![[Pasted image 20230116083045.png]]
Sin embargo, podemos crear otro script que haga lo mismo pero que por defecto permita todo el tráfico:
![[Pasted image 20230116083219.png]]
Y volvemos a tener conectividad:
![[Pasted image 20230116083325.png]]
Ahora tenemos 2 scripts, uno para permitir el tráfico y otro para denegarlo:
![[Pasted image 20230116083412.png]]
## IR AÑADIENDO REGLAS PARA PERMITIR CIERTO TRÁFICO (desde script restrictivo que limita todo el tráfico)
Tenemos activado el script que deniega todo el tráfico, y ahora podemos ir permitiendo regla a regla; por ejemplo vamos a permitir que haya tráfico ICMP tanto de salida como de entrada con estos comandos:
![[Pasted image 20230116084717.png]]
![[Pasted image 20230116084738.png]]
## AÑADIENDO REGLAS AL SCRIPT
Ahora desde un script restrictivo, podemos ir permitiendo tráfico con las reglas, por ejemplo vamos añadir al script que nos permita el tráfico ICMP para hacer ping:
![[Pasted image 20230116085135.png]]
Lo que pasa con esto es que sólo hemos permitido el tráfico de ping, pero nos falta el tráfico DNS, ya que ahora mismo si hacemos un ping a google no podrá resolver:
![[Pasted image 20230116085335.png]]
Para permitir el tráfico DNS en iptables lo hacemos con este comando, donde además también habilitamos la interfaz loopback:
![[Pasted image 20230117163755.png]]
Y ahora si hacemos un ping vemos que ya lo realiza correctamente:
![[Pasted image 20230117163938.png]]
## REGULAR TRÁFICO HTTP Y HTTPS NAVEGADOR WEB
Ahora en este punto no tenemos la capacidad de navegar por la web, ya que el firewall lo está bloqueando, por lo que tendremos que añadir nuevas reglas para regularlo, por ejemplo vamos a habilitar el tráfico por el puerto 80:
![[Pasted image 20230117165314.png]]
Y ahora si hacemos un curl a una web http vemos que funciona:
![[Pasted image 20230117165410.png]]
Pero el problema está en que tenemos que habilitar el tráfico https y este se mueve por el puerto 443; por lo que para ello deberemos habilitar el tráfico también por este puerto:
![[Pasted image 20230117165550.png]]
Y ahora ya si podremos hacer un curl a un https://google.es o acceder a él a través del navegador:
![[Pasted image 20230117165657.png]]
## BLOQUEAR DIRECCIÓN IP EN FIREWALL IPTABLES
Para bloquear una dirección IP, tendremos que hacerlo así, y ahora si intentamos hacer un ping no vamos a poder:
![[Pasted image 20230119110858.png]]
Aunque también podemos limitar sólo el tráfico por ICMP de esta IP:
![[Pasted image 20230119111132.png]]
## BLOQUEAR TRÁFICO DE UN PUERTO CON IPTABLES
Por ejemplo vamos a limitar todo el tráfico por el puerto 22, que es el ssh:
![[Pasted image 20230121012227.png]]
Y si intentamos acceder por ssh desde otro equipo no podremos:
![[Pasted image 20230121012249.png]]
Y podríamos habilitar el tráfico por ssh sólo a una determinada dirección IP; y para ello lo haríamos de esta forma:
![[Pasted image 20230121012405.png]]
Y si accedemos desde esta dirección IP, sí podremos, ya que será la única IP que podrá acceder:

