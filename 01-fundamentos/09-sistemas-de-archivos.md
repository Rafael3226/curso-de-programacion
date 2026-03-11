# Sistemas de Archivos

## Que es un sistema de archivos?

Un disco (SSD o HDD) es simplemente una secuencia enorme de bytes. El **sistema de archivos** (file system) es la estructura que organiza esos bytes en archivos y carpetas, permitiendo nombrarlos, encontrarlos y gestionarlos.

Sin un sistema de archivos, un disco seria como un libro sin indice, sin capitulos y sin numeros de pagina.

## Conceptos basicos

### Archivo (File)

Una coleccion de datos almacenados con un nombre. Puede contener cualquier cosa: texto, imagen, programa, musica.

Un archivo tiene:
- **Nombre:** `documento.txt`, `foto.jpg`
- **Contenido:** los datos reales
- **Metadatos:** informacion sobre el archivo (tamano, fecha de creacion, permisos, etc.)

### Directorio / Carpeta (Directory)

Un contenedor que agrupa archivos y otros directorios. Forman una estructura de **arbol**:

```
/
├── home/
│   └── usuario/
│       ├── documentos/
│       │   ├── tesis.pdf
│       │   └── notas.txt
│       ├── fotos/
│       │   ├── vacaciones.jpg
│       │   └── perfil.png
│       └── .bashrc
├── etc/
│   └── config.conf
└── tmp/
    └── cache.dat
```

### Ruta (Path)

La direccion de un archivo en el arbol de directorios.

**Ruta absoluta:** desde la raiz del sistema.

```
Linux/Mac:   /home/usuario/documentos/tesis.pdf
Windows:     C:\Users\usuario\documentos\tesis.pdf
```

**Ruta relativa:** desde el directorio actual.

```
Si estoy en /home/usuario:
  documentos/tesis.pdf          (bajar a documentos)
  ../otro_usuario/archivo.txt   (subir un nivel, luego bajar)
  ./script.sh                   (en el directorio actual)
```

Simbolos especiales:
```
/    → separador de directorios (Linux/Mac) o raiz
\    → separador de directorios (Windows)
.    → directorio actual
..   → directorio padre (un nivel arriba)
~    → directorio home del usuario (Linux/Mac)
```

## Archivos de texto vs archivos binarios

### Archivos de texto

Contienen caracteres legibles codificados (UTF-8, ASCII, etc.). Puedes abrirlos con cualquier editor de texto.

```
Ejemplos: .txt, .csv, .html, .py, .js, .json, .xml, .md
```

Caracteristicas:
- Cada linea termina con un caracter de nueva linea (`\n` en Linux, `\r\n` en Windows)
- Son legibles por humanos
- Generalmente mas grandes que su equivalente binario

### Archivos binarios

Contienen datos en un formato que no es texto legible. Necesitas un programa especifico para interpretarlos.

```
Ejemplos: .jpg, .png, .mp3, .mp4, .exe, .zip, .pdf, .docx
```

Un archivo JPG abierto como texto se veria asi:
```
ÿØÿà JFIF    ÿÛ C $.' ",#(7),01444'9=82<.342ÿÛ C ...
(basura ilegible)
```

### Magic numbers

Muchos archivos binarios empiezan con una secuencia de bytes que identifica su formato:

```
Formato  │ Primeros bytes (hex)  │ Como se ve
─────────┼───────────────────────┼──────────────
PNG      │ 89 50 4E 47           │ .PNG
PDF      │ 25 50 44 46           │ %PDF
ZIP      │ 50 4B 03 04           │ PK..
JPEG     │ FF D8 FF              │ ÿØÿ
GIF      │ 47 49 46 38           │ GIF8
EXE/DLL  │ 4D 5A                 │ MZ
```

El sistema operativo puede usar estos bytes (no solo la extension) para saber que tipo de archivo es.

## Extensiones de archivo

La extension es la parte despues del ultimo punto en el nombre: `foto.jpg`, `script.py`.

Importante: **la extension es solo una convencion**. Renombrar `foto.jpg` a `foto.txt` no convierte la imagen en texto. El contenido no cambia, solo el nombre. El sistema operativo puede confundirse sobre que programa usar para abrirlo, pero los datos internos siguen siendo los mismos.

## Metadatos de archivos

Cada archivo almacena informacion adicional:

```
Metadato              │ Ejemplo
──────────────────────┼─────────────────────
Nombre                │ informe.pdf
Tamano                │ 2,456,789 bytes
Fecha de creacion     │ 2025-01-15 10:30
Fecha de modificacion │ 2025-03-08 14:22
Fecha de acceso       │ 2025-03-10 09:00
Permisos              │ rwxr-xr-x (Unix)
Propietario           │ usuario
Tipo                  │ archivo regular
```

## Permisos (Unix/Linux)

En sistemas Unix, cada archivo tiene permisos para tres categorias:

