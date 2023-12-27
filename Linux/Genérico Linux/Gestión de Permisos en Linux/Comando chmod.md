Sirve para gestionar los permisos en un sistema Linux; y en esta parte tenemos los permisos:
![[Pasted image 20231119105827.png]]
Vamos quitar el permiso de lectura al archivo hola:
![[Pasted image 20231119105914.png]]
Ahora vamos a quitar el permiso de escritura al archivo hola con chmod -w:
![[Pasted image 20231119110120.png]]
Y si intentamos escribir, vemos que no podremos:
![[Pasted image 20231119110137.png]]
## GESTIÓN DE PERMISOS CON NÚMEROS EN LINUX
Los permisos se pueden representar mediante números octales, donde:

- 4 representa todos los permisos de lectura (r).
- 2 representa todos los permisos de escritura (w).
- 1 representa todos los permisos de ejecución (x).

Por lo tanto, la combinación de estos números octales representa los permisos de cada conjunto (propietario, grupo y otros). Aquí hay algunos ejemplos:

- **4** (lectura): 100 en binario.
- **2** (escritura): 010 en binario.
- **1** (ejecución): 001 en binario.

Estos números octales se suman para representar los permisos. Por ejemplo:

- **rwx** (lectura, escritura, ejecución): 4+2+1 = 7 en octal.
- **rw-** (lectura, escritura, sin ejecución): 4+2+0 = 6 en octal.
- **r--** (lectura, sin escritura, sin ejecución): 4+0+0 = 4 en octal.

Entonces, cuando utilizas `chmod` con números, especificas los permisos para el propietario, el grupo y otros en ese orden. Por ejemplo:
```bash
chmod 755 archivo
```
- `Propietario` -> 1º dígito. Esto daría permisos completos al propietario (lectura, escritura y ejecución, con el número 7).
- `Grupos` -> 2º dígito. El grupo tiene permisos de lectura y ejecución, pero no de escritura, debido a la suma de Lectura (4) + Ejecución (1) = 5 en octal.
- `Otros` -> 3º dígito. Los otros tienen lo mismo, permisos de lectura y ejecución, pero no de escritura, debido a la suma de Lectura (4) + Ejecución (1) = 5 en octal.
### EJEMPLOS
**Otorgar permisos completos al propietario y sólo de lectura a grupos y otros**
```bash
chmod 744 nombre_carpeta
```
Explicación de los permisos:

- **Propietario (dueño):** Lectura (4) + Escritura (2) + Ejecución (1) = 7
- **Grupo:** Lectura (4)
- **Otros:** Lectura (4)

Esto significa que el propietario tiene permisos completos (lectura, escritura y ejecución), mientras que el grupo y otros tienen permisos solo de lectura. Puedes adaptar el comando según tus necesidades específicas.
## GESTIÓN DE PERMISOS CON LETRAS EN LINUX
Así es cómo funciona la gestión de permisos con el comando `chmod` en Linux usando letras en lugar de números:

- **u**: Usuario (propietario del archivo)
- **g**: Grupo
- **o**: Otros (resto de usuarios)
- **a**: Todos (equivalente a ugo)

Luego, las letras siguientes indican la acción a realizar:

- **+**: Agregar permisos
- ****: Quitar permisos
- **=**: Establecer permisos exactos

Y, finalmente, las letras que representan los permisos:

- **r**: Lectura (read)
- **w**: Escritura (write)
- **x**: Ejecución (execute)
#### EJEMPLOS:

Por ejemplo, con la u damos permisos al usuario propietario y luego establecemos permiso de escritura, lectura o ejecución:
![[Pasted image 20231215104856.png]]
Ahora por ejemplo, vamos a dar permiso de ejecución al grupo:
![[Pasted image 20231215104926.png]]
Y ahora, de escritura a los otros:
![[Pasted image 20231215104948.png]]


-----------------


Además, la letra 's' se utiliza junto con 'u', 'g' o 'o' y con 'x' para establecer atributos especiales:

- **s**:
    - Con 'u' y 'x', establece el bit SUID, permitiendo que el programa se ejecute con los privilegios del propietario del archivo.
    - Con 'g' y 'x', establece el bit SGID, lo que significa que el programa se ejecutará con los privilegios del grupo del archivo.
    - Con 'o' y 'x', establece el sticky bit, principalmente utilizado en directorios para asegurar que solo el propietario del archivo pueda eliminar o renombrar sus archivos.

Por ejemplo, `chmod u+rw archivo` dará permisos de lectura y escritura al usuario, y `chmod u+s programa` establecerá el bit SUID en el programa.

Es decir, si por ejemplo ejecutamos el comando sudo chmod u+s /bin/bash establece el bit de SUID (Set User ID) en un archivo ejecutable. Cuando se establece el bit SUID en un archivo ejecutable, dicho archivo se ejecuta con los privilegios del propietario del archivo en lugar de los del usuario que lo ejecuta.

![[Pasted image 20231215105220.png]]