Para incluir imágenes en php, lo haríamos de la siguiente forma:
```php
<?php
$nombre = "Mario";
$imagen_url = "pinguino.jpg";  // Reemplaza con la ruta real de tu imagen

echo "<h1>Hola, $nombre!</h1>";
echo "<img src='$imagen_url' alt='Imagen de $nombre'>";
?>
```
Dejamos la imagen en el mismo directorio:
![[Pasted image 20231216164047.png]]
Y la visualizamos en el navegador:
![[Pasted image 20231216164101.png]]
