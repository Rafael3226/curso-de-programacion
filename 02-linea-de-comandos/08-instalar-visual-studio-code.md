# Instalar Visual Studio Code

## Que es Visual Studio Code?

**Visual Studio Code (VS Code)** es un editor de codigo gratuito creado por Microsoft. Es el editor mas popular entre programadores por ser:

- **Gratuito y open source**
- **Ligero** — se abre rapido, no consume muchos recursos
- **Extensible** — miles de extensiones para cualquier lenguaje o herramienta
- **Multiplataforma** — funciona en Windows, macOS y Linux
- **Con terminal integrada** — puedes escribir codigo y ejecutar comandos en el mismo lugar

No hay que confundirlo con **Visual Studio** (sin "Code"), que es un IDE mucho mas pesado orientado a C# y .NET.

## Descargar e instalar

### Windows

1. Ve a **https://code.visualstudio.com**
2. Haz clic en el boton de descarga para Windows
3. Ejecuta el instalador `.exe`
4. Durante la instalacion, marca estas opciones importantes:
   ```
   [x] Agregar la accion "Abrir con Code" al menu contextual de archivos
   [x] Agregar la accion "Abrir con Code" al menu contextual de directorios
   [x] Agregar a PATH (disponible despues de reiniciar)
   ```
5. Finaliza la instalacion

### macOS

1. Ve a **https://code.visualstudio.com**
2. Descarga el archivo `.zip` para macOS
3. Extrae el archivo y arrastra **Visual Studio Code.app** a la carpeta **Aplicaciones**
4. Para usar `code` desde la terminal:
   - Abre VS Code
   - Presiona `Cmd + Shift + P`
   - Escribe "Shell Command: Install 'code' command in PATH"
   - Selecciona la opcion

### Linux (Ubuntu/Debian)

```bash
# Opcion 1: Descargar .deb desde la pagina oficial e instalar
sudo dpkg -i code_*.deb

# Opcion 2: Usando snap
sudo snap install code --classic

# Opcion 3: Usando apt (agregar repositorio de Microsoft)
sudo apt update
sudo apt install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt update
sudo apt install code
```

## Verificar la instalacion

Desde la terminal:

```bash
code --version
```

Si muestra un numero de version (por ejemplo `1.90.0`), la instalacion fue exitosa.

## Abrir VS Code desde la terminal

Una de las ventajas de VS Code es que puedes abrirlo directamente desde la linea de comandos:

```bash
code .                  # Abre la carpeta actual en VS Code
code archivo.js         # Abre un archivo especifico
code mi-proyecto/       # Abre una carpeta especifica
code -n .               # Abre en una ventana nueva
code --diff a.js b.js   # Compara dos archivos
```

Esto es muy util cuando ya estas trabajando en la terminal y quieres editar archivos rapidamente.

## La interfaz de VS Code

```
┌──────────────────────────────────────────────────────────┐
│  Menu    Archivo.js  ×   Otro.js  ×                      │
├────────┬─────────────────────────────────────────────────┤
│        │                                                  │
│  E     │   1  const nombre = "Mundo";                     │
│  x     │   2  console.log(`Hola ${nombre}`);              │
│  p     │   3                                              │
│  l     │                                                  │
│  o     │                                                  │
│  r     │                                                  │
│  a     │                                                  │
│  d     ├─────────────────────────────────────────────────┤
│  o     │  TERMINAL                                        │
│  r     │  $ node archivo.js                               │
│        │  Hola Mundo                                      │
├────────┴─────────────────────────────────────────────────┤
│  main ●   Ln 2, Col 1   UTF-8   JavaScript               │
└──────────────────────────────────────────────────────────┘
```

### Areas principales

```
Area                  │ Descripcion
──────────────────────┼──────────────────────────────────────
Barra lateral izq.    │ Explorador de archivos, busqueda, Git, extensiones
Editor central        │ Donde escribes y editas tu codigo
Terminal integrada    │ Terminal dentro de VS Code (Ctrl + `)
Barra de estado       │ Rama de Git, lenguaje, codificacion, posicion del cursor
Paleta de comandos    │ Acceso rapido a todas las funciones (Ctrl + Shift + P)
```

## Atajos de teclado esenciales

```
Atajo                      │ Funcion
───────────────────────────┼──────────────────────────────────
Ctrl + Shift + P           │ Paleta de comandos (el atajo mas importante)
Ctrl + P                   │ Buscar y abrir archivos rapidamente
Ctrl + `                   │ Abrir/cerrar la terminal integrada
Ctrl + B                   │ Mostrar/ocultar la barra lateral
Ctrl + S                   │ Guardar archivo
Ctrl + Z                   │ Deshacer
Ctrl + Shift + Z           │ Rehacer
Ctrl + D                   │ Seleccionar la siguiente ocurrencia de la palabra
Ctrl + Shift + K           │ Eliminar linea completa
Alt + ↑ / ↓                │ Mover linea arriba/abajo
Ctrl + Shift + L           │ Seleccionar todas las ocurrencias de la palabra
Ctrl + /                   │ Comentar/descomentar linea
Ctrl + F                   │ Buscar en el archivo actual
Ctrl + Shift + F           │ Buscar en todos los archivos del proyecto
```

