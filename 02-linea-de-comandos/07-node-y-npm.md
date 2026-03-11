# Node.js y npm

## Que es Node.js?

**Node.js** es un entorno de ejecucion que permite correr JavaScript **fuera del navegador**. Antes de Node.js (2009), JavaScript solo funcionaba dentro de un browser. Node.js abrio la puerta a usar JavaScript para:

- Servidores web
- Herramientas de linea de comandos
- Scripts de automatizacion
- Aplicaciones de escritorio (Electron)

## Instalar Node.js

1. Ve a **https://nodejs.org**
2. Descarga la version **LTS** (Long Term Support = estable)
3. Instala siguiendo las instrucciones

Verificar instalacion:

```bash
node --version     # v20.x.x (o superior)
npm --version      # 10.x.x (o superior)
```

Node.js viene con **npm** incluido.

## Usar Node.js

### Modo interactivo (REPL)

```bash
node
```

```
> 2 + 2
4
> "Hola".toUpperCase()
'HOLA'
> Math.random()
0.7234567890123456
> .exit
```

REPL = Read-Eval-Print-Loop. Lee tu codigo, lo ejecuta, imprime el resultado, repite.

### Ejecutar un archivo

```bash
# Crear archivo
echo 'console.log("Hola desde Node.js");' > app.js

# Ejecutar
node app.js
# Hola desde Node.js
```

## Que es npm?

**npm** (Node Package Manager) es el gestor de paquetes de JavaScript. Permite:

- Instalar librerias/paquetes que otros crearon
- Manejar las dependencias de tu proyecto
- Ejecutar scripts de tu proyecto
- Publicar tus propios paquetes

npm tiene el registro de paquetes mas grande del mundo: mas de **2 millones de paquetes**.

## Iniciar un proyecto con npm

```bash
mkdir mi-proyecto
cd mi-proyecto
npm init
```

Te hara una serie de preguntas (nombre, version, descripcion...). Para aceptar todo por defecto:

```bash
npm init -y
```

Esto crea un archivo `package.json`:

```json
{
  "name": "mi-proyecto",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

## package.json

Es el archivo central de todo proyecto Node.js. Define:

```json
{
  "name": "mi-app",
  "version": "1.0.0",
  "description": "Mi primera aplicacion",
  "main": "src/index.js",
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node tests/index.test.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "eslint": "^8.50.0"
  }
}
```

### Secciones clave

```
name             → Nombre del proyecto
version          → Version actual (versionado semantico)
main             → Archivo de entrada principal
scripts          → Comandos personalizados
dependencies     → Paquetes necesarios en produccion
devDependencies  → Paquetes solo para desarrollo
```

## Instalar paquetes

### Dependencia de produccion

```bash
npm install express          # o: npm i express
```

Esto:
1. Descarga `express` a la carpeta `node_modules/`
2. Agrega `express` a `dependencies` en `package.json`
3. Actualiza `package-lock.json` (registra versiones exactas)

### Dependencia de desarrollo

```bash
npm install --save-dev eslint   # o: npm i -D eslint
```

Se agrega a `devDependencies`. Son paquetes que solo necesitas al desarrollar (linters, testing, etc.), no en produccion.

### Instalar todo (desde package.json existente)

Cuando clonas un proyecto:

```bash
git clone https://github.com/usuario/proyecto.git
cd proyecto
npm install    # Lee package.json e instala todas las dependencias
```

### Instalar globalmente

```bash
npm install -g nodemon    # Disponible como comando en toda la computadora
```

Los paquetes globales se instalan en una ruta del sistema, no en el proyecto.

## node_modules

Carpeta donde npm guarda todos los paquetes instalados. Reglas importantes:

1. **Nunca la subas a Git.** Puede pesar cientos de MB. Agregala a `.gitignore`:
   ```
   node_modules/
   ```

2. **Nunca la edites manualmente.** npm la gestiona por ti.

3. **Se regenera con `npm install`.** Si la borras, simplemente ejecutas `npm install` de nuevo.

## package-lock.json

Registra las **versiones exactas** de cada paquete instalado. Garantiza que todos los que trabajan en el proyecto tengan las mismas versiones. **Si lo subas a Git, no lo borres.**

## Scripts de npm

Los scripts te permiten definir comandos personalizados:

```json
{
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node tests/run.js",
    "lint": "eslint src/",
    "build": "node scripts/build.js"
  }
}
```

Ejecutar:

```bash
npm start           # Ejecuta "start" (no necesita "run")
npm test            # Ejecuta "test" (no necesita "run")
npm run dev         # Ejecuta "dev" (scripts custom necesitan "run")
npm run lint        # Ejecuta "lint"
npm run build       # Ejecuta "build"
```

`npm start` y `npm test` son atajos especiales. Para cualquier otro nombre, se usa `npm run nombre`.

## Versionado semantico (SemVer)

Los paquetes de npm usan versionado semantico:

```
MAJOR.MINOR.PATCH
  2  .  4  .  1
