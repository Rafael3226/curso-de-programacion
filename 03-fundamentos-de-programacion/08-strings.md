# Strings (Cadenas de Texto)

## Que es un string?

Un string es una secuencia de caracteres. En JavaScript, los strings son **inmutables**: no puedes cambiar caracteres individuales, solo crear strings nuevos.

```javascript
const saludo = "Hola mundo";
const nombre = 'Ana';
const template = `Hola, ${nombre}`;
```

## Crear strings

```javascript
// Comillas dobles
const a = "Hola";

// Comillas simples
const b = 'Hola';

// Template literals (backticks) — permiten interpolacion y multilinea
const c = `Hola, ${nombre}`;
const d = `Primera linea
Segunda linea
Tercera linea`;
```

## Acceso a caracteres

```javascript
const texto = "JavaScript";

texto[0];            // "J"
texto[4];            // "S"
texto[texto.length - 1]; // "t"
texto.at(-1);        // "t" (indice negativo)

texto.length;        // 10

// Los strings son inmutables
texto[0] = "j";      // No hace nada, no da error
console.log(texto);   // "JavaScript" (sin cambio)
```

## Metodos de busqueda

```javascript
const frase = "Aprender JavaScript es divertido";

// Incluye?
frase.includes("JavaScript");    // true
frase.includes("Python");        // false

// Empieza con?
frase.startsWith("Aprender");    // true

// Termina con?
frase.endsWith("divertido");     // true

// Posicion de la primera aparicion
frase.indexOf("JavaScript");     // 9
frase.indexOf("xyz");            // -1 (no encontrado)

// Buscar con regex
frase.search(/java/i);           // 9 (i = case insensitive)
```

## Metodos de extraccion

```javascript
const texto = "Hola mundo cruel";

// slice(inicio, fin) — fin no incluido
texto.slice(5, 10);       // "mundo"
texto.slice(5);            // "mundo cruel"
texto.slice(-5);           // "cruel"

// substring(inicio, fin) — similar a slice
texto.substring(5, 10);   // "mundo"
```

## Metodos de transformacion

Todos devuelven un **string nuevo** (el original no cambia):

```javascript
const texto = "  Hola Mundo  ";

// Cambiar caso
texto.toUpperCase();          // "  HOLA MUNDO  "
texto.toLowerCase();          // "  hola mundo  "

// Quitar espacios
texto.trim();                 // "Hola Mundo"
texto.trimStart();            // "Hola Mundo  "
texto.trimEnd();              // "  Hola Mundo"

// Reemplazar
"Hola mundo".replace("mundo", "JavaScript");   // "Hola JavaScript"
"aaa bbb aaa".replace("aaa", "xxx");           // "xxx bbb aaa" (solo el primero)
"aaa bbb aaa".replaceAll("aaa", "xxx");        // "xxx bbb xxx" (todos)

// Repetir
"ja".repeat(3);              // "jajaja"

// Rellenar
"5".padStart(3, "0");        // "005"
"5".padEnd(3, "0");          // "500"
"42".padStart(5, " ");       // "   42"
```

## Split y Join

Convertir entre strings y arrays:

```javascript
// String → Array
"Hola mundo".split(" ");            // ["Hola", "mundo"]
"a,b,c,d".split(",");               // ["a", "b", "c", "d"]
"JavaScript".split("");              // ["J", "a", "v", "a", "S", "c", "r", "i", "p", "t"]
"uno--dos--tres".split("--");        // ["uno", "dos", "tres"]

// Array → String
["Hola", "mundo"].join(" ");         // "Hola mundo"
["2026", "03", "11"].join("-");      // "2026-03-11"
["a", "b", "c"].join("");            // "abc"
```

## Template literals

Los backticks permiten cosas que las comillas normales no:

```javascript
const nombre = "Ana";
const edad = 25;

// Interpolacion de expresiones
console.log(`${nombre} tiene ${edad} anos`);
console.log(`En 10 anos tendra ${edad + 10}`);
console.log(`Es ${edad >= 18 ? "mayor" : "menor"} de edad`);

// Multilinea
const html = `
<div>
  <h1>${nombre}</h1>
  <p>Edad: ${edad}</p>
