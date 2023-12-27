Esta librería sirve para aplicar colores al texto de nuestro código Python, donde podemos definir primero los colores dentro de variables y después utilizarlos cuando queramos de esta forma:
```python
from colorama import Fore, Style

red = Fore.RED
green = Fore.GREEN
blue = Fore.BLUE
yellow = Fore.YELLOW
purple = Fore.MAGENTA
reset = Style.RESET_ALL


colors = {'red': red, 'green': green, 'blue': blue, 'yellow': yellow, 'purple': purple, 'reset': reset}


print(colors['red'] + "This text is red." + colors['reset'])
print(colors['green'] + "This text is green." + colors['reset'])
print(colors['blue'] + "This text is blue." + colors['reset'])
print(colors['yellow'] + "This text is yellow." + colors['reset'])
print(colors['purple'] + "This text is purple." + colors['reset'])
```
Y el resultado se vería así:
![[Pasted image 20230201140245.png]]