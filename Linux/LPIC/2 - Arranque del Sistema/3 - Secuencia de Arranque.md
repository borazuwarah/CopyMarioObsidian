**PASOS CUANDO ENCENDEMOS EL PC**

1 - Cuando encendemos el PC se ejecuta el firmware (BIOS o UEFI).
2 - El POST se encarga de comprobar que el hardware está bien.
3 - Se busca un cargador de arranque por orden en las unidades que hayamos indicado en la secuencia de arranque del setup.
4 - Si es BIOS se lee la unidad MBR  y aquí se encuentra el código que busca el gestor de arranque.
5 - Si es UEFI, se ejecuta el gestor de arranque que se encuentra en una partición especial (ESP)


