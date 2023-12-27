## Gestión de Paquetes con YUM:

- **`yum install {package}`**: instala la última versión del paquete que se indique.
-   **`yum update {package}`**: actualiza paquetes en el sistema.
-   **`yum remove {package}`**: remueve un paquete instalado, pero deja los archivos de configuración.
-   **`yum clean packages`**: elimina paquetes instalados o actualizados.
-   **`yum clean headers`**: elimina los archivos encabezados usados por yum para resolver dependencias.
-   **`yum update`**: con este comando actualizamos todos los paquetes instalados en el sistema Linux, incluyendo los paquetes de seguridad.
-   **`yum update --security`**: actualiza solo los paquetes de seguridad.
-   **`yum list installed`**: lista todos los paquetes instalados en el sistema.
-   **`yum list`**: lista de los paquetes disponibles en el sistema.
-   **`yum search {package}`**: busca paquetes mediante su nombre.
-   **`yum grouplist`**: lista de visualización de los grupos del software.
-   **`yum check- update`**: arroja una lista de paquetes que requieren ser actualizados sin instalarlos.
-   **`yum info paquete`**: ofrece una descripción completa del paquete indicado.
-   **`yum repolist`**: muestra la lista de los repositorios que se tengan de yum
-   **`yum autoremove`**: similar al apt autoremove de Ubuntu.
-
## Gestión de Paquetes con DNF:

El gestor de paquetes DNF es la versión mejorada y actualizada de yum; y estos son sus usos más habituales:

`dnf upgrade`
`dnf distro-sync` 
`dnf system-upgrade`
`dnf remove`
`dnf autoremove`
`dnf search`
`dnf repoquery` - Es útil para búsquedas avanzadas.