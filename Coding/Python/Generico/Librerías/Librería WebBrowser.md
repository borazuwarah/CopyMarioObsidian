Por ejemplo, para abrir una página web con Python lo haríamos con este código:
```python
import webbrowser

webbrowser.open("http://www.python.org")
```

Y se abre la página de Python:

![[Pasted image 20230108135023.png]]

Ahora vamos a automatizar por ejemplo rellenar un formulario web con Python, por ejemplo que se abra la web de have i been pwned y vaya rellenando el campo de texto con los correos que se encuentren dentro de un documento txt:
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

![[Pasted image 20230108141856.png]]
