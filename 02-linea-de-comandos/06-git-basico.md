# Git Basico

## Que es Git?

**Git** es un sistema de **control de versiones**. Registra los cambios que haces a tus archivos a lo largo del tiempo, permitiendote:

- Ver el historial completo de cambios
- Volver a una version anterior si algo sale mal
- Trabajar en nuevas funcionalidades sin afectar el codigo estable
- Colaborar con otros desarrolladores sin pisarse el trabajo

Git fue creado en 2005 por Linus Torvalds (el creador de Linux).

## Configuracion inicial

Solo necesitas hacer esto una vez:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

Verificar:

```bash
git config --list
```

## Conceptos fundamentales

### Repositorio (repo)

Un directorio que Git esta rastreando. Contiene tu codigo y una carpeta oculta `.git` con todo el historial.

### Los tres estados de Git

```
Directorio de trabajo     Staging Area (Index)     Repositorio
(Working Directory)       (Area de preparacion)    (Historial)

  Archivos modificados  →  Cambios listos para  →  Snapshot guardado
                            commit                  permanentemente

       git add ──────→          git commit ──────→
```

1. **Working Directory:** tus archivos como estan ahora
2. **Staging Area:** cambios que marcaste para incluir en el proximo commit
3. **Repository:** historial de commits (snapshots guardados)

### Commit

Un **commit** es una "foto" del estado de tu proyecto en un momento dado. Cada commit tiene:
- Un identificador unico (hash SHA-1)
- Los cambios realizados
- Un mensaje descriptivo
- Autor y fecha

## Flujo basico

### 1. Crear un repositorio

```bash
# Iniciar un repo nuevo
mkdir mi-proyecto
cd mi-proyecto
git init

# O clonar uno existente
git clone https://github.com/usuario/repositorio.git
```

### 2. Ver el estado

```bash
git status
```

```
On branch main
Changes not staged for commit:
  modified:   index.js

Untracked files:
  utils.js
```

Esto te dice:
- En que rama estas
- Que archivos modificaste
- Que archivos son nuevos (untracked)

### 3. Agregar cambios al staging

```bash
git add archivo.js           # Agregar un archivo especifico
git add src/                 # Agregar toda una carpeta
git add .                    # Agregar todo (con cuidado)
git add -p                   # Revisar cambio por cambio antes de agregar
```

### 4. Hacer commit

```bash
git commit -m "Agregar funcion de login"
```

El mensaje debe describir **que** hiciste y **por que**. Buenos mensajes:

```
Agregar validacion de email en formulario de registro
Corregir error de division por cero en calculadora
Refactorizar funcion de busqueda para mejorar rendimiento
```

Malos mensajes:

```
Fix
Cambios
asdfgh
WIP
```

### 5. Ver el historial

```bash
git log                      # Historial completo
git log --oneline            # Version compacta
git log --oneline -10        # Ultimos 10 commits
git log --graph --oneline    # Con grafico de ramas
```

```
a1b2c3d Agregar funcion de login
d4e5f6a Crear pagina principal
f7g8h9i Commit inicial
```

## Ver cambios

```bash
git diff                     # Cambios en working directory (no staged)
git diff --staged            # Cambios en staging area
git diff HEAD                # Todos los cambios desde el ultimo commit
git diff abc123..def456      # Diferencia entre dos commits
```

## Deshacer cambios

```bash
# Descartar cambios en un archivo (volver al ultimo commit)
git checkout -- archivo.js
# O en versiones nuevas de git:
git restore archivo.js

# Quitar del staging (pero mantener los cambios)
git reset HEAD archivo.js
# O en versiones nuevas:
git restore --staged archivo.js

# Volver al estado de un commit anterior (crea un nuevo commit)
git revert abc123
```

## Ramas (Branches)

Las ramas permiten trabajar en funcionalidades nuevas **sin afectar** el codigo principal.

```
          main:    A ── B ── C ── D ── E
                              \       ↑
          feature:             F ── G ─┘ (merge)
```

```bash
# Ver ramas
git branch                   # Lista ramas locales
git branch -a                # Lista todas (locales + remotas)

# Crear y cambiar a una nueva rama
git checkout -b nueva-rama
# O en versiones nuevas:
git switch -c nueva-rama

# Cambiar de rama
git checkout main
# O:
git switch main

# Eliminar una rama (despues de merge)
git branch -d nombre-rama
```

