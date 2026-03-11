# Operadores

## Que es un operador?

Un operador es un simbolo que realiza una operacion sobre uno o mas valores (operandos) y produce un resultado.

```javascript
let resultado = 5 + 3;
//              ↑ ↑ ↑
//        operando operador operando
```

## Operadores aritmeticos

```javascript
let a = 10;
let b = 3;

console.log(a + b);   // 13  Suma
console.log(a - b);   // 7   Resta
console.log(a * b);   // 30  Multiplicacion
console.log(a / b);   // 3.3333...  Division (siempre decimal en JS)
console.log(a % b);   // 1   Modulo (resto de la division)
console.log(a ** b);  // 1000  Exponenciacion (10³)
```

### El operador modulo (%)

Devuelve el **resto** de la division entera. Es mas util de lo que parece:

```javascript
// Saber si un numero es par o impar
console.log(7 % 2);   // 1 (impar)
console.log(8 % 2);   // 0 (par)

// Limitar un valor a un rango (ej: horas del reloj)
console.log(25 % 24);  // 1  (25 horas = 1 hora)
console.log(50 % 24);  // 2  (50 horas = 2 horas)

// Obtener el ultimo digito
console.log(1234 % 10);  // 4
```

### Operadores de incremento/decremento

```javascript
let x = 5;

x++;    // x = x + 1 → x ahora es 6
x--;    // x = x - 1 → x ahora es 5

// Prefijo vs Postfijo
let a = 5;
let b = a++;  // b = 5, a = 6 (asigna ANTES de incrementar)

let c = 5;
let d = ++c;  // d = 6, c = 6 (incrementa ANTES de asignar)
```

### Operadores de asignacion compuesta

Atajos para operaciones comunes:

```javascript
let x = 10;

x += 5;   // x = x + 5  → 15
x -= 3;   // x = x - 3  → 12
x *= 2;   // x = x * 2  → 24
x /= 4;   // x = x / 4  → 6
x %= 4;   // x = x % 4  → 2
x **= 3;  // x = x ** 3 → 8
```

## Operadores de comparacion

Devuelven un boolean (`true` o `false`):

```javascript
let a = 10;
let b = 5;

console.log(a > b);    // true   Mayor que
console.log(a < b);    // false  Menor que
console.log(a >= 10);  // true   Mayor o igual
console.log(a <= 9);   // false  Menor o igual
console.log(a === b);  // false  Igual estricto
console.log(a !== b);  // true   Diferente estricto
```

### Comparacion de strings

Los strings se comparan **caracter por caracter** usando su valor Unicode:

```javascript
console.log("a" < "b");       // true (97 < 98)
console.log("abc" < "abd");   // true (compara en la 3ra posicion: c < d)
console.log("A" < "a");       // true (65 < 97, mayusculas van antes)
console.log("10" < "9");      // true (compara "1" vs "9", y "1" < "9")
```

Cuidado: comparar strings numericos da resultados inesperados. Convierte a numero primero:

```javascript
console.log(Number("10") < Number("9"));  // false (10 < 9 es falso)
```

## Operadores logicos

Combinan expresiones booleanas (como vimos en logica booleana):

### AND logico (`&&`)

Devuelve `true` solo si **ambos** son verdaderos:

```javascript
console.log(true && true);    // true
console.log(true && false);   // false
console.log(false && true);   // false
console.log(false && false);  // false

// Uso practico
let edad = 25;
let tieneID = true;
console.log(edad >= 18 && tieneID);  // true (cumple ambas condiciones)
```

### OR logico (`||`)

Devuelve `true` si **al menos uno** es verdadero:

```javascript
console.log(true || false);   // true
console.log(false || true);   // true
console.log(false || false);  // false

// Uso practico
let esAdmin = false;
let esModerador = true;
console.log(esAdmin || esModerador);  // true (al menos uno es true)
```

### NOT logico (`!`)

Invierte el valor:

```javascript
console.log(!true);   // false
console.log(!false);  // true

let activo = true;
console.log(!activo);  // false
```

### Evaluacion de cortocircuito

JavaScript no evalua mas de lo necesario:

```javascript
// AND: si el primero es false, no evalua el segundo
false && console.log("Esto nunca se ejecuta");

// OR: si el primero es true, no evalua el segundo
true || console.log("Esto nunca se ejecuta");
```

Uso practico — valores por defecto:

```javascript
let nombre = null;
let saludo = nombre || "Invitado";
console.log(saludo);  // "Invitado"

let nombre2 = "Ana";
let saludo2 = nombre2 || "Invitado";
console.log(saludo2);  // "Ana"
```

