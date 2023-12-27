Si usamos Windows, debemos instalar git bash para que sea compatible:
![[Pasted image 20230108121910.png]]
Durante la instalación, se nos mostrará lo que se va a instalar:
![[Pasted image 20230108121919.png]]
Y ahora tenemos una terminal de Linux con git en su interior:
![[Pasted image 20230108121927.png]]
# Configuración Inicial

Primero debemos configurar nuestro nombre y correo electrónico:
![[Pasted image 20230108122315.png]]
![[Pasted image 20230108122318.png]]
Ahora hay que vincular Visual Studio Code con git, indicando que será nuestro editor de código por defecto:
![[Pasted image 20230108122327.png]]
Ahora para comprobar que se haya hecho todo correctamente y para revisar nuestro archivo de configuración el visual studio code, ejecutamos este comando:
![[Pasted image 20230108122336.png]]
Y ahora se nos abre el visual studio code automáticamente mostrando el archivo de configuración:
![[Pasted image 20230108122346.png]]
Y ahora introducimos un último comando, donde será true si usamos windows, o será input si usamos mac o linux:
![[Pasted image 20230108122355.png]]
Voy a crear una carpeta donde trabajaré con los comandos de git:
![[Pasted image 20230108122402.png]]
Ahora dentro de esta carpeta vamos a inicializar un repositorio, lo cual sería el primer paso:
![[Pasted image 20230108122409.png]]
Debemos tener en cuenta que dentro de este directorio se creó un directorio oculto que se llama .git, si queremos verlo hacemos lo siguiente:
![[Pasted image 20230108122418.png]]
Dentro de este directorio, si miramos su contenido podremos ver diferentes archivos y directorios, aquí será donde se almacene la configuración de nuestros programas y lo que vayamos haciendo:
![[Pasted image 20230108122434.png]]
Cuando estemos creando un programa y vayamos a subirlo a github, va a haber un proceso que será el siguiente:
![[Pasted image 20230108122441.png]]
