# Arrays

## Que es un array?

Un array es una **lista ordenada de valores**. Permite almacenar multiples elementos en una sola variable.

```javascript
const frutas = ["manzana", "pera", "uva"];
const numeros = [10, 20, 30, 40, 50];
const mixto = [1, "hola", true, null]; // puede mezclar tipos
```

## Acceso por indice

Los indices empiezan en **0**:

```javascript
const colores = ["rojo", "verde", "azul"];
//                 [0]     [1]      [2]

console.log(colores[0]);   // "rojo"
console.log(colores[1]);   // "verde"
console.log(colores[2]);   // "azul"
console.log(colores[3]);   // undefined (no existe)

// Ultimo elemento
console.log(colores[colores.length - 1]); // "azul"

// Metodo moderno (ES2022)
console.log(colores.at(-1));  // "azul" (indice negativo = desde el final)
console.log(colores.at(-2));  // "verde"
```

## Propiedades basicas

```javascript
const nums = [10, 20, 30];

console.log(nums.length);  // 3 (cantidad de elementos)

// Modificar un elemento
nums[1] = 99;
console.log(nums); // [10, 99, 30]

// Aunque sea const, puedes modificar el contenido
// const impide reasignar la variable, no mutar el array
// nums = [1, 2, 3]; // Error: Assignment to constant variable
```

## Agregar y quitar elementos

```javascript
const arr = [1, 2, 3];

// Al final
arr.push(4);           // [1, 2, 3, 4]
arr.push(5, 6);        // [1, 2, 3, 4, 5, 6]

// Al inicio
arr.unshift(0);        // [0, 1, 2, 3, 4, 5, 6]

// Quitar del final
const ultimo = arr.pop();     // ultimo = 6, arr = [0, 1, 2, 3, 4, 5]

// Quitar del inicio
const primero = arr.shift();  // primero = 0, arr = [1, 2, 3, 4, 5]
```

## Metodos esenciales

### Buscar

```javascript
const frutas = ["manzana", "pera", "uva", "pera"];

frutas.includes("pera");          // true
frutas.includes("kiwi");          // false

frutas.indexOf("pera");           // 1 (primera aparicion)
frutas.lastIndexOf("pera");       // 3 (ultima aparicion)
frutas.indexOf("kiwi");           // -1 (no encontrado)

// Buscar con condicion
const nums = [5, 12, 8, 130, 44];
nums.find(n => n > 10);           // 12 (primer elemento que cumple)
nums.findIndex(n => n > 10);      // 1 (indice del primero que cumple)
```

### Verificar condiciones

```javascript
const edades = [22, 18, 30, 15, 25];

// Todos cumplen la condicion?
edades.every(e => e >= 18);       // false (15 no cumple)

// Al menos uno cumple?
edades.some(e => e >= 18);        // true
```

### Cortar y combinar

```javascript
const letras = ["a", "b", "c", "d", "e"];

// slice: extraer porcion (no modifica el original)
letras.slice(1, 3);       // ["b", "c"] (indice 1 hasta 3, sin incluir 3)
letras.slice(2);           // ["c", "d", "e"] (desde indice 2 hasta el final)
letras.slice(-2);          // ["d", "e"] (ultimos 2)

// splice: insertar/eliminar en posicion (SI modifica el original)
const arr = [1, 2, 3, 4, 5];
arr.splice(2, 1);          // elimina 1 elemento en indice 2 → arr = [1, 2, 4, 5]
arr.splice(1, 0, 99);      // inserta 99 en indice 1 → arr = [1, 99, 2, 4, 5]

// concat: combinar arrays (no modifica los originales)
const a = [1, 2];
const b = [3, 4];
const c = a.concat(b);     // [1, 2, 3, 4]

// spread operator (forma moderna)
const d = [...a, ...b];    // [1, 2, 3, 4]
const e = [...a, 99, ...b]; // [1, 2, 99, 3, 4]
```

### Ordenar

```javascript
// Orden alfabetico
const nombres = ["Carlos", "Ana", "Beto"];
nombres.sort();  // ["Ana", "Beto", "Carlos"]

// Cuidado con numeros:
const nums = [10, 1, 21, 2];
nums.sort();     // [1, 10, 2, 21] — ordena como STRINGS!

// Orden numerico correcto
nums.sort((a, b) => a - b);  // [1, 2, 10, 21] (ascendente)
nums.sort((a, b) => b - a);  // [21, 10, 2, 1] (descendente)

// Invertir orden
const arr = [1, 2, 3];
arr.reverse();   // [3, 2, 1]
```

### Convertir a string

```javascript
const frutas = ["manzana", "pera", "uva"];

frutas.join(", ");   // "manzana, pera, uva"
frutas.join(" - ");  // "manzana - pera - uva"
frutas.join("");     // "manzanaperauva"
```

## Los metodos mas poderosos: map, filter, reduce

Estos tres metodos son fundamentales en JavaScript moderno.

### map — transformar cada elemento

Crea un **nuevo array** aplicando una funcion a cada elemento:

