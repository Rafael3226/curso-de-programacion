# Funciones

## Que es una funcion?

Una funcion es un **bloque de codigo reutilizable** al que le das un nombre. En vez de repetir el mismo codigo, lo defines una vez y lo "llamas" cuantas veces quieras.

```javascript
// Sin funcion (repeticion)
console.log("Hola, Ana!");
console.log("Hola, Pedro!");
console.log("Hola, Luis!");

// Con funcion (reutilizable)
function saludar(nombre) {
  console.log(`Hola, ${nombre}!`);
}

saludar("Ana");
saludar("Pedro");
saludar("Luis");
```

## Declaracion de funciones

### Function declaration

```javascript
function sumar(a, b) {
  return a + b;
}

let resultado = sumar(3, 5); // 8
```

Partes:
```
function sumar(a, b) {
│        │     │  │
│        │     └──┴── Parametros (entradas)
│        └── Nombre
└── Palabra clave

  return a + b;
  │      └────── Valor de retorno (salida)
  └── Devuelve el resultado y termina la funcion
}
```

### Function expression

Guardar una funcion en una variable:

```javascript
const sumar = function(a, b) {
  return a + b;
};

sumar(3, 5); // 8
```

### Arrow function (funcion flecha)

Sintaxis moderna y mas corta:

```javascript
const sumar = (a, b) => {
  return a + b;
};

// Si el cuerpo es una sola expresion, se puede acortar mas:
const sumar = (a, b) => a + b;

// Con un solo parametro, los parentesis son opcionales:
const doble = n => n * 2;

// Sin parametros:
const saludar = () => console.log("Hola");
```

## Parametros y argumentos

**Parametros** son los nombres en la definicion. **Argumentos** son los valores al llamar:

```javascript
function saludar(nombre, saludo) {  // nombre y saludo son parametros
  console.log(`${saludo}, ${nombre}!`);
}

saludar("Ana", "Buenos dias");  // "Ana" y "Buenos dias" son argumentos
```

### Parametros por defecto

```javascript
function saludar(nombre, saludo = "Hola") {
  console.log(`${saludo}, ${nombre}!`);
}

saludar("Ana");              // "Hola, Ana!"
saludar("Ana", "Buenas");   // "Buenas, Ana!"
```

### Parametros rest (...)

Recibir un numero variable de argumentos como array:

```javascript
function sumarTodos(...numeros) {
  let total = 0;
  for (const n of numeros) {
    total += n;
  }
  return total;
}

sumarTodos(1, 2, 3);        // 6
sumarTodos(10, 20, 30, 40); // 100
```

## return

`return` hace dos cosas:
1. Devuelve un valor al codigo que llamo la funcion
2. Termina la ejecucion de la funcion inmediatamente

```javascript
function dividir(a, b) {
  if (b === 0) {
    return null;  // termina aqui si b es 0
  }
  return a / b;   // solo llega aqui si b no es 0
}

console.log(dividir(10, 2));  // 5
console.log(dividir(10, 0));  // null
```

Si no hay `return`, la funcion devuelve `undefined`:

```javascript
function saludar(nombre) {
  console.log(`Hola, ${nombre}`);
  // no hay return
}

let resultado = saludar("Ana");  // imprime "Hola, Ana"
console.log(resultado);           // undefined
```

## Scope (alcance)

Las variables declaradas dentro de una funcion solo existen ahi:

```javascript
function ejemplo() {
  let interna = "Solo existo aqui";
  console.log(interna); // funciona
}

ejemplo();
// console.log(interna); // ReferenceError: interna is not defined
```

Las funciones pueden acceder a variables externas:

```javascript
let saludo = "Hola";

function saludar(nombre) {
  console.log(`${saludo}, ${nombre}`);  // puede usar 'saludo'
}

saludar("Ana"); // "Hola, Ana"
```

### Scope de bloque

`let` y `const` respetan los bloques `{}`:

```javascript
if (true) {
  let x = 10;
  const y = 20;
}
// console.log(x); // Error: x is not defined
// console.log(y); // Error: y is not defined

// var NO respeta bloques (otra razon para no usarlo)
if (true) {
  var z = 30;
}
console.log(z); // 30 (se "escapa" del bloque)
```

## Funciones como valores

En JavaScript, las funciones son **valores**. Puedes pasarlas como argumentos, guardarlas en variables, retornarlas desde otras funciones:

