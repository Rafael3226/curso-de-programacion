# Navegacion de Directorios

## Donde estoy?

Siempre que estas en la terminal, estas "parado" en un directorio. Para saber cual:

```bash
pwd
```

```
/home/usuario/proyectos
```

`pwd` = **P**rint **W**orking **D**irectory (imprimir directorio de trabajo).

## Ver el contenido de un directorio

```bash
ls
```

```
archivo.txt  fotos  notas.md  scripts
```

`ls` = **l**i**s**t (listar). Muestra archivos y carpetas en el directorio actual.

### Opciones utiles de ls

```bash
ls -l       # Lista larga (permisos, tamano, fecha)
ls -a       # Muestra archivos ocultos (empiezan con .)
ls -la      # Combinar ambas
ls -lh      # Tamanos legibles (KB, MB en vez de bytes)
ls -lt      # Ordenar por fecha de modificacion
```

Ejemplo de `ls -la`:

```
total 24
drwxr-xr-x  4 usuario grupo 4096 Mar 11 10:00 .
drwxr-xr-x 20 usuario grupo 4096 Mar 10 09:00 ..
-rw-r--r--  1 usuario grupo  150 Mar 11 09:30 .gitignore
-rw-r--r--  1 usuario grupo 2048 Mar 11 10:00 index.js
drwxr-xr-x  2 usuario grupo 4096 Mar 10 15:00 src
-rw-r--r--  1 usuario grupo  512 Mar 09 12:00 package.json
```

Desglose:

```
drwxr-xr-x  →  d = directorio, rwxr-xr-x = permisos
-rw-r--r--  →  - = archivo regular
.           →  directorio actual
..          →  directorio padre
.gitignore  →  archivo oculto (empieza con punto)
```

## Moverse entre directorios

```bash
cd nombre_carpeta     # Entrar a una carpeta
cd ..                 # Subir un nivel
cd ../..              # Subir dos niveles
cd ~                  # Ir al home del usuario
cd /                  # Ir a la raiz del sistema
cd -                  # Volver al directorio anterior
cd                    # Sin argumentos: ir al home
```

`cd` = **C**hange **D**irectory

### Ejemplo de navegacion

```
Estructura:
/home/usuario/
├── proyectos/
│   ├── web/
│   │   ├── index.html
│   │   └── style.css
│   └── api/
│       └── server.js
└── documentos/
    └── notas.txt
```

```bash
pwd                    # /home/usuario
cd proyectos           # /home/usuario/proyectos
cd web                 # /home/usuario/proyectos/web
ls                     # index.html  style.css
cd ..                  # /home/usuario/proyectos
cd api                 # /home/usuario/proyectos/api
cd ~                   # /home/usuario
cd proyectos/web       # Saltar directamente (ruta relativa)
cd /home/usuario/documentos  # Ruta absoluta
```

## Rutas absolutas vs relativas

```bash
# Ruta absoluta: empieza desde la raiz (/)
cd /home/usuario/proyectos/web

# Ruta relativa: empieza desde donde estoy
cd proyectos/web        # Si estoy en /home/usuario
cd ../api               # Si estoy en web, sube y entra a api
cd ./scripts            # ./ es "desde aqui" (el ./ es opcional)
```

## Autocompletado con Tab

No necesitas escribir nombres completos. Escribe las primeras letras y presiona **Tab**:

```bash
cd pro[Tab]         # Se completa a: cd proyectos/
cd proyectos/we[Tab]  # Se completa a: cd proyectos/web/
```

Si hay multiples opciones, presiona Tab dos veces para ver las posibilidades:

```bash
cd p[Tab][Tab]
# proyectos/  public/  packages/
```

**Usa Tab constantemente.** Es mas rapido y evita errores de escritura.

## Crear directorios

```bash
mkdir nueva_carpeta              # Crear una carpeta
mkdir -p ruta/con/subcarpetas    # Crear toda la ruta si no existe
```

```bash
# Ejemplo: crear estructura de un proyecto
mkdir -p mi-proyecto/src
mkdir -p mi-proyecto/tests
mkdir -p mi-proyecto/docs
```

O crear multiples a la vez:

```bash
mkdir -p mi-proyecto/{src,tests,docs}
```

## Consejos practicos

### Nombres con espacios

Si un archivo o carpeta tiene espacios en el nombre, usa comillas o escapa con `\`:

```bash
cd "mi carpeta"
cd mi\ carpeta
```

Recomendacion: evita espacios en nombres. Usa guiones o guiones bajos:

```
mi-proyecto       ← bien
mi_proyecto       ← bien
mi proyecto       ← funciona pero es incomodo
```

### Historial rapido

```bash
history              # Ver todos los comandos ejecutados
history | tail -20   # Ver los ultimos 20
```

O usa `Ctrl + R` y empieza a escribir para buscar un comando anterior.

## Ejercicios

1. Abre la terminal, ejecuta `pwd` y anota donde estas
2. Navega a tu carpeta home con `cd ~`, luego usa `ls -la` para ver todo su contenido
3. Crea una carpeta `practica` con una subcarpeta `datos` dentro usando un solo comando
4. Entra a `practica/datos`, verifica con `pwd`, luego vuelve al home con `cd ~`
5. Usa `cd -` para volver al ultimo directorio donde estabas

<details>
<summary>Respuestas</summary>

1. Depende de tu sistema. Ejemplos: `/home/tu_usuario`, `/Users/tu_usuario`, `/c/Users/tu_usuario`
2. Veras archivos ocultos como `.bashrc`, `.gitconfig`, etc.
3. `mkdir -p practica/datos`
4. `cd practica/datos` → `pwd` muestra la ruta completa → `cd ~`
5. `cd -` te lleva de vuelta a `practica/datos`

</details>
