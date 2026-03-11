# Pipes y Redireccion

## La filosofia Unix

Los sistemas Unix se disenaron con una idea poderosa: cada programa hace **una sola cosa bien**, y puedes **encadenarlos** para resolver problemas complejos. La herramienta para encadenarlos es el **pipe** (`|`).

## Redireccion de salida

Todo comando produce una salida. Por defecto se muestra en la pantalla, pero puedes **redirigirla** a un archivo:

### `>` — Escribir a archivo (sobreescribe)

```bash
echo "Hola" > saludo.txt      # Crea/sobreescribe saludo.txt con "Hola"
ls -la > listado.txt           # Guarda el resultado de ls en un archivo
date > timestamp.txt           # Guarda la fecha actual
```

### `>>` — Agregar a archivo (sin sobreescribir)

```bash
echo "Linea 1" > log.txt      # Crea con "Linea 1"
echo "Linea 2" >> log.txt     # Agrega "Linea 2" al final
echo "Linea 3" >> log.txt     # Agrega "Linea 3" al final
cat log.txt
# Linea 1
# Linea 2
# Linea 3
```

### `2>` — Redirigir errores

Los comandos tienen dos salidas: **stdout** (salida normal) y **stderr** (errores):

```bash
ls archivo_que_no_existe 2> errores.txt   # Solo redirige errores
ls archivo_que_no_existe > salida.txt 2>&1  # Redirige todo (salida + errores)
comando 2>/dev/null                        # Descartar errores silenciosamente
```

`/dev/null` es un "agujero negro": todo lo que le envias desaparece. Util para silenciar salida que no te interesa.

## Redireccion de entrada

### `<` — Leer desde archivo

```bash
wc -l < archivo.txt            # Cuenta lineas del archivo
sort < nombres.txt             # Ordena lineas del archivo
```

## Pipes (`|`)

Un pipe toma la **salida** de un comando y la pasa como **entrada** al siguiente:

```bash
comando1 | comando2 | comando3
```

La salida de `comando1` se convierte en la entrada de `comando2`, y asi sucesivamente.

### Ejemplos basicos

```bash
# Listar archivos y contar cuantos hay
ls | wc -l

# Ver procesos y buscar uno especifico
ps aux | grep node

# Listar archivos ordenados por tamano
ls -lS | head -10

# Ver historial y buscar un comando
history | grep "git"
```

### Comandos utiles para combinar con pipes

#### `sort` — Ordenar lineas

```bash
cat nombres.txt | sort           # Orden alfabetico
cat numeros.txt | sort -n        # Orden numerico
cat datos.txt | sort -r          # Orden inverso
cat datos.txt | sort -u          # Ordenar y eliminar duplicados
```

#### `uniq` — Eliminar lineas duplicadas consecutivas

```bash
cat datos.txt | sort | uniq       # Eliminar duplicados (requiere sort primero)
cat datos.txt | sort | uniq -c    # Contar ocurrencias de cada linea
```

#### `head` y `tail` — Primeras/ultimas lineas

```bash
cat log.txt | head -5             # Primeras 5 lineas
cat log.txt | tail -5             # Ultimas 5 lineas
cat log.txt | tail -f             # Seguir en tiempo real (para logs)
```

#### `wc` — Contar

```bash
cat archivo.txt | wc -l           # Contar lineas
cat archivo.txt | wc -w           # Contar palabras
cat archivo.txt | wc -c           # Contar bytes
```

#### `cut` — Extraer columnas

```bash
# Extraer la primera columna (separada por comas)
echo "Ana,25,Madrid" | cut -d',' -f1    # Ana
echo "Ana,25,Madrid" | cut -d',' -f2    # 25
echo "Ana,25,Madrid" | cut -d',' -f1,3  # Ana,Madrid
```

#### `tr` — Transformar caracteres

```bash
echo "hola mundo" | tr 'a-z' 'A-Z'      # HOLA MUNDO
echo "hola   mundo" | tr -s ' '          # hola mundo (elimina espacios extra)
echo "a,b,c" | tr ',' '\n'               # Cada elemento en su linea
```

#### `xargs` — Pasar resultado como argumentos

```bash
# Buscar archivos .tmp y borrarlos
find . -name "*.tmp" | xargs rm

# Buscar archivos .js y buscar "TODO" en ellos
find . -name "*.js" | xargs grep "TODO"
```

## Combinaciones practicas

### Analizar un log

```bash
# Contar errores en un log
grep "ERROR" app.log | wc -l

# Ver los 10 errores mas recientes
grep "ERROR" app.log | tail -10

# Contar errores por tipo
grep "ERROR" app.log | cut -d':' -f2 | sort | uniq -c | sort -rn
```

### Trabajar con un proyecto JavaScript

```bash
# Contar lineas de codigo JS en el proyecto
find . -name "*.js" -not -path "*/node_modules/*" | xargs wc -l

# Buscar todos los console.log que quedaron
grep -rn "console.log" --include="*.js" . | grep -v node_modules

# Listar las dependencias del package.json
cat package.json | grep -A 100 '"dependencies"'

# Ver los 10 archivos mas grandes del proyecto
find . -type f -not -path "*/node_modules/*" | xargs ls -la | sort -k5 -rn | head -10
```

### Procesamiento de datos

```bash
# Archivo CSV: extraer una columna y contar valores unicos
cat datos.csv | cut -d',' -f3 | sort | uniq -c | sort -rn

# Reemplazar texto en la salida
cat config.txt | tr '=' ': '
```

## Encadenamiento de comandos

Ademas de pipes, puedes encadenar comandos de otras formas:

```bash
# && — Ejecuta el segundo SOLO si el primero tuvo exito
mkdir proyecto && cd proyecto

# || — Ejecuta el segundo SOLO si el primero fallo
cd proyecto || echo "La carpeta no existe"

# ; — Ejecuta ambos sin importar si el primero fallo
echo "Inicio"; ls; echo "Fin"
```

## Ejercicios

1. Guarda la lista de archivos de tu directorio home en un archivo `mi_home.txt`
2. Cuenta cuantos archivos `.js` hay en un directorio (recursivamente)
3. Ordena alfabeticamente el contenido de un archivo de texto
4. Busca "function" en archivos `.js` y cuenta cuantas coincidencias hay
5. Crea un pipeline que liste archivos, filtre solo los `.txt` y cuente cuantos son

<details>
<summary>Respuestas</summary>

1. `ls -la ~ > mi_home.txt`
2. `find . -name "*.js" | wc -l`
3. `sort archivo.txt` o `cat archivo.txt | sort`
4. `grep -r "function" --include="*.js" . | wc -l`
5. `ls | grep "\.txt$" | wc -l`

</details>
