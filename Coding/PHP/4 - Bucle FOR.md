Un bucle se haría de esta fortma:
```php
<?php
for ($i = 0; $i < 5; $i++) {
    echo "Iteración $i <br>";
}
?>
```
![[Pasted image 20231216164435.png]]
### OTRO EJEMPLO
```php
<?php
for ($i = 1; $i <= 10; $i++) {
    echo $i . "<br>";
}
?>
```
En este ejemplo:

- La variable `$i` se inicializa en 1.
- La condición es `$i` debe ser menor o igual a 10.
- Después de cada iteración, incrementamos el valor de `$i` en 1 usando `$i++`.
![[Pasted image 20231216164546.png]]
