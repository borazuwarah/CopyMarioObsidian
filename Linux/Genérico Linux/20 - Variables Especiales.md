En Linux, las variables especiales como `$?`, `$#`, y `$@` son utilizadas en scripts de shell para obtener información sobre el resultado de comandos.

**$? (Variable de Estado):**
Esta variable almacena el estado de salida del último comando ejecutado. Un valor de 0 generalmente indica que el comando se ejecutó correctamente, mientras que un valor diferente de 0 indica un error o fallo en la ejecución del comando:
![[Pasted image 20231119175849.png]]
Y por ejemplo, si ejecutamos un comando correctamente, mostrará la salida 0:
![[Pasted image 20231119175917.png]]
**$# (Número de Argumentos):**
Representa la cantidad de argumentos que se pasaron al script. Es útil para determinar cuántos argumentos se proporcionaron al script. Por ejemplo, dentro de un script añadimos la variable:
![[Pasted image 20231119180135.png]]
Y ahora si le pasamos dos argumentos al script, veremos cómo nos muestra el número 2:
![[Pasted image 20231119180159.png]]
**$@ (Todos los Argumentos):**
Esta variable representa todos los argumentos proporcionados al script. Cada argumento es tratado como una palabra separada.
![[Pasted image 20231119180302.png]]
Y nos imprimirá los argumentos proporcionados:
![[Pasted image 20231119180321.png]]

