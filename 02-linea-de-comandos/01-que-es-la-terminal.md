# Que es la Terminal

## La interfaz antes de las ventanas

Antes de que existieran las interfaces graficas (ventanas, iconos, raton), la unica forma de interactuar con una computadora era escribiendo comandos de texto. Esa interfaz de texto se llama **terminal** o **linea de comandos**.

Hoy sigue siendo una herramienta fundamental para cualquier programador. Muchas tareas son mas rapidas, mas precisas y mas automatizables desde la terminal que con clicks.

## Terminologia

Estos terminos se usan a menudo de forma intercambiable, pero tienen diferencias:

```
Terminal     → La ventana/aplicacion donde escribes comandos
Shell        → El programa que interpreta tus comandos
CLI          → Command Line Interface (cualquier programa que se usa con texto)
Consola      → Sinonimo informal de terminal
```

### Shells comunes

```
Shell           │ Sistema        │ Descripcion
────────────────┼────────────────┼──────────────────────────
Bash            │ Linux, macOS   │ El mas comun en Linux
Zsh             │ macOS (default)│ Bash mejorado, default en Mac
PowerShell      │ Windows        │ Shell moderno de Microsoft
CMD             │ Windows        │ El clasico de Windows (limitado)
Git Bash        │ Windows        │ Bash para Windows (viene con Git)
```

Para este curso usaremos comandos de **Bash**, que funcionan en Linux, macOS y en Git Bash/WSL en Windows.

## Como abrir la terminal

**Windows:**
- Busca "Git Bash" (si tienes Git instalado)
- O instala Windows Terminal desde la Microsoft Store
- O usa WSL (Windows Subsystem for Linux)

**macOS:**
- `Cmd + Space` → escribe "Terminal" → Enter
- O instala iTerm2 para una terminal mejorada

**Linux:**
- `Ctrl + Alt + T` en la mayoria de distribuciones

**VS Code:**
- `` Ctrl + ` `` (backtick) abre la terminal integrada

## Anatomia del prompt

Cuando abres la terminal, ves algo como esto:

```
usuario@computadora:~/proyectos$
│         │           │         │
│         │           │         └── $ indica que puedes escribir
│         │           └── directorio actual (~ = home)
│         └── nombre de la computadora
└── tu nombre de usuario
```

En algunos sistemas se ve diferente:

```
PS C:\Users\ana>          (PowerShell)
C:\Users\ana>             (CMD)
ana@laptop:~$             (Bash/Linux)
➜  proyectos git:(main)  (Zsh con tema personalizado)
```

Despues del prompt (`$` o `>`), escribes un comando y presionas **Enter** para ejecutarlo.

## Tu primer comando

```bash
echo "Hola mundo"
```

Salida:

```
Hola mundo
```

`echo` simplemente imprime texto en la terminal. Es el equivalente a `console.log()` de JavaScript, pero en la shell.

## La terminal es poderosa porque...

**1. Es precisa** — dices exactamente lo que quieres, sin ambiguedad.

```bash
# Renombrar 500 archivos de .jpeg a .jpg
for f in *.jpeg; do mv "$f" "${f%.jpeg}.jpg"; done
```

Hacer esto con clicks tomaria mucho tiempo. Con un comando, segundos.

**2. Es automatizable** — puedes guardar comandos en scripts y repetirlos.

**3. Es universal** — los servidores no tienen interfaz grafica. Si quieres administrar un servidor, necesitas la terminal.

**4. Es eficiente** — para muchas tareas de programacion (git, npm, compilar, ejecutar), la terminal es mas rapida que cualquier menu grafico.

## Atajos esenciales del teclado

```
Atajo              │ Funcion
───────────────────┼──────────────────────────
Tab                │ Autocompletar nombres de archivos/comandos
↑ / ↓              │ Navegar historial de comandos
Ctrl + C           │ Cancelar/interrumpir el comando actual
Ctrl + L           │ Limpiar la pantalla (equivale a 'clear')
Ctrl + A           │ Ir al inicio de la linea
Ctrl + E           │ Ir al final de la linea
Ctrl + R           │ Buscar en el historial de comandos
Ctrl + D           │ Cerrar la terminal (o enviar EOF)
Ctrl + U           │ Borrar desde el cursor hasta el inicio
Ctrl + K           │ Borrar desde el cursor hasta el final
```

## Ejercicios

1. Abre la terminal en tu computadora
2. Escribe `echo "Hola mundo"` y presiona Enter
3. Presiona la flecha arriba para repetir el ultimo comando
4. Escribe `echo "Ho` y presiona Tab — que pasa?
5. Escribe `date` y presiona Enter — que muestra?

<details>
<summary>Respuestas</summary>

1-3. Practica directa en la terminal.
4. Tab intenta autocompletar. Si no hay coincidencia unica, puede no completar o mostrar opciones.
5. Muestra la fecha y hora actual del sistema, por ejemplo: `Wed Mar 11 10:30:00 UTC 2026`

</details>
