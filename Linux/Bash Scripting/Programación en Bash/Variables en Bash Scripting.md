Una variable es un espacio de almacenamiento en la memoria del ordenador que se utiliza para almacenar un valor. Las variables se utilizan para almacenar información que puede cambiar durante la ejecución de un programa.

--------------------

El valor de una variable se puede asignar mediante el uso del operador de asignación (=). Por ejemplo, la siguiente declaración asigna el valor 10 a la variable `edad`:
```zsh
edad=10
```
Después, podemos imprimir el valor de una variable mediante el uso del operador de referencia `$`:
```zsh
echo $edad
```
![[Pasted image 20231123173928.png]]
Podemos imprimir el valor de una variable en cualquier frase o concepto:
![[Pasted image 20231123174050.png]]
#### VARIABLES NORMALES VS VARIABLES DE ENTORNO
La principal diferencia entre las variables de entorno y las variables normales es su alcance. Las variables de entorno están disponibles para todos los procesos que se ejecutan en el sistema, mientras que las variables normales solo están disponibles para el proceso en el que se definen.

**Variables de entorno**


**Variables normales**

Las variables normales son variables que solo están disponibles para el proceso en el que se definen. Se pueden utilizar para almacenar cualquier tipo de información, como el nombre de un archivo, el número de un contador o el resultado de un cálculo.

Las variables normales se definen mediante el uso de la siguiente sintaxis:

```
variable_name=valor
```

Por ejemplo, para definir una variable normal llamada `nombre` que contenga el nombre del usuario, podemos usar el siguiente comando:

```
nombre="usuario"
```

Las variables normales se pueden leer utilizando el comando `echo`:

```
echo $nombre
```

**Diferencias entre variables de entorno y variables normales**

La principal diferencia entre las variables de entorno y las variables normales es su alcance. Las variables de entorno están disponibles para todos los procesos que se ejecutan en el sistema, mientras que las variables normales solo están disponibles para el proceso en el que se definen.

Otra diferencia importante es que las variables de entorno se heredan por los procesos secundarios, mientras que las variables normales no. Esto significa que si un proceso define una variable de entorno, esa variable también estará disponible para todos los procesos secundarios que genere.