```

```
MAJOR  → Cambios que ROMPEN compatibilidad (hay que adaptar tu codigo)
MINOR  → Funcionalidad nueva, compatible con versiones anteriores
PATCH  → Correcciones de bugs, compatible con versiones anteriores
```

En `package.json`:

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "lodash": "~4.17.21",
    "cors": "2.8.5"
  }
}
```

```
^4.18.2  → Acepta 4.18.2 a 4.x.x (cualquier minor/patch nuevo)
~4.17.21 → Acepta 4.17.21 a 4.17.x (solo patches nuevos)
2.8.5    → Exactamente 2.8.5 (sin actualizaciones)
```

## Desinstalar y actualizar

```bash
npm uninstall express         # Desinstalar un paquete
npm update                    # Actualizar todos los paquetes
npm outdated                  # Ver que paquetes tienen versiones nuevas
```

## npx — ejecutar sin instalar

`npx` viene con npm y permite ejecutar paquetes sin instalarlos globalmente:

```bash
# En vez de:
npm install -g create-react-app
create-react-app mi-app

# Puedes hacer:
npx create-react-app mi-app
```

Descarga temporalmente, ejecuta, y limpia. Muy util para herramientas que usas pocas veces.

## Ejemplo: crear un proyecto desde cero

```bash
# 1. Crear carpeta e inicializar
mkdir mi-api
cd mi-api
npm init -y

# 2. Instalar dependencias
npm install express
npm install -D nodemon

# 3. Crear estructura
mkdir src
```

Crear `src/index.js`:

```javascript
const express = require("express");
const app = express();
const PORT = 3000;

app.get("/", (req, res) => {
  res.json({ mensaje: "Hola mundo!" });
});

app.listen(PORT, () => {
  console.log(`Servidor corriendo en http://localhost:${PORT}`);
});
```

Agregar scripts al `package.json`:

```json
{
  "scripts": {
    "start": "node src/index.js",
    "dev": "nodemon src/index.js"
  }
}
```

```bash
# 4. Ejecutar
npm run dev
# Servidor corriendo en http://localhost:3000
```

## Resumen de comandos npm

```
Comando                      │ Funcion
─────────────────────────────┼──────────────────────────────
npm init -y                  │ Crear package.json
npm install paquete          │ Instalar dependencia
npm install -D paquete       │ Instalar dep. de desarrollo
npm install                  │ Instalar todo desde package.json
npm uninstall paquete        │ Desinstalar
npm run script               │ Ejecutar script personalizado
npm start                    │ Ejecutar script "start"
npm test                     │ Ejecutar script "test"
npm outdated                 │ Ver paquetes desactualizados
npx comando                  │ Ejecutar sin instalar global
```

## Ejercicios

1. Crea un proyecto nuevo con `npm init -y` y revisa el `package.json` generado
2. Instala el paquete `chalk` y verifica que aparece en `dependencies`
3. Agrega un script `"hello"` que ejecute `node -e "console.log('Hola')"` y ejecutalo con `npm run hello`
4. Que version se instalaria con `^2.3.4` si la ultima disponible es `2.9.0`? Y `3.0.0`?
5. Por que `node_modules` no debe subirse a Git?

<details>
<summary>Respuestas</summary>

1. `mkdir test-npm && cd test-npm && npm init -y && cat package.json`

2. `npm install chalk` — luego en package.json veras `"chalk": "^5.x.x"` en dependencies.

3. En package.json agregar `"hello": "node -e \"console.log('Hola')\""` dentro de scripts. Ejecutar: `npm run hello`.

4. Con `^2.3.4` se instalaria `2.9.0` (acepta hasta `2.x.x`). **No** instalaria `3.0.0` porque `^` no cruza la version major.

5. Porque puede pesar cientos de MB, contiene miles de archivos, y se regenera facilmente con `npm install` leyendo `package.json` y `package-lock.json`. Subirla haria el repo enorme e innecesariamente lento.

</details>
