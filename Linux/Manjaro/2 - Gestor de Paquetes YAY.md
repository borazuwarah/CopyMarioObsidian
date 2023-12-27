Yay es un ayudante de construcción de paquetes para Arch Linux que facilita la gestión de paquetes, incluyendo la instalación, actualización y eliminación de paquetes desde los repositorios oficiales de Arch y el Arch User Repository (AUR).

-----------------------------

Para instalar un paquete:
```bash
yay -S nombre_del_paquete
```
Para actualizar la base de datos de paquetes:
```bash
yay -Sy
```
Para actualizar todos los paquetes del sistema:
```bash
yay -Syu
```
Para eliminar un paquete:
```bash
yay -R nombre_del_paquete
```
Para buscar paquetes en los repositorios y AUR:
```bash
yay -Ss nombre_del_paquete
```
Limpiar la caché de los paquetes descargados:
```bash
yay -Sc
```
