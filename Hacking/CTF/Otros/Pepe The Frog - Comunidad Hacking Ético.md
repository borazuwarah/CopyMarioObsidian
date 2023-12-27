Encontramos la máquina con arp-scan:
![[Pasted image 20230725094536.png]]
La máquina nos pedirá unas credenciales para bootearla, las cuales son las siguientes:
```bash
pepefrog:Ciudadanokane1
```
Hacemos el reconocimiento con nmap y estos son los resultados:
![[Pasted image 20230725094616.png]]
Esta es la página web:
![[Pasted image 20230725094637.png]]
Si vemos el código fuente, vemos que nos dice algo de salvoconducto:
![[Pasted image 20230725094657.png]]
Por otra parte, si hacemos fuzzing vemos que existe el directorios /img, donde podemos ver que tenemos acceso a 2 imágenes:
![[Pasted image 20230725094740.png]]
![[Pasted image 20230725094750.png]]
Nos las vamos a descargar para comprobar mediante esteganografía su contenido, por tanto decodificamos el texto que estaba en base64 y vemos el salvoconducto:
![[Pasted image 20230725095021.png]]
![[Pasted image 20230725095042.png]]
```bash
pepethefrog:4d3l4nt3p3p3
```
Probamos en entrar vía ssh y ya estamos dentro:
![[Pasted image 20230725095138.png]]
Si ejecutamos el comando ls -la vemos un archivo oculto llamado .bash_none extraño, y ahí tenemos la flag de root:
![[Pasted image 20230725101333.png]]