### Nullish coalescing (`??`)

Similar a `||` pero solo considera `null` y `undefined` (no `0` ni `""`):

```javascript
let puntos = 0;

console.log(puntos || 10);   // 10  (0 es falsy, no es lo que queremos)
console.log(puntos ?? 10);   // 0   (0 NO es null/undefined, lo mantiene)

let nombre = "";
console.log(nombre || "Invitado");   // "Invitado" (string vacio es falsy)
console.log(nombre ?? "Invitado");   // "" (no es null/undefined)
```

## Operador ternario

Un `if/else` compacto en una sola linea:

```javascript
// condicion ? valor_si_true : valor_si_false

let edad = 20;
let mensaje = edad >= 18 ? "Mayor de edad" : "Menor de edad";
console.log(mensaje);  // "Mayor de edad"

// Equivale a:
// if (edad >= 18) {
//     mensaje = "Mayor de edad";
// } else {
//     mensaje = "Menor de edad";
// }
```

Usalo para expresiones simples. Si se vuelve complejo, usa `if/else` normal.

## Operadores de bits (bitwise)

Operan directamente sobre los bits (como vimos en fundamentos):

```javascript
console.log(12 & 10);   // 8    AND
console.log(12 | 10);   // 14   OR
console.log(12 ^ 10);   // 6    XOR
console.log(~12);        // -13  NOT
console.log(5 << 1);    // 10   Shift izquierdo (×2)
console.log(20 >> 2);   // 5    Shift derecho (÷4)
```

## Operador de concatenacion (+)

Cuando `+` se usa con strings, une (concatena) en vez de sumar:

```javascript
console.log("Hola" + " " + "mundo");  // "Hola mundo"
console.log("Edad: " + 25);           // "Edad: 25" (convierte 25 a string)
console.log(5 + 3);                    // 8 (ambos numeros: suma)
console.log("5" + 3);                  // "53" (hay string: concatena)
console.log(5 + 3 + " gatos");        // "8 gatos" (5+3=8, luego concatena)
console.log("tengo " + 5 + 3);        // "tengo 53" (todo es concatenacion)
```

## Precedencia de operadores

Los operadores tienen un orden de prioridad (como en matematicas):

```
Prioridad (de mayor a menor):
──────────────────────────────
()         Parentesis (siempre primero)
**         Exponenciacion
* / %      Multiplicacion, division, modulo
+ -        Suma, resta
< > <= >=  Comparacion
=== !==    Igualdad
&&         AND logico
||         OR logico
?:         Ternario
= += -=    Asignacion
```

```javascript
// Sin parentesis
let r = 2 + 3 * 4;    // 14 (primero 3*4, luego +2)

// Con parentesis
let r2 = (2 + 3) * 4; // 20 (primero 2+3, luego *4)

// En caso de duda, usa parentesis para ser explicito
let puede = (edad >= 18) && (tieneID === true);
```

## Operador optional chaining (`?.`)

Accede a propiedades de objetos que podrian no existir, sin causar error:

```javascript
let usuario = null;

// Sin optional chaining
// console.log(usuario.nombre);   // TypeError: Cannot read properties of null

// Con optional chaining
console.log(usuario?.nombre);     // undefined (sin error)
```

## Ejercicios

1. Sin ejecutar, que resultado da: `10 + 5 * 2`? Y `(10 + 5) * 2`?
2. Escribe una expresion que determine si un numero es par usando el operador `%`
3. Que devuelve `0 || "default"` vs `0 ?? "default"`? Por que?
4. Usa el operador ternario para asignar "Aprobado" si la nota es >= 6, o "Reprobado" si no
5. Que devuelve `"5" + 3 - 1`? Explica paso a paso

<details>
<summary>Respuestas</summary>

1. `10 + 5 * 2 = 20` (multiplicacion primero). `(10 + 5) * 2 = 30` (parentesis primero).

2. `numero % 2 === 0` devuelve `true` si es par.

3. `0 || "default"` devuelve `"default"` porque `0` es falsy. `0 ?? "default"` devuelve `0` porque `??` solo reacciona a `null`/`undefined`, y `0` no es ninguno de los dos.

4. `let resultado = nota >= 6 ? "Aprobado" : "Reprobado";`

5. Paso 1: `"5" + 3` → `"53"` (concatenacion, porque hay un string). Paso 2: `"53" - 1` → `52` (resta convierte a numero). Resultado: `52`.

</details>