### Merge — unir ramas

```bash
# Primero, ir a la rama destino
git switch main

# Luego, unir la otra rama
git merge nueva-rama
```

Si ambas ramas modificaron las mismas lineas, Git no puede decidir cual conservar. Esto es un **conflicto de merge**:

```
<<<<<<< HEAD
let mensaje = "Hola desde main";
=======
let mensaje = "Hola desde feature";
>>>>>>> feature
```

Para resolverlo: edita el archivo manualmente, elige que version conservar, elimina los marcadores (`<<<<`, `====`, `>>>>`), y haz commit.

## Trabajar con repositorios remotos (GitHub)

### Conectar un repo local a GitHub

```bash
git remote add origin https://github.com/tu-usuario/tu-repo.git
git push -u origin main
```

### Enviar cambios (push)

```bash
git push                     # Enviar al remoto
git push origin main         # Especificar rama
```

### Obtener cambios (pull)

```bash
git pull                     # Descargar y aplicar cambios del remoto
```

### Clonar un repositorio existente

```bash
git clone https://github.com/usuario/repo.git
cd repo
```

## .gitignore

Archivo que le dice a Git que archivos **no** rastrear:

```
# .gitignore

node_modules/        # Dependencias (se reinstalan con npm install)
.env                 # Variables de entorno secretas
dist/                # Archivos generados
*.log                # Archivos de log
.DS_Store            # Archivos del sistema (macOS)
Thumbs.db            # Archivos del sistema (Windows)
```

Crea el `.gitignore` **antes** de hacer tu primer commit.

## Flujo de trabajo tipico

```bash
# 1. Crear rama para nueva funcionalidad
git switch -c agregar-login

# 2. Hacer cambios, agregar y commitear
# ... editas archivos ...
git add src/login.js src/auth.js
git commit -m "Agregar formulario de login"

# ... mas cambios ...
git add src/login.js
git commit -m "Agregar validacion de campos"

# 3. Volver a main y actualizar
git switch main
git pull

# 4. Unir tu rama
git merge agregar-login

# 5. Subir a remoto
git push

# 6. Eliminar la rama (ya no se necesita)
git branch -d agregar-login
```

## Resumen de comandos

```
Comando               │ Funcion
──────────────────────┼──────────────────────────────
git init              │ Iniciar repositorio
git clone URL         │ Clonar repositorio remoto
git status            │ Ver estado actual
git add archivo       │ Agregar al staging
git commit -m "msg"   │ Guardar snapshot
git log --oneline     │ Ver historial
git diff              │ Ver cambios
git branch            │ Listar ramas
git switch -c rama    │ Crear y cambiar a rama
git merge rama        │ Unir rama
git push              │ Enviar al remoto
git pull              │ Obtener del remoto
git restore archivo   │ Descartar cambios
```

## Ejercicios

1. Crea un repositorio nuevo, agrega un archivo `index.js` y haz tu primer commit
2. Crea una rama `feature`, haz cambios, y unela de vuelta a `main`
3. Usa `git log --oneline` para ver tu historial
4. Crea un `.gitignore` que ignore `node_modules/` y archivos `.env`
5. Modifica un archivo y usa `git diff` para ver los cambios antes de hacer commit

<details>
<summary>Respuestas</summary>

1.
```bash
mkdir mi-repo && cd mi-repo
git init
echo "console.log('hola');" > index.js
git add index.js
git commit -m "Commit inicial con index.js"
```

2.
```bash
git switch -c feature
echo "// nueva funcion" >> index.js
git add index.js
git commit -m "Agregar nueva funcion"
git switch main
git merge feature
```

3. `git log --oneline` mostrara algo como:
```
a1b2c3d Agregar nueva funcion
d4e5f6a Commit inicial con index.js
```

4.
```bash
echo "node_modules/" > .gitignore
echo ".env" >> .gitignore
git add .gitignore
git commit -m "Agregar .gitignore"
```

5. Modifica un archivo, luego ejecuta `git diff` antes de `git add`.

</details>