En macOS, reemplaza `Ctrl` por `Cmd`.

## Extensiones recomendadas

Las extensiones agregan funcionalidad a VS Code. Para instalar una extension:

1. Haz clic en el icono de extensiones en la barra lateral (o `Ctrl + Shift + X`)
2. Busca el nombre de la extension
3. Haz clic en **Install**

### Extensiones esenciales para este curso

```
Extension                         │ Para que sirve
──────────────────────────────────┼────────────────────────────────
ESLint                            │ Detecta errores en JavaScript
Prettier - Code formatter         │ Formatea tu codigo automaticamente
Error Lens                        │ Muestra errores directamente en la linea
GitLens                           │ Informacion avanzada de Git
```

### Otras extensiones utiles

```
Extension                         │ Para que sirve
──────────────────────────────────┼────────────────────────────────
Live Server                       │ Servidor local para HTML/CSS/JS
Path Intellisense                 │ Autocompletado de rutas de archivos
Material Icon Theme               │ Iconos bonitos para archivos y carpetas
Spanish Language Pack             │ Interfaz de VS Code en espanol
```

## Configuracion basica recomendada

Abre la configuracion con `Ctrl + ,` (o `Cmd + ,` en macOS). Algunas configuraciones recomendadas:

```
Configuracion              │ Valor recomendado  │ Por que
───────────────────────────┼────────────────────┼─────────────────────────
Tab Size                   │ 2                  │ Estandar en JavaScript
Format On Save             │ ✓ (activado)       │ Formatea al guardar
Auto Save                  │ afterDelay         │ Guarda automaticamente
Word Wrap                  │ on                 │ No necesitas scroll horizontal
Bracket Pair Colorization  │ ✓ (activado)       │ Colorea pares de llaves/parentesis
```

Tambien puedes editar el archivo de configuracion directamente. Presiona `Ctrl + Shift + P` y busca "Open User Settings (JSON)":

```json
{
  "editor.tabSize": 2,
  "editor.formatOnSave": true,
  "editor.wordWrap": "on",
  "editor.bracketPairColorization.enabled": true,
  "files.autoSave": "afterDelay",
  "terminal.integrated.defaultProfile.windows": "Git Bash"
}
```

La ultima linea configura Git Bash como terminal por defecto en Windows, lo cual es recomendable para seguir este curso.

## La terminal integrada

VS Code tiene una terminal integrada que puedes abrir con `` Ctrl + ` ``:

```bash
# Puedes hacer todo lo que harias en una terminal normal:
node archivo.js
npm install
git status
```

### Ventajas de la terminal integrada

- **No cambias de ventana** — codigo y terminal en el mismo lugar
- **Abre en la carpeta del proyecto** — no necesitas navegar con `cd`
- **Multiples terminales** — puedes tener varias abiertas al mismo tiempo (`Ctrl + Shift + `)

### Cambiar la shell de la terminal

Si en Windows te abre CMD o PowerShell y prefieres Git Bash:

1. Abre la paleta de comandos (`Ctrl + Shift + P`)
2. Busca "Terminal: Select Default Profile"
3. Selecciona **Git Bash**

## Ejercicios

1. Descarga e instala Visual Studio Code desde https://code.visualstudio.com
2. Abre VS Code desde la terminal con `code .` en cualquier carpeta
3. Abre la terminal integrada con `` Ctrl + ` `` y ejecuta `echo "Hola desde VS Code"`
4. Instala la extension **ESLint** desde el panel de extensiones
5. Abre la paleta de comandos (`Ctrl + Shift + P`) y cambia el tema de color

<details>
<summary>Respuestas</summary>

1. Practica directa — sigue las instrucciones de instalacion para tu sistema operativo.

2. Abre tu terminal, navega a una carpeta (`cd ~/proyectos`) y escribe `code .`. VS Code se abrira con esa carpeta en el explorador.

3. La terminal integrada funciona igual que una terminal externa. Deberia imprimir `Hola desde VS Code`.

4. En la barra lateral, haz clic en el icono de extensiones (cuadros), busca "ESLint", y haz clic en Install.

5. En la paleta de comandos, escribe "Color Theme" y selecciona el tema que prefieras. Algunos populares: Dark+ (default), One Dark Pro, Dracula.

</details>
