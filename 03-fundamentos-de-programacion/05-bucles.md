# Bucles (Loops)

## Que es un bucle?

Un bucle repite un bloque de codigo **mientras se cumpla una condicion**. Sin bucles, tendrias que copiar y pegar el mismo codigo muchas veces:

```javascript
// Sin bucle (terrible)
console.log(1);
console.log(2);
console.log(3);
// ... hasta 100?

// Con bucle
for (let i = 1; i <= 100; i++) {
  console.log(i);
}
```

## for

El bucle mas comun. Tiene tres partes:

```javascript
for (inicializacion; condicion; actualizacion) {
  // codigo que se repite
}
```

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
// 0, 1, 2, 3, 4
```

Desglose:

```
let i = 0    → Se ejecuta UNA vez al inicio
i < 5        → Se verifica ANTES de cada iteracion
i++          → Se ejecuta DESPUES de cada iteracion

Flujo:
i=0 → 0<5? si → ejecuta → i++ → i=1
i=1 → 1<5? si → ejecuta → i++ → i=2
i=2 → 2<5? si → ejecuta → i++ → i=3
i=3 → 3<5? si → ejecuta → i++ → i=4
i=4 → 4<5? si → ejecuta → i++ → i=5
i=5 → 5<5? no → termina
```

### Variaciones

```javascript
// Contar hacia atras
for (let i = 10; i >= 0; i--) {
  console.log(i);
}
// 10, 9, 8, ... 0

// Saltar de 2 en 2
for (let i = 0; i <= 20; i += 2) {
  console.log(i);
}
// 0, 2, 4, 6, ... 20

// Recorrer un array
const frutas = ["manzana", "pera", "uva"];
for (let i = 0; i < frutas.length; i++) {
  console.log(frutas[i]);
}
```

## while

Repite mientras la condicion sea `true`. Util cuando no sabes cuantas veces vas a repetir:

```javascript
while (condicion) {
  // codigo que se repite
}
```

```javascript
let intentos = 0;

while (intentos < 3) {
  console.log(`Intento ${intentos + 1}`);
  intentos++;
}
// Intento 1
// Intento 2
// Intento 3
```

### Cuidado con bucles infinitos

Si la condicion nunca se vuelve `false`, el programa se queda colgado:

```javascript
// BUCLE INFINITO — nunca termina!
while (true) {
  console.log("Esto nunca para");
}

// Otro error comun: olvidar actualizar la variable
let i = 0;
while (i < 5) {
  console.log(i);
  // Falta i++, nunca cambia, siempre es < 5
}
```

Si te pasa, presiona `Ctrl + C` en la terminal para detener el programa.

## do...while

Similar a `while`, pero ejecuta el bloque **al menos una vez** antes de verificar la condicion:

```javascript
do {
  // codigo (se ejecuta minimo una vez)
} while (condicion);
```

```javascript
let numero = 10;

do {
  console.log(numero);
  numero++;
} while (numero < 5);
// 10 (se ejecuta una vez aunque 10 no es < 5)
```

Util para menus o validacion de entrada donde necesitas ejecutar al menos una vez.

## for...of

Recorre los **valores** de algo iterable (arrays, strings):

```javascript
const colores = ["rojo", "verde", "azul"];

for (const color of colores) {
  console.log(color);
}
// rojo
// verde
// azul
```

Mas limpio que un `for` con indice cuando no necesitas el indice:

```javascript
// Con for clasico
for (let i = 0; i < colores.length; i++) {
  console.log(colores[i]);
}

// Con for...of (mas limpio)
for (const color of colores) {
  console.log(color);
}
```

Recorrer un string:

```javascript
for (const letra of "Hola") {
  console.log(letra);
}
// H, o, l, a
```

## for...in

Recorre las **propiedades** (keys) de un objeto:

```javascript
const persona = {
  nombre: "Ana",
  edad: 25,
  ciudad: "Madrid"
};

for (const propiedad in persona) {
  console.log(`${propiedad}: ${persona[propiedad]}`);
}
// nombre: Ana
// edad: 25
// ciudad: Madrid
```

No uses `for...in` con arrays (puede dar resultados inesperados). Usa `for...of` para arrays.

## break y continue

### break — salir del bucle inmediatamente

```javascript
for (let i = 0; i < 100; i++) {
  if (i === 5) {
    break;  // sale del bucle
  }
  console.log(i);
}
// 0, 1, 2, 3, 4
```

Ejemplo practico — buscar un elemento:

```javascript
const numeros = [4, 8, 15, 16, 23, 42];
let buscado = 16;

