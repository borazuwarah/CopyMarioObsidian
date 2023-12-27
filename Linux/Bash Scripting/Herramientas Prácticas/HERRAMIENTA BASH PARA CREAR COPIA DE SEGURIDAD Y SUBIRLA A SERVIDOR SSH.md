
```bash
#!/bin/bash

# Nombre del archivo ZIP
zip_file="escritorio.zip"

# Usuario y servidor SSH
ssh_user="mario"
ssh_server="192.168.0.44"

# Ruta en el servidor SSH donde se copiará el archivo ZIP
remote_path="/home/mario/Escritorio/"

# Comprimir el escritorio en un archivo ZIP
zip -r "$zip_file" .

# Copiar el archivo ZIP al servidor SSH
scp "$zip_file" "$ssh_user@$ssh_server:$remote_path"

# Verificar el estado de la operación de copia
if [ $? -eq 0 ]; then
  echo "El archivo ZIP se ha copiado correctamente al servidor SSH."
else
  echo "Ha ocurrido un error al copiar el archivo ZIP al servidor SSH."
fi
 
rm "$zip_file"
```