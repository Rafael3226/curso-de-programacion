# Variables y Tipos de Datos

## Que es una variable?

Una variable es un **nombre** que le asignas a un **valor** almacenado en memoria. Es como una caja etiquetada donde guardas informacion.

```javascript
let edad = 25;
//  ↑      ↑
// nombre  valor
```

Cuando usas `edad` en tu codigo, JavaScript va a buscar el valor guardado (25).

## Declaracion de variables

JavaScript tiene tres formas de declarar variables:

### `let` — variable que puede cambiar

```javascript
let puntos = 0;
puntos = 10;     // OK, puedes reasignar
puntos = 25;     // OK, cambio de nuevo
```

### `const` — constante, no puede cambiar

```javascript
const PI = 3.14159;
PI = 3;  // TypeError: Assignment to constant variable.
```

Usa `const` por defecto. Solo usa `let` cuando necesites reasignar el valor.

### `var` — la forma antigua (evitala)

```javascript
var nombre = "Ana";  // Funciona, pero tiene comportamientos confusos
```

`var` tiene problemas de alcance (scope) que causan bugs sutiles. En codigo moderno se usa `let` y `const` exclusivamente.

## Reglas para nombres de variables

```
Valido:              No valido:
─────────────────    ──────────────────
edad                 2nombre   (no empieza con numero)
nombre_completo      mi-var    (guion no permitido)
_privado             let       (palabra reservada)
$precio              mi var    (no puede tener espacios)
camelCase
miVariable123
```

Convencion en JavaScript: usar **camelCase** para variables y funciones:

```javascript
let nombreCompleto = "Ana Garcia";
let fechaDeNacimiento = "1999-03-15";
let esEstudiante = true;
```

## Tipos de datos primitivos

JavaScript tiene 7 tipos primitivos. Los mas importantes son:

### Number (numeros)

JavaScript no distingue entre enteros y decimales. Todos son `number`:

```javascript
let entero = 42;
let decimal = 3.14;
let negativo = -7;
let grande = 1e6;       // 1,000,000 (notacion cientifica)
let hex = 0xFF;          // 255 (hexadecimal)
let binario = 0b1010;    // 10 (binario)

console.log(typeof entero);  // "number"
console.log(typeof decimal); // "number"
```

Valores especiales:

```javascript
console.log(1 / 0);         // Infinity
console.log(-1 / 0);        // -Infinity
console.log("hola" * 2);    // NaN (Not a Number)
console.log(typeof NaN);    // "number" (ironicamente)
```

Limitaciones (como vimos en fundamentos):

```javascript
console.log(0.1 + 0.2);                  // 0.30000000000000004
console.log(Number.MAX_SAFE_INTEGER);     // 9007199254740991 (2^53 - 1)
```

### String (texto)

Secuencias de caracteres. Se pueden crear con comillas simples, dobles o backticks:

```javascript
let simple = 'Hola';
let doble = "Mundo";
let backtick = `Hola Mundo`;  // template literal

// Los tres son equivalentes para texto simple
```

Template literals (backticks) permiten **interpolacion**:

```javascript
let nombre = "Ana";
let edad = 25;

// Concatenacion clasica (con +)
console.log("Hola, " + nombre + ". Tienes " + edad + " anos.");

// Template literal (mas limpio)
console.log(`Hola, ${nombre}. Tienes ${edad} anos.`);
```

Caracteres de escape:

```javascript
let lineas = "Primera linea\nSegunda linea";   // \n = nueva linea
let tab = "Columna1\tColumna2";                // \t = tabulacion
let comilla = "El dijo \"hola\"";              // \" = comilla dentro de string
let ruta = "C:\\Users\\ana";                    // \\ = barra invertida
```

Propiedades y metodos utiles:

```javascript
let texto = "JavaScript";

console.log(texto.length);          // 10
console.log(texto[0]);              // "J" (primer caracter)
console.log(texto[texto.length-1]); // "t" (ultimo caracter)
console.log(texto.toUpperCase());   // "JAVASCRIPT"
console.log(texto.toLowerCase());   // "javascript"
console.log(texto.includes("Script")); // true
console.log(texto.indexOf("S"));    // 4
console.log(texto.slice(0, 4));     // "Java"
```

Los strings son **inmutables**: no puedes cambiar un caracter individual.

```javascript
let s = "Hola";
s[0] = "h";        // No hace nada (no da error, pero no cambia)
console.log(s);     // "Hola"
s = "hola";         // Esto si funciona: creas un STRING NUEVO
```

### Boolean (verdadero/falso)

Solo dos valores posibles: `true` o `false`.

```javascript
let esActivo = true;
let tieneCuenta = false;

console.log(typeof esActivo);  // "boolean"

// Resultado de comparaciones
console.log(5 > 3);      // true
console.log(10 === 10);   // true
console.log("a" === "b"); // false
```

