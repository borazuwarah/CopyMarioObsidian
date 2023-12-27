Sirve para registrar las pulsaciones de teclas de mi teclado:
```python
import keyboard

def on_key_event(e):
    print(f'Tecla presionada: {e.name}') # Imprimimos la tecla presionada

# Registrar la función de devolución de llamada
keyboard.on_press(on_key_event)

# Mantener el programa en ejecución
keyboard.wait('esc')
```
![[Pasted image 20231216103220.png]]
Esto se puede utilizar para recoger palabras enteras, por ejemplo de la siguiente forma:
```python
import keyboard

current_word = ""

def on_key_event(e):
    global current_word

    if e.event_type == keyboard.KEY_DOWN:
        if e.name == 'space':
            print(f'Palabra ingresada: {current_word}')
            current_word = ""
        else:
            current_word += e.name

# Registrar la función de devolución de llamada
keyboard.hook(on_key_event)

# Mantener el programa en ejecución
keyboard.wait('esc')
```