```
Categorias:  u (user/dueno)  g (group/grupo)  o (others/otros)
Permisos:    r (read/leer)   w (write/escribir)  x (execute/ejecutar)
```

```
rwxr-xr--
│││ │││ │││
│││ │││ └┴┴─ Otros:  r-- (solo lectura)
│││ └┴┴───── Grupo:  r-x (leer y ejecutar)
└┴┴──────── Dueno:  rwx (todo)
```

Representacion numerica (octal):
```
r = 4,  w = 2,  x = 1

rwx = 4+2+1 = 7
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4

rwxr-xr-- = 754
```

## Sistemas de archivos comunes

### FAT32

- **Uso:** memorias USB, tarjetas SD
- **Limite de archivo:** 4 GB maximo
- **Compatible:** con practicamente todo (Windows, Mac, Linux, camaras, consolas)
- **Sin permisos:** no soporta permisos de archivo

### NTFS (New Technology File System)

- **Uso:** discos internos de Windows
- **Sin limite practico de archivo:** hasta 16 TB
- **Soporta:** permisos, encriptacion, compresion, journaling
- **Journaling:** mantiene un registro de cambios pendientes para recuperarse de fallos

### ext4 (Fourth Extended Filesystem)

- **Uso:** estandar en Linux
- **Soporta:** permisos Unix, journaling
- **Tamano maximo de archivo:** 16 TB
- **Muy maduro:** estable y bien probado

### APFS (Apple File System)

- **Uso:** macOS, iOS
- **Optimizado para SSD:** snapshots, clonacion eficiente
- **Encriptacion nativa**

### exFAT

- **Uso:** memorias USB grandes, tarjetas SD
- **Sin limite de 4 GB:** soporta archivos grandes
- **Compatible:** con Windows, Mac y Linux moderno
- **Ideal para:** unidades externas que se comparten entre sistemas operativos

## Fragmentacion

### En HDD

Cuando se crean, modifican y eliminan archivos, los datos pueden quedar dispersos por el disco. Esto obliga al cabezal a moverse mucho, haciendo el acceso mas lento.

```
Disco fragmentado:
[ArchivoA-parte1] [ArchivoB] [ArchivoA-parte2] [vacio] [ArchivoA-parte3]

Disco desfragmentado:
[ArchivoA-completo] [ArchivoB] [vacio] [vacio] [vacio]
```

La **desfragmentacion** reorganiza los archivos para que sean contiguos.

### En SSD

Los SSD no sufren de fragmentacion de la misma manera porque no tienen cabezal. Acceder a cualquier posicion toma el mismo tiempo. **No debes desfragmentar un SSD** — solo desgasta las celdas sin beneficio.

Los SSD usan **TRIM** para informar al disco que bloques ya no se usan.

## Enlaces (Links)

### Enlace duro (Hard link)

Otro nombre para el mismo archivo. Ambos nombres apuntan a los mismos datos en disco. El archivo solo se borra cuando se eliminan todos sus enlaces.

### Enlace simbolico (Symbolic link / Symlink)

Un "atajo" que apunta a otro archivo por su ruta. Si se elimina el archivo original, el enlace simbolico queda roto.

```
archivo_original.txt  ← datos reales
enlace_duro.txt       ← otro nombre para los mismos datos
enlace_simbolico.txt  → apunta a "archivo_original.txt"
```

## Ejercicios

1. Cual es la diferencia entre una ruta absoluta y una relativa?
2. Un archivo `video.mp4` pesa 5 GB. Puedes guardarlo en una memoria USB con FAT32? Y con exFAT?
3. Que significan los permisos `chmod 644`?
4. Si renombras `programa.exe` a `programa.txt`, el archivo deja de ser un programa?
5. Por que no se debe desfragmentar un SSD?

<details>
<summary>Respuestas</summary>

1. Una ruta **absoluta** empieza desde la raiz del sistema (`/home/user/file.txt` o `C:\Users\file.txt`). Una ruta **relativa** parte del directorio actual (`../otro/file.txt`). La absoluta siempre identifica el mismo archivo sin importar donde estes; la relativa depende de tu ubicacion.
2. **FAT32:** No, tiene un limite de 4 GB por archivo. **exFAT:** Si, soporta archivos mucho mas grandes.
3. `6 = rw-` (dueno: leer y escribir), `4 = r--` (grupo: solo leer), `4 = r--` (otros: solo leer). Tipico para archivos de texto/configuracion.
4. No. El contenido del archivo no cambia, sigue siendo un ejecutable. Solo cambia el nombre. En Windows, el sistema operativo ya no lo reconocera como ejecutable por la extension, pero los datos internos son identicos.
5. Porque un SSD accede a cualquier bloque con la misma velocidad (no tiene cabezal). Desfragmentar solo causaria escrituras innecesarias, reduciendo la vida util de las celdas flash.

</details>
