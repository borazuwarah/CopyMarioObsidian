Tenemos este reto donde nos dicen que escuchemos con netcat a una determinada dirección por el puerto 7449:
![[Pasted image 20230409213645.png]]
Por tanto nos podemos a la escucha de esta dirección e imprimimos la respuesta por pantalla, donde recibimos una serie de números:
![[Pasted image 20230409213726.png]]
Si consultamos en chatgpt como convertir una secuencia de números a ASCII podemos ver que nos genera un script de bash, donde solo tenemos que añadir los números obtenidos anteriormente a la variables numeros:
![[Pasted image 20230409213827.png]]
Creamos el script con dichas modificaciones:
![[Pasted image 20230409213857.png]]
Lo ejecutamos y ya tenemos la conversión hecha y la flag:
![[Pasted image 20230409213921.png]]
