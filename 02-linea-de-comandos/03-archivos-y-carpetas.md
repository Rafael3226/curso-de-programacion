# Manipulacion de Archivos y Carpetas

## Crear archivos

### touch — crear archivos vacios

```bash
touch archivo.txt              # Crea un archivo vacio
touch index.js style.css       # Crear varios a la vez
```

Si el archivo ya existe, `touch` solo actualiza su fecha de modificacion sin cambiar el contenido.

### Crear archivos con contenido

```bash
echo "Hola mundo" > saludo.txt          # Escribe texto en un archivo (sobreescribe)
echo "Otra linea" >> saludo.txt         # Agrega al final (no sobreescribe)
```

`>` sobreescribe, `>>` agrega al final. Cuidado con `>`, borra lo que habia.

## Leer archivos

```bash
cat archivo.txt          # Muestra todo el contenido
head archivo.txt         # Muestra las primeras 10 lineas
head -n 5 archivo.txt    # Muestra las primeras 5 lineas
tail archivo.txt         # Muestra las ultimas 10 lineas
tail -n 5 archivo.txt    # Muestra las ultimas 5 lineas
less archivo.txt         # Visor interactivo (q para salir)
wc archivo.txt           # Cuenta lineas, palabras y bytes
wc -l archivo.txt        # Solo cuenta lineas
```

### less — el visor interactivo

Cuando un archivo es largo, `less` es mejor que `cat`:

```
Dentro de less:
  q         → Salir
  Space     → Avanzar una pagina
  b         → Retroceder una pagina
  /texto    → Buscar "texto"
  n         → Siguiente resultado de busqueda
  g         → Ir al inicio
  G         → Ir al final
```

## Copiar

```bash
cp origen.txt destino.txt           # Copiar archivo
cp archivo.txt carpeta/             # Copiar a otra carpeta
cp -r carpeta_origen/ carpeta_destino/  # Copiar carpeta completa (-r = recursivo)
```

## Mover / Renombrar

`mv` hace ambas cosas: mover y renombrar.

```bash
mv viejo.txt nuevo.txt              # Renombrar
mv archivo.txt carpeta/             # Mover a otra carpeta
mv archivo.txt carpeta/nuevo_nombre.txt  # Mover y renombrar a la vez
mv carpeta1/ carpeta2/              # Mover carpeta completa
```

## Eliminar

```bash
rm archivo.txt                      # Eliminar archivo
rm archivo1.txt archivo2.txt        # Eliminar varios
rm -r carpeta/                      # Eliminar carpeta y todo su contenido
rm -i archivo.txt                   # Pedir confirmacion antes de borrar
```

**CUIDADO:** `rm` no tiene papelera de reciclaje. Lo que borras, desaparece. Estas combinaciones son especialmente peligrosas:

```bash
# NUNCA hagas esto sin estar MUY seguro:
rm -rf /              # Borra TODO el sistema
rm -rf *              # Borra todo en el directorio actual
rm -rf ~              # Borra todo tu home
```

Consejo: usa `rm -i` cuando no estes seguro, o ejecuta `ls` primero para verificar que vas a borrar.

```bash
# Primero verifica
ls *.log
# resultado: error.log  debug.log  access.log

# Luego borra
rm *.log
```

## Comodines (wildcards)

Los comodines permiten referirse a multiples archivos con un patron:

```bash
*           # Cualquier cosa (0 o mas caracteres)
?           # Exactamente un caracter
[abc]       # Uno de los caracteres listados
[0-9]       # Un digito
[a-z]       # Una letra minuscula
```

```bash
ls *.js             # Todos los archivos .js
ls *.{js,css}       # Todos los .js y .css
ls archivo?.txt     # archivo1.txt, archivoA.txt, etc.
ls foto[0-9].jpg    # foto0.jpg a foto9.jpg
rm *.tmp            # Eliminar todos los .tmp
cp src/*.js dist/   # Copiar todos los .js de src a dist
```

## Buscar archivos

### find — buscar por nombre o propiedades

```bash
find . -name "*.js"                   # Buscar archivos .js desde el directorio actual
find . -name "index.html"             # Buscar un archivo especifico
find . -type d -name "node_modules"   # Buscar directorios con ese nombre
find . -type f -size +1M              # Archivos mayores a 1 MB
find . -name "*.log" -mtime -7        # Archivos .log modificados en los ultimos 7 dias
```

Opciones de `find`:
```
-name "patron"    → buscar por nombre
-type f           → solo archivos
-type d           → solo directorios
-size +10M        → mayores a 10 MB
-mtime -7         → modificados hace menos de 7 dias
```

### Buscar texto dentro de archivos

```bash
grep "texto" archivo.txt              # Buscar "texto" en un archivo
grep -r "TODO" .                      # Buscar recursivamente en todos los archivos
grep -n "error" log.txt               # Mostrar numero de linea
grep -i "hola" archivo.txt            # Ignorar mayusculas/minusculas
grep -c "function" *.js               # Contar coincidencias por archivo
```

```bash
# Ejemplo: buscar todas las funciones en tu proyecto
grep -rn "function" --include="*.js" .
```

## Comparar archivos

```bash
diff archivo1.txt archivo2.txt     # Ver diferencias entre dos archivos
```

## Ejemplo practico: organizar un proyecto

```bash
# Crear estructura
mkdir -p mi-app/{src,tests,docs}
touch mi-app/src/index.js
touch mi-app/src/utils.js
touch mi-app/tests/index.test.js
touch mi-app/package.json
touch mi-app/.gitignore

# Verificar
find mi-app -type f

# Resultado:
# mi-app/src/index.js
# mi-app/src/utils.js
# mi-app/tests/index.test.js
# mi-app/package.json
# mi-app/.gitignore
```

## Resumen de comandos

```
Comando │ Funcion                    │ Ejemplo
────────┼────────────────────────────┼────────────────────────
touch   │ Crear archivo vacio        │ touch archivo.txt
mkdir   │ Crear directorio           │ mkdir -p ruta/carpeta
cat     │ Ver contenido completo     │ cat archivo.txt
head    │ Ver inicio de archivo      │ head -n 20 archivo.txt
tail    │ Ver final de archivo       │ tail -n 20 archivo.txt
less    │ Visor interactivo          │ less archivo_largo.txt
cp      │ Copiar                     │ cp -r origen/ destino/
mv      │ Mover/Renombrar            │ mv viejo.txt nuevo.txt
rm      │ Eliminar                   │ rm -r carpeta/
find    │ Buscar archivos            │ find . -name "*.js"
grep    │ Buscar texto en archivos   │ grep -rn "TODO" .
diff    │ Comparar archivos          │ diff a.txt b.txt
wc      │ Contar lineas/palabras     │ wc -l archivo.txt
```

## Ejercicios

1. Crea un archivo `notas.txt` con el contenido "Mi primera nota" usando `echo` y `>`
2. Agrega una segunda linea "Segunda nota" sin borrar la primera
3. Crea una carpeta `backup` y copia `notas.txt` dentro
4. Busca todos los archivos `.txt` en tu directorio actual
5. Busca la palabra "nota" dentro de todos los archivos `.txt`

<details>
<summary>Respuestas</summary>

1. `echo "Mi primera nota" > notas.txt`
2. `echo "Segunda nota" >> notas.txt`
3. `mkdir backup && cp notas.txt backup/`
4. `find . -name "*.txt"`
5. `grep "nota" *.txt` o `grep -r "nota" --include="*.txt" .`

</details>