### Undefined

Una variable declarada pero sin valor asignado:

```javascript
let x;
console.log(x);          // undefined
console.log(typeof x);   // "undefined"
```

### Null

Representa la **ausencia intencional** de valor. Tu lo asignas a proposito:

```javascript
let usuario = null;  // "No hay usuario todavia"
console.log(typeof null);  // "object" (un bug historico de JS)
```

Diferencia clave:
- `undefined` = "no se le ha dado valor"
- `null` = "intencionalmente vacio"

### BigInt (enteros grandes)

Para numeros enteros mas alla del limite seguro:

```javascript
let enorme = 9007199254740993n;  // nota la 'n' al final
let otro = BigInt("123456789012345678901234567890");

console.log(typeof enorme);  // "bigint"
```

### Symbol (identificadores unicos)

Usado en programacion avanzada para crear identificadores unicos. No es comun al empezar:

```javascript
let id = Symbol("descripcion");
```

## El operador typeof

Devuelve el tipo de un valor como string:

```javascript
console.log(typeof 42);          // "number"
console.log(typeof "hola");      // "string"
console.log(typeof true);        // "boolean"
console.log(typeof undefined);   // "undefined"
console.log(typeof null);        // "object" (bug historico)
console.log(typeof Symbol());    // "symbol"
console.log(typeof 42n);         // "bigint"
```

## Conversion de tipos

JavaScript convierte tipos automaticamente en muchas situaciones (**coercion**), lo cual puede causar resultados inesperados:

```javascript
// Coercion automatica (implicita)
console.log("5" + 3);      // "53" (numero se convierte a string)
console.log("5" - 3);      // 2   (string se convierte a numero)
console.log("5" * "2");    // 10  (ambos se convierten a numero)
console.log(true + 1);     // 2   (true se convierte a 1)
console.log(false + 1);    // 1   (false se convierte a 0)
```

Conversion explicita (manual) — mucho mas seguro:

```javascript
// A numero
let num = Number("42");        // 42
let num2 = Number("hola");    // NaN
let num3 = parseInt("42px");  // 42 (ignora lo que no es numero)
let num4 = parseFloat("3.14");// 3.14

// A string
let str = String(42);          // "42"
let str2 = (42).toString();    // "42"

// A boolean
let bool = Boolean(1);         // true
let bool2 = Boolean(0);        // false
let bool3 = Boolean("");       // false
let bool4 = Boolean("hola");  // true
```

### Valores falsy y truthy

En JavaScript, algunos valores se evaluan como `false` en contextos booleanos (**falsy**):

```javascript
// Todos estos son FALSY (se evaluan como false):
false
0
-0
""           // string vacio
null
undefined
NaN

// TODO lo demas es TRUTHY (se evalua como true):
true
42
"hola"
[]           // array vacio (cuidado, es truthy!)
{}           // objeto vacio (truthy!)
"0"          // string "0" (truthy, no es el numero 0)
"false"      // string "false" (truthy, no es el boolean false)
```

## == vs ===

```javascript
// == (igualdad con coercion) — EVITAR
console.log(5 == "5");     // true (convierte "5" a numero)
console.log(0 == false);   // true
console.log("" == false);  // true
console.log(null == undefined); // true

// === (igualdad estricta) — USAR SIEMPRE
console.log(5 === "5");    // false (diferente tipo)
console.log(0 === false);  // false
console.log("" === false); // false
console.log(null === undefined); // false
```

Regla: **siempre usa `===` y `!==`**. Olvidate de `==` y `!=`.

## Ejercicios

1. Declara variables para almacenar: tu nombre, tu edad, si eres estudiante. Usa el tipo apropiado para cada una.
2. Que devuelve `typeof null`? Por que es confuso?
3. Sin ejecutar el codigo, predice el resultado: `"10" + 5`, `"10" - 5`, `true + true`
4. Cual es la diferencia entre `undefined` y `null`?
5. Cuales de estos valores son falsy? `0`, `"0"`, `""`, `null`, `[]`, `false`, `"false"`

<details>
<summary>Respuestas</summary>

1.
```javascript
const nombre = "Ana";
let edad = 25;
const esEstudiante = true;
```

2. Devuelve `"object"`. Es confuso porque `null` NO es un objeto; es un bug del primer JavaScript que nunca se corrigio por compatibilidad.

3. `"10" + 5 = "105"` (concatenacion), `"10" - 5 = 5` (resta convierte a numero), `true + true = 2` (true = 1)

4. `undefined` significa que una variable fue declarada pero no se le asigno valor. `null` es un valor que TU asignas intencionalmente para indicar "vacio" o "sin valor".

5. Falsy: `0`, `""`, `null`, `false`. Truthy: `"0"` (string no vacio), `[]` (array, es objeto), `"false"` (string no vacio).

</details>
