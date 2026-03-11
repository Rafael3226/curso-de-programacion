# Introduccion a JavaScript y Configuracion del Entorno

## Por que JavaScript?

JavaScript es uno de los lenguajes de programacion mas usados en el mundo. Algunas razones para aprenderlo como primer lenguaje:

- **Versatil:** funciona en el navegador, en servidores (Node.js), en aplicaciones moviles y de escritorio
- **Accesible:** no necesitas instalar nada para empezar — tu navegador ya lo tiene
- **Demanda laboral:** es el lenguaje mas solicitado en desarrollo web
- **Comunidad enorme:** millones de recursos, librerias y desarrolladores

## Donde ejecutar JavaScript

### Opcion 1: La consola del navegador

La forma mas rapida de empezar. No necesitas instalar nada.

1. Abre tu navegador (Chrome, Firefox, Edge)
2. Presiona `F12` o `Ctrl + Shift + J` (Windows/Linux) / `Cmd + Option + J` (Mac)
3. Ve a la pestana **Console**
4. Escribe y presiona Enter:

```javascript
console.log("Hola mundo");
```

### Opcion 2: Node.js

Node.js permite ejecutar JavaScript fuera del navegador, directamente en tu computadora.

**Instalacion:**

1. Ve a https://nodejs.org
2. Descarga la version LTS (Long Term Support)
3. Instala siguiendo las instrucciones
4. Abre una terminal y verifica:

```
node --version
```

**Usar Node.js:**

Modo interactivo (REPL):
```
node
> console.log("Hola mundo");
Hola mundo
> 2 + 2
4
> .exit
```

Ejecutar un archivo:
```
node mi_programa.js
```

### Opcion 3: Editor de codigo + terminal

Para proyectos reales necesitaras un editor de codigo. El mas popular es **Visual Studio Code** (VS Code):

1. Descarga VS Code desde https://code.visualstudio.com
2. Crea un archivo con extension `.js` (ejemplo: `hola.js`)
3. Escribe tu codigo
4. Abre la terminal integrada (`Ctrl + ñ` o `` Ctrl + ` ``)
5. Ejecuta con `node hola.js`

## Tu primer programa

Crea un archivo llamado `hola.js`:

```javascript
console.log("Hola mundo");
```

Ejecutalo:

```
node hola.js
```

Salida:

```
Hola mundo
```

`console.log()` es la forma principal de mostrar informacion en la consola. Lo usaras constantemente para ver resultados y depurar tu codigo.

## Anatomia basica de un programa

```javascript
// Esto es un comentario de una linea. El programa lo ignora.

/*
   Esto es un comentario
   de varias lineas.
*/

// Declarar una variable
let nombre = "Ana";

// Mostrar algo en consola
console.log("Hola, " + nombre);

// Una operacion
let resultado = 10 + 5;
console.log(resultado); // 15
```

Observaciones:
- Cada instruccion termina con `;` (punto y coma). Es opcional en JS pero es buena practica usarlo.
- Los comentarios con `//` o `/* */` son ignorados al ejecutar. Sirven para documentar.
- JavaScript se ejecuta **de arriba hacia abajo**, linea por linea.

## Errores comunes al empezar

### SyntaxError (error de sintaxis)

Escribiste algo que JS no entiende:

```javascript
console.log("Hola mundo)  // Falta cerrar la comilla
// SyntaxError: Invalid or unexpected token
```

### ReferenceError (variable no definida)

Usas una variable que no existe:

```javascript
console.log(nombre);  // 'nombre' no fue declarada
// ReferenceError: nombre is not defined
```

### TypeError (operacion invalida para ese tipo)

Intentas hacer algo que no tiene sentido para ese tipo de dato:

```javascript
let x = 5;
x();  // Intentas llamar un numero como si fuera una funcion
// TypeError: x is not a function
```

No te asustes por los errores. **Leer el mensaje de error** es una de las habilidades mas importantes que vas a desarrollar. Siempre te dice que salio mal y en que linea.

## Ejercicios

1. Abre la consola del navegador y escribe `console.log("Hola mundo")`
2. Instala Node.js y verifica que funciona con `node --version`
3. Crea un archivo `ejercicio.js` que muestre tu nombre y tu edad en consola
4. Provoca un error a proposito (escribe algo mal) y lee el mensaje de error
5. Escribe un programa que muestre el resultado de `123 * 456`

<details>
<summary>Respuestas</summary>

3.
```javascript
console.log("Mi nombre es Ana");
console.log("Tengo 25 anos");
```

5.
```javascript
console.log(123 * 456); // 56088
```

</details>
