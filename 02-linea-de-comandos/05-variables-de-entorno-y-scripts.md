# Variables de Entorno y Scripts

## Variables de entorno

Las variables de entorno son **valores almacenados en la sesion de tu terminal** que los programas pueden leer. Funcionan como una configuracion global del sistema.

### Ver variables de entorno

```bash
env                    # Ver todas las variables de entorno
echo $HOME             # Ver una variable especifica
echo $PATH             # Ver el PATH
echo $USER             # Tu nombre de usuario
```

El `$` antes del nombre indica que es una variable.

### Variables importantes

```
Variable │ Que contiene                          │ Ejemplo
─────────┼───────────────────────────────────────┼─────────────────────
HOME     │ Ruta de tu directorio home            │ /home/usuario
PATH     │ Donde buscar programas ejecutables    │ /usr/bin:/usr/local/bin
USER     │ Tu nombre de usuario                  │ ana
SHELL    │ Tu shell actual                       │ /bin/bash
PWD      │ Directorio actual                     │ /home/usuario/proyecto
LANG     │ Idioma del sistema                    │ es_ES.UTF-8
EDITOR   │ Editor de texto por defecto           │ vim
```

### PATH — la variable mas importante

Cuando escribes un comando como `node`, la terminal busca un programa con ese nombre en las carpetas listadas en `PATH`:

```bash
echo $PATH
# /usr/local/bin:/usr/bin:/bin:/home/usuario/.local/bin
```

Las rutas estan separadas por `:`. La terminal busca en orden:
1. `/usr/local/bin/node` — existe? Si → lo ejecuta
2. `/usr/bin/node` — si no encontro en la anterior, busca aqui
3. Y asi sucesivamente...

Si el programa no esta en ninguna carpeta del PATH:

```
bash: node: command not found
```

#### Encontrar donde esta un programa

```bash
which node          # /usr/local/bin/node
which git           # /usr/bin/git
```

### Crear y modificar variables

```bash
# Crear una variable local (solo esta sesion)
MI_VARIABLE="hola"
echo $MI_VARIABLE      # hola

# Exportar para que los programas hijos la vean
export MI_VARIABLE="hola"

# Crear y exportar en una linea
export API_KEY="abc123"
```

Estas variables desaparecen al cerrar la terminal. Para hacerlas permanentes, agregalas a tu archivo de configuracion del shell.

### Archivos de configuracion del shell

```bash
# Bash
~/.bashrc         # Se ejecuta al abrir cada terminal
~/.bash_profile   # Se ejecuta al iniciar sesion

# Zsh
~/.zshrc          # Se ejecuta al abrir cada terminal

# Ejemplo: agregar una variable permanente
echo 'export MI_VARIABLE="hola"' >> ~/.bashrc
source ~/.bashrc  # Recargar sin cerrar la terminal
```

### Variables de entorno en Node.js

JavaScript accede a las variables de entorno a traves de `process.env`:

```javascript
// archivo: app.js
console.log(process.env.HOME);        // /home/usuario
console.log(process.env.API_KEY);     // el valor que hayas definido
console.log(process.env.NODE_ENV);    // "production" o "development"
```

Ejecutar con una variable temporal:

```bash
API_KEY="mi_clave_secreta" node app.js
NODE_ENV=production node app.js
```

## Scripts de shell

Un script es un archivo con una secuencia de comandos que se ejecutan uno tras otro. En lugar de escribir 10 comandos manualmente, los guardas en un archivo y los ejecutas con un solo comando.

### Tu primer script

Crea un archivo `saludo.sh`:

```bash
#!/bin/bash

echo "Hola, bienvenido!"
echo "Hoy es: $(date)"
echo "Estas en: $(pwd)"
echo "Tu usuario es: $USER"
```

La primera linea `#!/bin/bash` (llamada **shebang**) indica que shell debe ejecutar el script.

Hacerlo ejecutable y correr:

```bash
chmod +x saludo.sh     # Dar permiso de ejecucion
./saludo.sh            # Ejecutar
```

O sin cambiar permisos:

```bash
bash saludo.sh
```

### Variables en scripts

```bash
#!/bin/bash

nombre="Ana"
edad=25

echo "Me llamo $nombre y tengo $edad anos"
echo "En 10 anos tendre $((edad + 10)) anos"
```

`$(( ))` permite hacer operaciones aritmeticas dentro del script.

### Argumentos

Los scripts pueden recibir argumentos:

