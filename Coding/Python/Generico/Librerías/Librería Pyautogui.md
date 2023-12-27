Para controlar acciones del ordenador, por ejemplo para copiar y pegar cosas, donde vamos a automatizar la comprobación de muchos correos dentro de la web de haveibeenpwned:
```python
import webbrowser
import time
import pyperclip
import pyautogui
 

documento = open('escribir_correos.txt','r')
documento = documento.read().split('\n')

# Ahora aquí ordenamos que se escriba con el teclado:

for email in documento:

    webbrowser.open_new("https://haveibeenpwned.com/")
    time.sleep(3)
    pyperclip.copy(email) # Copiamos en portapapeles los email.
    pyautogui.hotkey('ctrl', 'v', interval = 0.15) # Los pegamos en la web.
    pyautogui.press("enter")
```
Y se abre una por una en pestañas distintas:
![[Pasted image 20230108141815.png]]