```javascript
// Pasar funcion como argumento (callback)
function ejecutar(fn, valor) {
  return fn(valor);
}

const doble = n => n * 2;
const triple = n => n * 3;

console.log(ejecutar(doble, 5));   // 10
console.log(ejecutar(triple, 5));  // 15
```

### Callbacks

Un **callback** es una funcion que pasas como argumento para que se ejecute despues:

```javascript
function procesarDatos(datos, callback) {
  const resultado = datos.map(callback);
  return resultado;
}

const numeros = [1, 2, 3, 4, 5];

const dobles = procesarDatos(numeros, n => n * 2);
console.log(dobles); // [2, 4, 6, 8, 10]

const textos = procesarDatos(numeros, n => `Numero: ${n}`);
console.log(textos); // ["Numero: 1", "Numero: 2", ...]
```

## Funciones puras vs impuras

### Pura: mismo input = mismo output, sin efectos secundarios

```javascript
function sumar(a, b) {
  return a + b;  // siempre devuelve lo mismo para los mismos argumentos
}
```

### Impura: depende de o modifica estado externo

```javascript
let contador = 0;

function incrementar() {
  contador++;  // modifica una variable externa
  return contador;
}

incrementar(); // 1
incrementar(); // 2 (diferente resultado cada vez)
```

Las funciones puras son mas predecibles y faciles de testear. Prefierelas cuando sea posible.

## Recursion

Una funcion que se llama a si misma:

```javascript
function factorial(n) {
  if (n <= 1) return 1;     // caso base (condicion de parada)
  return n * factorial(n - 1); // caso recursivo
}

console.log(factorial(5)); // 120 (5 * 4 * 3 * 2 * 1)
```

```
factorial(5)
  5 * factorial(4)
    4 * factorial(3)
      3 * factorial(2)
        2 * factorial(1)
          return 1        ← caso base
        return 2 * 1 = 2
      return 3 * 2 = 6
    return 4 * 6 = 24
  return 5 * 24 = 120
```

Siempre necesitas un **caso base**, o sera un bucle infinito (stack overflow).

## Patrones comunes

### Funcion de validacion

```javascript
function esEmailValido(email) {
  return email.includes("@") && email.includes(".");
}

if (esEmailValido("ana@mail.com")) {
  console.log("Email valido");
}
```

### Funcion de transformacion

```javascript
function formatearPrecio(centavos) {
  return `$${(centavos / 100).toFixed(2)}`;
}

console.log(formatearPrecio(2999)); // "$29.99"
console.log(formatearPrecio(500));  // "$5.00"
```

### Funcion que retorna funcion (closure)

```javascript
function crearSaludo(saludo) {
  return function(nombre) {
    return `${saludo}, ${nombre}!`;
  };
}

const saludarES = crearSaludo("Hola");
const saludarEN = crearSaludo("Hello");

console.log(saludarES("Ana"));    // "Hola, Ana!"
console.log(saludarEN("Ana"));    // "Hello, Ana!"
```

## Ejercicios

1. Escribe una funcion `esPar(n)` que devuelva `true` si el numero es par
2. Escribe una funcion `maximo(a, b, c)` que devuelva el mayor de tres numeros
3. Escribe una funcion `contarVocales(texto)` que cuente las vocales en un string
4. Escribe una funcion `invertirString(texto)` que devuelva el texto al reves
5. Escribe una funcion `fibonacci(n)` que devuelva el n-esimo numero de Fibonacci
6. Reescribe la funcion `esPar` como arrow function de una linea

<details>
<summary>Respuestas</summary>

1. `function esPar(n) { return n % 2 === 0; }`

2.
```javascript
function maximo(a, b, c) {
  if (a >= b && a >= c) return a;
  if (b >= c) return b;
  return c;
}
```

3.
```javascript
function contarVocales(texto) {
  let cuenta = 0;
  for (const c of texto.toLowerCase()) {
    if ("aeiou".includes(c)) cuenta++;
  }
  return cuenta;
}
```

4.
```javascript
function invertirString(texto) {
  let resultado = "";
  for (let i = texto.length - 1; i >= 0; i--) {
    resultado += texto[i];
  }
  return resultado;
}
```

5.
```javascript
function fibonacci(n) {
  if (n <= 0) return 0;
  if (n === 1) return 1;
  return fibonacci(n - 1) + fibonacci(n - 2);
}
```

6. `const esPar = n => n % 2 === 0;`

</details>
