```python
import os
import shutil

# Definir las carpetas de destino
documentos_dir = 'documentos'
fotos_dir = 'fotos'
videos_dir = 'videos'
musica_dir = 'musica'

# Crear las carpetas de destino si no existen
if not os.path.exists(documentos_dir):
    os.makedirs(documentos_dir)
if not os.path.exists(fotos_dir):
    os.makedirs(fotos_dir)
if not os.path.exists(videos_dir):
    os.makedirs(videos_dir)
if not os.path.exists(musica_dir):
    os.makedirs(musica_dir)

# Definir las extensiones para cada tipo de archivo
extensiones_documentos = ('.pdf', '.doc', '.docx', '.txt', '.xls', '.xlsx', '.ppt', '.pptx')
extensiones_fotos = ('.jpg', '.jpeg', '.png', '.gif', '.bmp', '.tiff')
extensiones_videos = ('.mp4', '.mov', '.avi', '.mkv', '.flv', '.wmv')
extensiones_musica = ('.mp3', '.wav', '.aac', '.flac', '.ogg')

# Obtener la lista de archivos en el directorio actual
archivos = []
for f in os.listdir():
    archivos.append(f)

# Función para mover archivos
def mover_archivo(archivo, destino):
    shutil.move(archivo, destino)

# Organizar los archivos en las carpetas correspondientes
for archivo in archivos:
    if archivo.endswith(extensiones_documentos):
        mover_archivo(archivo, documentos_dir)
    elif archivo.endswith(extensiones_fotos):
        mover_archivo(archivo, fotos_dir)
    elif archivo.endswith(extensiones_videos):
        mover_archivo(archivo, videos_dir)
    elif archivo.endswith(extensiones_musica):
        mover_archivo(archivo, musica_dir)

print("Organización de archivos completada.")
```
