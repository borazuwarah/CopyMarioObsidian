Hacemos el reconocimiento con nmap:
![[Pasted image 20230907100155.png]]
Y este es el puerto 80:
![[Pasted image 20230907100237.png]]
Pero accedemos también al puerto 8765 para ver qué hay:
![[Pasted image 20230907100349.png]]
Vamos a hacer fuzzing dentro de esta ruta:
![[Pasted image 20230907100634.png]]
Y dentro del directorio custom encontramos el directorio js, que a su vez tiene estos archivos:
![[Pasted image 20230907100730.png]]
Guardamos este users.bak:
![[Pasted image 20230907100810.png]]
Y si leemos el contenido de este archivo vemos unas credenciales:
![[Pasted image 20230907100922.png]]
Pero la contraseña de este usuario admin viene hasheada, por lo que vamos a crackearla con john the ripper:
![[Pasted image 20230907101419.png]]
![[Pasted image 20230907101448.png]]
Estas son las credenciales:
```bash
admin:bulldog19
```
Y con esto ya entramos dentro de la web:
![[Pasted image 20230907101532.png]]
Si miramos el código fuente de la página podemos ver que nos dan una pista de una clave SSH para el usuario Barry:
![[Pasted image 20230907101829.png]]
Teniendo en cuenta la existencia de un archivo dontforget.bak, probamos en descargarlo:
![[Pasted image 20230907101946.png]]
Y vemos que dentro de este archivo hay una referencia a xml, lo que implica que quizá estemos ante un sml external entity:
![[Pasted image 20230907102129.png]]
Y ahora vamos a la web donde podemos insertar el XXE e interceptamos la petición con burp suite:
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE data [
   <!ELEMENT data ANY >
   <!ENTITY name SYSTEM "file:///etc/passwd" >]>
<comment>
  <name>&name;</name>
  <author>Pavandeep</author>
  <com>Hacking Articles</com>
</comment>
```
![[Pasted image 20230907103629.png]]
![[Pasted image 20230907103617.png]]
![[Pasted image 20230907103549.png]]
Vemos un usuario llamado Barry, donde quizá podamos obtener su id_rsa:
![[Pasted image 20230907103702.png]]
Por lo que lanzamos de nuevo el payload para apuntar contra el id_rsa:
![[Pasted image 20230907103742.png]]
Nos guardamos este id_rsa para crackearlo con john the ripper:
![[Pasted image 20230907104045.png]]
Y obtenemos una contraseña:
![[Pasted image 20230907104230.png]]
Le ponemos un nombre correcto al id_rsa:
![[Pasted image 20230907104303.png]]
Y accedemos como el usuario barry y pasándole el id_rsa:
![[Pasted image 20230907104430.png]]
Si enumeramos los binarios vemos algo raro:
![[Pasted image 20230907104701.png]]
Vemos que se trata de un binario que monitoriza el navegador firefox:
![[Pasted image 20230907104735.png]]
Si hacemos un strings de este archivo, vemos que ejecuta el comando tail sin hacerlo desde la raiz, por lo que podemos hacer un path hijacking:
![[Pasted image 20230907104856.png]]
Por tanto guardamos dentro de archivo tail el comando de /bin/bash y lo exportamos al path del sistema, de tal forma que al ejecutar otra vez el binario, ganaremos acceso como root:
![[Pasted image 20230907105029.png]]