```bash
#!/bin/bash
# archivo: saludar.sh

echo "Hola, $1!"
echo "Tu edad es $2"
echo "Numero de argumentos: $#"
echo "Todos los argumentos: $@"
```

```bash
./saludar.sh Ana 25
# Hola, Ana!
# Tu edad es 25
# Numero de argumentos: 2
# Todos los argumentos: Ana 25
```

```
$0  → nombre del script
$1  → primer argumento
$2  → segundo argumento
$#  → cantidad de argumentos
$@  → todos los argumentos
```

### Condicionales en scripts

```bash
#!/bin/bash

if [ -f "package.json" ]; then
    echo "Este directorio tiene un package.json"
    echo "Instalando dependencias..."
    npm install
else
    echo "No es un proyecto Node.js"
fi
```

Operadores de comparacion en bash:

```
Archivos:
-f archivo    → archivo existe y es un archivo regular
-d directorio → existe y es un directorio
-e path       → existe (archivo o directorio)

Strings:
= o ==        → iguales
!=            → diferentes
-z "$var"     → variable vacia

Numeros:
-eq    → igual
-ne    → no igual
-gt    → mayor que
-lt    → menor que
-ge    → mayor o igual
-le    → menor o igual
```

### Bucles en scripts

```bash
#!/bin/bash

# Iterar sobre archivos
for archivo in *.js; do
    echo "Archivo JS: $archivo"
done

# Iterar sobre una lista
for nombre in Ana Pedro Luis; do
    echo "Hola, $nombre"
done

# Iterar con numeros
for i in {1..5}; do
    echo "Numero: $i"
done
```

### Script practico: setup de proyecto JS

```bash
#!/bin/bash
# archivo: crear-proyecto.sh

if [ -z "$1" ]; then
    echo "Uso: ./crear-proyecto.sh nombre-del-proyecto"
    exit 1
fi

NOMBRE=$1

echo "Creando proyecto: $NOMBRE"

mkdir -p "$NOMBRE"/{src,tests,docs}
cd "$NOMBRE"

# Crear package.json basico
cat > package.json << 'EOF'
{
  "name": "NOMBRE_PROYECTO",
  "version": "1.0.0",
  "description": "",
  "main": "src/index.js",
  "scripts": {
    "start": "node src/index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
EOF

# Reemplazar el nombre
sed -i "s/NOMBRE_PROYECTO/$NOMBRE/g" package.json 2>/dev/null || \
    sed -i '' "s/NOMBRE_PROYECTO/$NOMBRE/g" package.json

# Crear archivos iniciales
echo "// Punto de entrada" > src/index.js
echo "node_modules/" > .gitignore

# Inicializar git
git init

echo ""
echo "Proyecto '$NOMBRE' creado con exito!"
echo "Estructura:"
find . -not -path "./.git/*" -not -path "./.git"
```

```bash
chmod +x crear-proyecto.sh
./crear-proyecto.sh mi-app
```

## Alias — atajos personalizados

Un alias es un atajo para un comando largo:

```bash
# Crear alias temporales
alias ll="ls -la"
alias gs="git status"
alias gp="git push"

# Ahora puedes escribir:
ll       # en vez de ls -la
gs       # en vez de git status
```

Para que sean permanentes, agregalos a tu `~/.bashrc` o `~/.zshrc`:

```bash
echo 'alias ll="ls -la"' >> ~/.bashrc
echo 'alias gs="git status"' >> ~/.bashrc
source ~/.bashrc
```

## Ejercicios

1. Muestra el valor de tu variable `PATH` y cuenta cuantas rutas contiene
2. Crea una variable de entorno `MI_NOMBRE` con tu nombre y muestrala con `echo`
3. Escribe un script que reciba un nombre como argumento y diga "Hola, [nombre]!"
4. Escribe un script que verifique si existe un archivo `index.js` en el directorio actual
5. Crea un alias `proyectos` que te lleve directamente a tu carpeta de proyectos

<details>
<summary>Respuestas</summary>

1. `echo $PATH | tr ':' '\n' | wc -l`
2. `export MI_NOMBRE="Ana"` y luego `echo $MI_NOMBRE`
3.
```bash
#!/bin/bash
echo "Hola, $1!"
```
4.
```bash
#!/bin/bash
if [ -f "index.js" ]; then
    echo "index.js existe"
else
    echo "index.js no encontrado"
fi
```
5. `alias proyectos="cd ~/proyectos"` (agregar a `~/.bashrc` para que sea permanente)

</details>
