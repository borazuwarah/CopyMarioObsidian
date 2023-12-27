Hacemos el reconocimiento de puertos con nmap, donde vemos abiertos los puertos 22 y 80:
![[Pasted image 20230920134415.png]]
Si inspeccionamos el servidor web, vemos que nos encontramos con una plantilla de apache por defecto:
![[Pasted image 20230917210336.png]]
Realizamos fuzzing para detectar directorios o archivos dentro de la web; y si utilizamos la extensión -x de gobuster podemos encontrar un archivo llamado penguin.html:
![[Pasted image 20230920140248.png]]
Accedemos al directorio penguin.html y tenemos esta página web:
![[Pasted image 20230920152410.png]]
La imagen que vemos en la web vamos a descargarla para examinarla más en profundidad:
![[Pasted image 20230920140323.png]]
![[Pasted image 20230920140452.png]]
Si intentamos comprobar la existencia de archivos ocultos dentro de esta imagen, vemos que nos pide un salvo conducto, el cual no tenemos:
![[Pasted image 20230920152551.png]]
Por lo que tenemos que hacer un ataque de fuerza bruta para tratar de descubrir cual es esta contraseña y así poder ver los archivos dentro de la imagen, por lo que haremos el siguiente script de bash:
```bash
#!/bin/bash

imagen="$1"
diccionario="$2"

while IFS= read -r password; do
    echo "Probando contraseña: $password"
    steghide extract -sf "$imagen" -p "$password" &>/dev/null
    if [ $? -eq 0 ]; then
        echo "Extracción exitosa con contraseña: $password"
        exit 0
    fi
done < "$diccionario"

echo "No se pudo extraer el archivo oculto. Se probaron todas las contraseñas."
exit 1
```
Ejecutamos el script y le pasamos como argumentos tanto la imagen como el diccionario rockyou, donde vemos que nos encuentra la contraseña chocolate:
![[Pasted image 20230917210321.png]]
Extraemos el contenido de la imagen con steghide proporcionando la contraseña:
![[Pasted image 20230920152633.png]]
Y vemos que nos devuelve un keepass, por lo que tenemos que hacerle otro ataque de fuerza bruta con keepass2john de la siguiente forma, donde nos encuentra la contraseña password1:
![[Pasted image 20230917210430.png]]
Entramos en la base de datos del keepass proporcionando esta contraseña y vemos que funciona:
![[Pasted image 20230917210444.png]]
El nombre de usuario no nos coincide por ssh, pero podemos probar el mismo nombre de la máquina hackpenguin; y en este caso vemos que funciona:
![[Pasted image 20230917210521.png]]
Una vez dentro, además de la flag de user podemos ver un script y un archivo de texto:
![[Pasted image 20230920152808.png]]
Vemos que root es el propietario de este script y además tenemos permisos sobre él:
![[Pasted image 20230920152958.png]]
Revisamos si este script se ejecuta de forma recurrente con pspy:
![[Pasted image 20230920153854.png]]
![[Pasted image 20230920153923.png]]
Y vemos que el script se ejecuta cada cierto tiempo:
![[Pasted image 20230920154300.png]]
![[Pasted image 20230920154131.png]]
Editamos el script y añadimos un comando para que nos cambie permisos a la bash:
![[Pasted image 20230920153256.png]]
Lanzamos una bash con privilegios y vemos que se han cambiado los permisos a la bash:
![[Pasted image 20230920153450.png]]