for (const num of numeros) {
  if (num === buscado) {
    console.log(`Encontrado: ${num}`);
    break;  // no necesito seguir buscando
  }
}
```

### continue — saltar a la siguiente iteracion

```javascript
for (let i = 0; i < 10; i++) {
  if (i % 2 !== 0) {
    continue;  // salta los impares
  }
  console.log(i);
}
// 0, 2, 4, 6, 8
```

## Bucles anidados

Un bucle dentro de otro. Cada iteracion del bucle externo ejecuta el bucle interno completo:

```javascript
for (let fila = 1; fila <= 3; fila++) {
  for (let col = 1; col <= 3; col++) {
    console.log(`(${fila}, ${col})`);
  }
}
// (1,1) (1,2) (1,3) (2,1) (2,2) (2,3) (3,1) (3,2) (3,3)
```

Ejemplo — tabla de multiplicar:

```javascript
for (let i = 1; i <= 10; i++) {
  let linea = "";
  for (let j = 1; j <= 10; j++) {
    linea += String(i * j).padStart(4);
  }
  console.log(linea);
}
```

## Patrones comunes

### Acumulador

Sumar o acumular valores:

```javascript
const precios = [10, 25, 5, 30, 15];
let total = 0;

for (const precio of precios) {
  total += precio;
}
console.log(total); // 85
```

### Encontrar maximo/minimo

```javascript
const numeros = [34, 12, 78, 5, 91, 23];
let maximo = numeros[0];

for (const num of numeros) {
  if (num > maximo) {
    maximo = num;
  }
}
console.log(maximo); // 91
```

### Filtrar elementos

```javascript
const edades = [15, 22, 8, 30, 17, 45];
const mayores = [];

for (const edad of edades) {
  if (edad >= 18) {
    mayores.push(edad);
  }
}
console.log(mayores); // [22, 30, 45]
```

### Contar ocurrencias

```javascript
const texto = "programacion";
let contadorA = 0;

for (const letra of texto) {
  if (letra === "a") {
    contadorA++;
  }
}
console.log(contadorA); // 2
```

## Cual bucle usar?

```
Situacion                              │ Bucle recomendado
───────────────────────────────────────┼───────────────────
Sabes cuantas veces repetir            │ for
Recorrer un array o string             │ for...of
Recorrer propiedades de un objeto      │ for...in
No sabes cuantas veces, pero hay       │ while
condicion de parada                    │
Necesitas ejecutar al menos una vez    │ do...while
```

## Ejercicios

1. Imprime los numeros del 1 al 50
2. Imprime solo los numeros pares del 1 al 30
3. Dado un array de numeros, calcula la suma de todos
4. Recorre un string y cuenta cuantas vocales tiene
5. Imprime la tabla de multiplicar del 7 (7x1=7, 7x2=14, ..., 7x10=70)
6. Dado un array, encuentra el numero mas pequeno
7. Usando `while`, simula una cuenta regresiva de 10 a 0 y al final imprime "Despegue!"

<details>
<summary>Respuestas</summary>

1. `for (let i = 1; i <= 50; i++) console.log(i);`

2. `for (let i = 2; i <= 30; i += 2) console.log(i);`

3.
```javascript
const nums = [4, 8, 15, 16, 23, 42];
let suma = 0;
for (const n of nums) suma += n;
console.log(suma); // 108
```

4.
```javascript
const texto = "murcielago";
let vocales = 0;
for (const c of texto) {
  if ("aeiou".includes(c)) vocales++;
}
console.log(vocales); // 5
```

5. `for (let i = 1; i <= 10; i++) console.log(\`7 x ${i} = ${7 * i}\`);`

6.
```javascript
const nums = [34, 12, 78, 5, 91];
let min = nums[0];
for (const n of nums) if (n < min) min = n;
console.log(min); // 5
```

7.
```javascript
let cuenta = 10;
while (cuenta >= 0) {
  console.log(cuenta);
  cuenta--;
}
console.log("Despegue!");
```

</details>