```javascript
const numeros = [1, 2, 3, 4, 5];

const dobles = numeros.map(n => n * 2);
console.log(dobles); // [2, 4, 6, 8, 10]

const nombres = ["ana", "pedro", "luis"];
const mayusculas = nombres.map(n => n.toUpperCase());
console.log(mayusculas); // ["ANA", "PEDRO", "LUIS"]

// map recibe: (elemento, indice, array)
const indexados = nombres.map((nombre, i) => `${i}: ${nombre}`);
console.log(indexados); // ["0: ana", "1: pedro", "2: luis"]
```

### filter — seleccionar elementos

Crea un **nuevo array** con los elementos que cumplan una condicion:

```javascript
const numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const pares = numeros.filter(n => n % 2 === 0);
console.log(pares); // [2, 4, 6, 8, 10]

const mayoresA5 = numeros.filter(n => n > 5);
console.log(mayoresA5); // [6, 7, 8, 9, 10]

// Filtrar strings
const palabras = ["hola", "a", "mundo", "de", "programacion"];
const largas = palabras.filter(p => p.length > 3);
console.log(largas); // ["hola", "mundo", "programacion"]
```

### reduce — reducir a un solo valor

Acumula todos los elementos en un **unico resultado**:

```javascript
const numeros = [1, 2, 3, 4, 5];

// Sumar todos
const suma = numeros.reduce((acumulador, actual) => acumulador + actual, 0);
console.log(suma); // 15

// Encontrar el maximo
const max = numeros.reduce((acc, n) => n > acc ? n : acc, numeros[0]);
console.log(max); // 5
```

`reduce` recibe:
```
array.reduce((acumulador, elementoActual) => {
  return nuevoAcumulador;
}, valorInicial);
```

```
Paso a paso con suma y valor inicial 0:
acc=0, actual=1 → 0+1 = 1
acc=1, actual=2 → 1+2 = 3
acc=3, actual=3 → 3+3 = 6
acc=6, actual=4 → 6+4 = 10
acc=10, actual=5 → 10+5 = 15
```

### Encadenar metodos

```javascript
const personas = [
  { nombre: "Ana", edad: 28 },
  { nombre: "Pedro", edad: 16 },
  { nombre: "Luis", edad: 35 },
  { nombre: "Maria", edad: 14 },
  { nombre: "Carlos", edad: 22 }
];

// Obtener los nombres de los mayores de edad, en mayusculas
const resultado = personas
  .filter(p => p.edad >= 18)
  .map(p => p.nombre.toUpperCase())
  .sort();

console.log(resultado); // ["ANA", "CARLOS", "LUIS"]
```

## forEach

Ejecuta una funcion por cada elemento, pero **no devuelve un array nuevo**:

```javascript
const frutas = ["manzana", "pera", "uva"];

frutas.forEach((fruta, i) => {
  console.log(`${i}: ${fruta}`);
});
// 0: manzana
// 1: pera
// 2: uva
```

Diferencia con `map`: `forEach` no retorna nada. Usalo solo para efectos secundarios (imprimir, guardar, etc.). Si necesitas un array nuevo, usa `map`.

## Desestructuracion de arrays

```javascript
const coordenadas = [10, 20, 30];

// Sin desestructuracion
const x = coordenadas[0];
const y = coordenadas[1];

// Con desestructuracion
const [a, b, c] = coordenadas;
console.log(a, b, c); // 10 20 30

// Saltar elementos
const [primero, , tercero] = coordenadas;
console.log(primero, tercero); // 10 30

// Rest
const [head, ...rest] = [1, 2, 3, 4, 5];
console.log(head); // 1
console.log(rest); // [2, 3, 4, 5]
```

## Ejercicios

1. Dado `[3, 7, 2, 9, 1, 5]`, encuentra el numero mas grande usando `reduce`
2. Dado `["hola", "mundo", "javascript"]`, crea un nuevo array con la longitud de cada palabra
3. Dado `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`, filtra los impares y duplica cada uno
4. Dado un array de nombres, filtra los que tengan mas de 4 letras y unelos con comas
5. Escribe una funcion `aplanar` que convierta `[[1, 2], [3, 4], [5]]` en `[1, 2, 3, 4, 5]`

<details>
<summary>Respuestas</summary>

1. `[3, 7, 2, 9, 1, 5].reduce((max, n) => n > max ? n : max, -Infinity); // 9`

2. `["hola", "mundo", "javascript"].map(p => p.length); // [4, 5, 10]`

3.
```javascript
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  .filter(n => n % 2 !== 0)
  .map(n => n * 2);
// [2, 6, 10, 14, 18]
```

4.
```javascript
["Ana", "Pedro", "Luis", "Maria", "Jo"]
  .filter(n => n.length > 4)
  .join(", ");
// "Pedro, Maria"
```

5.
```javascript
function aplanar(arr) {
  return arr.reduce((acc, sub) => [...acc, ...sub], []);
}
// O simplemente: arr.flat()
```

</details>
