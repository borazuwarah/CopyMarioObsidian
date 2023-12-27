Esta librería sirve para cambiar el formato de vídeos con python:
```python
import argparse
from moviepy.editor import VideoFileClip

parser = argparse.ArgumentParser(description="Convierte un video a formato MP4 de manera rápida.")
parser.add_argument("input", help="Nombre del archivo de entrada")
parser.add_argument("output", help="Nombre del archivo de salida")

args = parser.parse_args()

archivo_entrada = args.input
archivo_salida = args.output

try:
    video_clip = VideoFileClip(archivo_entrada)
    video_clip.write_videofile(archivo_salida, codec='libx264', preset='ultrafast', bitrate='10000k')
    print("Conversión completada con éxito.")
except KeyboardInterrupt:
    print("Pulsado control +C, saliendo...")
except Exception as e:
    print(f"Error al convertir el video: {e}")
finally:
    print("Script finalizado")
```
También podemos establecer una sentencia condicional para elegir en qué formato de vídeo queremos convertirlo, si en avi o en mp4:
```python
import argparse
from moviepy.editor import VideoFileClip
import os

# Mapeo de extensiones a codecs
extension_to_codec = {
    "mp4": "libx264",
    "avi": "mpeg4",
}

decision = input("En qué formato quieres convertir tu vídeo (mp4, avi): ")

if decision not in extension_to_codec:
    print("Formato no válido. Debes elegir entre 'mp4' o 'avi'.")
else:
    parser = argparse.ArgumentParser(description="Convierte un video a formato específico de manera rápida.")
    parser.add_argument("input", help="Nombre del archivo de entrada")

    args = parser.parse_args()
    archivo_entrada = args.input

    # Obtener el nombre del archivo sin la extensión
    nombre_base = os.path.splitext(archivo_entrada)[0]

    try:
        codec = extension_to_codec[decision]
        archivo_salida = f"{nombre_base}.{decision}"
        video_clip = VideoFileClip(archivo_entrada)
        video_clip.write_videofile(archivo_salida, codec=codec, preset='ultrafast', bitrate='10000k')
        print(f"Conversión completada con éxito, el vídeo resultante es {archivo_salida}")
    except KeyboardInterrupt:
        print("Pulsado control + C, saliendo...")
    except Exception as e:
        print(f"Error al convertir el video: {e}")
    finally:
        print("Script finalizado")
```
![[Pasted image 20230928133712.png]]
![[Pasted image 20230928133719.png]]
