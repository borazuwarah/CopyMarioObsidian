## CREAR NUESTRA PROPIA IMAGEN DE DOCKER

Nos ubicamos en la carpeta donde vayamos a trabajar con la imagen, por ejemplo voy a crear un directorio llamado docker-images.

El siguiente paso será crear un dockerfile, lo vamos a crear con el editor nano, poniendo sudo nano Dockerfile:

![[Pasted image 20221217105641.png]]

Ya tengo creada mi imagen de docker donde pongo la imagen del sistema operativo donde va a correr esta imagen, que será ubuntu, y luego pongo la instalación del programa, que en este caso será python:

![[Pasted image 20221217105647.png]]

Y ahora tenemos una imagen de ubuntu pero con python3 instalado:

![[Pasted image 20221217105653.png]]

Si utilizo el comando docker history -H python-ubuntu, puedo ver todas las capas que tiene esta imagen de docker, básicamente paso a paso de cómo fue creada:

![[Pasted image 20221217105700.png]]

# **UTILIZAR EL DOCKERFILE A FONDO**

Partes del fockerfile:

FROM -> Aquí ponemos la imagen del sistema operativo donde va a correr nuestro contenedor.

RUN -> Los comandos que se van a ejecutar en el sistema operativo del from.

Partes del dockerfile: copy/add, env, workdir, expose, label, user, volume, cmd.

## **DOCKERFILE - FROM, RUN, COPY**

COPY -> Sirve para copiar archivos desde nuestra máquina al contenedor. Por ejemplo, voy a pasar un documento de texto de mi máquina anfitrión al contenedor de ubuntu con python que creamos antes:

![[Pasted image 20221217105718.png]]

Ponemos copy y el nombre del documento (siempre y cuando esté en el mismo directorio que el Dockerfile, y después ponemos el directorio del contenedor donde lo queramos pegar.

A continuación volvemos a construir la imagen y se añade el documento de texto a dicho directorio:

![[Pasted image 20221217105725.png]]

Y ahora como se puede ver, ya tenemos el documento prueba.txt dentro del directorio de nuestro contenedor:

![[Pasted image 20221217105732.png]]

## **DOCKERFILE - ENV, WORKDIR, EXPOSE**

**ENV →** Podemos crear una variable y enviarla a nuestro contenedor, en mi caso por ejemplo voy a definir una variable llamada contenido que va a ir dirigida a un bloc de notas que está dentro del contenedor:
![[Pasted image 20221217105742.png]]

Luego con el comando RUN puedo enviar el contenido de la variable $contenido al documento prueba.txt que está dentro de home; y en su interior estará escrita la palabra prueba:

![[Pasted image 20221217105749.png]]
**WORKDIR →**Es una forma de situarnos en un determinado directorio por defecto.

De esta manera, nosotros podemos copiar un fichero directamente a la ruta que hemos indicado en el WORKDIR dentro de nuestro Dockerfile:


![[Pasted image 20221217105756.png]]

Después solo tenemos que poner COPY hola.txt con el puntito y ya se copia directamente.

![[Pasted image 20221217105804.png]]

**EXPOSE →**Sirve para exponer los puertos que queramos.

## **DOCKERFILE - LABEL, USER, VOLUME**

**LABEL** →Es una etiqueta, por ejemplo para indicar la versión del programa, la descripción o el nombre.

![[Pasted image 20221217105818.png]]

**USER** →Sirve para establecer el usuario, por defecto es root pero si utilizo esta directiva, se pondrá el usuario que yo quiera, aunque tiene que existir previamente, por eso lo creo.

Primero añado el usuario con RUN useradd Mario. Después utilizo chown Mario /home -R para darle los permisos a este usuario del directorio /home, que será donde envíe el archivo. 

Después se envían los dos bloc de notas donde se pondrá desde la variable quién es el usuario en cada momento.

![[Pasted image 20221217105825.png]]

![[Pasted image 20221217105828.png]]

**VOLUME →**Para que al eliminar el contenedor, los datos de una determinada ruta no se eliminen.

## **DOCKERFILE - CMD, DOCKERIGNORE**

**CMD →**Es lo que mantiene vivo al contenedor en primer plano, por ejemplo veamos la diferencia entre intentar entrar en un contenedor de ubuntu sin CMD y otro con CMD:// Debemos crear un script en nuestra máquina por ejemplo con un mensaje de bienvenida al contenedor, y luego copiarlo al contenedor con copy. Una vez hecho esto, con CMD podemos ejecutar ese script dentro del contenedor.

Primero, por ejemplo, envío un script que tiene un mensaje de bienvenida al directorio /home. Después, con CMD doy la orden para que se ejecute:

![[Pasted image 20221217105843.png]]

Ahora si monto la nueva imagen y arranco el contenedor, se va a ejecutar ese script gracias al CMD:

![[Pasted image 20221217105849.png]]

**IGNORE →** Sirve para ignorar todo aquello que tengamos en nuestro directorio actual y no queramos.  

Por ejemplo voy a crear una imagen donde se ordenará dentro del Dockerfile que se copien todos los archivos del actual directorio al contenedor:

![[Pasted image 20221217105856.png]]
A continuación creamos un archivo llamado .dockerignore donde debemos escribir el nombre de aquellos archivos que queramos que se ignoren:

![[Pasted image 20221217105903.png]]

Veremos que tengo varios archivos y uno de ellos se llama ignorar, el cual será ignorado:

![[Pasted image 20221217105910.png]]

Ahora vamos a crear la imagen y al ejecutar un contenedor veremos como se copiaron todos los archivos excepto el que indicamos que se ignore:

![[Pasted image 20221217105918.png]]
