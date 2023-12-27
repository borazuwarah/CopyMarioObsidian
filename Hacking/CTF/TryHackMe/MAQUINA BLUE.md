Hacemos los reconocimientos con nmap y nos encuentra todos estos puertos:
![[Pasted image 20230427114901.png]]
Y con [[Crackmapexec]] podemos ver que estamos ante un windows 7 service pack 1, lo que lo hace muy vulnerable:
![[Pasted image 20230427114952.png]]
Podemos mirar si esta máquina es vulnerable a eternalblue con algún script de nmap y vemos que sí:
![[Pasted image 20230427115145.png]]
Por tanto vamos a usar metasploit para ganar acceso al sistema explotando esta vulnerabilidad utilizando el siguiente exploit:
![[Pasted image 20230427120812.png]]
Realizamos las configuraciones básicas:
![[Pasted image 20230427120834.png]]
Configuramos el payload correspondiente, ya que el que viene por defecto da error:
![[Pasted image 20230427120914.png]]
Y si ejecutamos el comando run ya estaríamos dentro:
![[Pasted image 20230427120933.png]]
Y ahora lo que tenemos que hacer es convertir una shell normal a una de metasploit, por lo que tenemos que enviar esta sesión al segundo plano dentro de metasploit pulsando CTRL y Z:
![[Pasted image 20230427121400.png]]
Y ahora lo seleccionamos:
![[Pasted image 20230427121644.png]]
Y ahora vamos a ver las opciones que tenemos que configurar, donde se nos dice que tenemos que poner el número de la sesión de antes que teníamos activa en segundo plano, por lo que la volvemos a listar con sessions -l y la incluimos:
![[Pasted image 20230427121826.png]]
Lo ejecutamos con el comando run pero veremos que se queda atascado, por lo que damos a enter y ya podemos poner el comando session -l para ver que se nos ha creado una nueva sesión de meterpreter:
![[Pasted image 20230427122824.png]]
Y con el comando sessions -i 2 podemos seleccionar la sessión de meterpreter que es la que tiene el número 2:
![[Pasted image 20230427122911.png]]
Y desde esta sesión de meterpreter podemos listar los procesos que corren en el sistema:
![[Pasted image 20230427123224.png]]