</div>
`;
```

## Comparacion de strings

```javascript
"abc" === "abc";     // true
"abc" === "ABC";     // false (case sensitive)

// Comparar ignorando mayusculas
"Hola".toLowerCase() === "hola".toLowerCase(); // true

// Orden lexicografico
"a" < "b";           // true
"abc" < "abd";       // true (compara caracter por caracter)

// Para comparacion sensible al idioma
"a".localeCompare("b");    // -1 (a va antes)
"b".localeCompare("a");    //  1 (b va despues)
"a".localeCompare("a");    //  0 (iguales)
```

## Patrones comunes

### Verificar si esta vacio

```javascript
const texto = "";

if (texto.length === 0) { /* vacio */ }
if (texto === "") { /* vacio */ }
if (!texto) { /* vacio, null o undefined (falsy) */ }
if (texto.trim() === "") { /* vacio o solo espacios */ }
```

### Capitalizar primera letra

```javascript
function capitalizar(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

capitalizar("hola");      // "Hola"
capitalizar("javascript"); // "Javascript"
```

### Contar apariciones

```javascript
function contarApariciones(texto, buscar) {
  return texto.split(buscar).length - 1;
}

contarApariciones("banana", "a");  // 3
contarApariciones("hola hola", "hola"); // 2
```

### Invertir un string

```javascript
function invertir(str) {
  return str.split("").reverse().join("");
}

invertir("hola");  // "aloh"
```

### Verificar palindromo

```javascript
function esPalindromo(str) {
  const limpio = str.toLowerCase().replace(/[^a-z0-9]/g, "");
  return limpio === limpio.split("").reverse().join("");
}

esPalindromo("Anita lava la tina"); // true
esPalindromo("hola");               // false
```

### Truncar texto

```javascript
function truncar(texto, maxLen) {
  if (texto.length <= maxLen) return texto;
  return texto.slice(0, maxLen - 3) + "...";
}

truncar("Hola mundo cruel", 10); // "Hola mu..."
```

## Numeros y strings

```javascript
// String a numero
Number("42");          // 42
parseInt("42px");      // 42
parseFloat("3.14");    // 3.14
+"42";                 // 42 (operador unario +)

// Numero a string
String(42);            // "42"
(42).toString();       // "42"
(255).toString(16);    // "ff" (hexadecimal)
(10).toString(2);      // "1010" (binario)

// Formatear numeros
(3.14159).toFixed(2);  // "3.14"
(1234567).toLocaleString(); // "1,234,567"
```

## Ejercicios

1. Escribe una funcion que cuente cuantas vocales tiene un string
2. Escribe una funcion que convierta "hola mundo cruel" a "Hola Mundo Cruel" (capitalizar cada palabra)
3. Escribe una funcion que verifique si dos strings son anagramas ("roma", "amor")
4. Dada la cadena "nombre=Ana&edad=25&ciudad=Madrid", extraela a un objeto `{nombre: "Ana", edad: "25", ciudad: "Madrid"}`
5. Escribe una funcion que censure una palabra en un texto, reemplazandola por asteriscos

<details>
<summary>Respuestas</summary>

1.
```javascript
function contarVocales(str) {
  return str.toLowerCase().split("").filter(c => "aeiou".includes(c)).length;
}
```

2.
```javascript
function capitalizarPalabras(str) {
  return str.split(" ").map(p => p[0].toUpperCase() + p.slice(1)).join(" ");
}
```

3.
```javascript
function sonAnagramas(a, b) {
  const ordenar = s => s.toLowerCase().split("").sort().join("");
  return ordenar(a) === ordenar(b);
}
```

4.
```javascript
function parsearQuery(query) {
  const obj = {};
  for (const par of query.split("&")) {
    const [clave, valor] = par.split("=");
    obj[clave] = valor;
  }
  return obj;
}
```

5.
```javascript
function censurar(texto, palabra) {
  return texto.replaceAll(palabra, "*".repeat(palabra.length));
}
censurar("Esto es malo y muy malo", "malo"); // "Esto es **** y muy ****"
```

</details>
