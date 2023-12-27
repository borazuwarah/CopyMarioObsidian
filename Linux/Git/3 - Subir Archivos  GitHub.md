PASO 1:
```bash
git init
```
PASO 2:
```bash
git add archivo1 archivo2
```
PASO 3:
```bash
git commit -m 'Subiendo archivos a github o gitlab'
```
PASO 4:
```bash
git remote add origin url.git
```
PASO 5:
```bash
git push -u origin master
```
Aquí nos pedirá credenciales para iniciar sesión en github; y luego desde la web tenemos que hacer un merge para pasar estos cambios a la rama main.

-------------------------------------

PASO 6:
Si tenemos errores, hacemos un git reset y volvemos a probar:
```bash
git reset
```
