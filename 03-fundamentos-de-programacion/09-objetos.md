# Objetos

## Que es un objeto?

Un objeto es una coleccion de **pares clave-valor**. Mientras que un array organiza datos por posicion (indice), un objeto los organiza por **nombre** (propiedad).

```javascript
const persona = {
  nombre: "Ana",
  edad: 25,
  ciudad: "Madrid"
};
```

Un objeto modela una "cosa" con sus caracteristicas (propiedades) y comportamientos (metodos).

## Crear objetos

```javascript
// Objeto literal (la forma mas comun)
const auto = {
  marca: "Toyota",
  modelo: "Corolla",
  ano: 2023,
  color: "rojo"
};

// Objeto vacio
const vacio = {};
```

## Acceder a propiedades

Dos formas: punto y corchetes.

```javascript
const persona = {
  nombre: "Ana",
  edad: 25,
  "correo electronico": "ana@mail.com"
};

// Notacion de punto
console.log(persona.nombre);   // "Ana"
console.log(persona.edad);     // 25

// Notacion de corchetes (necesaria para claves con espacios o variables)
console.log(persona["correo electronico"]); // "ana@mail.com"

// Acceder con una variable
const campo = "nombre";
console.log(persona[campo]);   // "Ana"

// Propiedad que no existe
console.log(persona.telefono); // undefined
```

## Modificar objetos

```javascript
const persona = { nombre: "Ana", edad: 25 };

// Modificar propiedad existente
persona.edad = 26;

// Agregar nueva propiedad
persona.ciudad = "Madrid";
persona["profesion"] = "Ingeniera";

// Eliminar propiedad
delete persona.ciudad;

console.log(persona);
// { nombre: "Ana", edad: 26, profesion: "Ingeniera" }
```

Aunque sea `const`, puedes modificar las propiedades. `const` impide reasignar la variable, no mutar el contenido (igual que con arrays).

## Verificar si una propiedad existe

```javascript
const persona = { nombre: "Ana", edad: 25 };

// Operador in
"nombre" in persona;     // true
"telefono" in persona;   // false

// Comparar con undefined
persona.nombre !== undefined;   // true
persona.telefono !== undefined; // false

// hasOwnProperty
persona.hasOwnProperty("nombre"); // true
```

## Metodos (funciones dentro de objetos)

```javascript
const calculadora = {
  sumar(a, b) {
    return a + b;
  },
  restar(a, b) {
    return a - b;
  },
  multiplicar(a, b) {
    return a * b;
  }
};

console.log(calculadora.sumar(5, 3));       // 8
console.log(calculadora.multiplicar(4, 2)); // 8
```

### this — referencia al objeto actual

```javascript
const usuario = {
  nombre: "Ana",
  edad: 25,
  presentarse() {
    console.log(`Hola, soy ${this.nombre} y tengo ${this.edad} anos`);
  }
};

usuario.presentarse(); // "Hola, soy Ana y tengo 25 anos"
```

`this` se refiere al objeto que llama al metodo. Cuidado: en arrow functions, `this` no se comporta igual (hereda el `this` del contexto exterior).

## Recorrer objetos

```javascript
const persona = { nombre: "Ana", edad: 25, ciudad: "Madrid" };

// Object.keys — array con las claves
console.log(Object.keys(persona));   // ["nombre", "edad", "ciudad"]

// Object.values — array con los valores
console.log(Object.values(persona)); // ["Ana", 25, "Madrid"]

// Object.entries — array de pares [clave, valor]
console.log(Object.entries(persona));
// [["nombre", "Ana"], ["edad", 25], ["ciudad", "Madrid"]]

// for...in
for (const clave in persona) {
  console.log(`${clave}: ${persona[clave]}`);
}

// Con entries y desestructuracion
for (const [clave, valor] of Object.entries(persona)) {
  console.log(`${clave}: ${valor}`);
}
```

## Desestructuracion de objetos

Extraer propiedades a variables de forma limpia:

```javascript
const persona = { nombre: "Ana", edad: 25, ciudad: "Madrid" };

// Sin desestructuracion
const nombre = persona.nombre;
const edad = persona.edad;

// Con desestructuracion
const { nombre, edad, ciudad } = persona;
console.log(nombre); // "Ana"
console.log(edad);   // 25

// Renombrar
const { nombre: n, edad: e } = persona;
console.log(n); // "Ana"

// Valores por defecto
const { telefono = "Sin telefono" } = persona;
console.log(telefono); // "Sin telefono"

// Rest
const { nombre: nom, ...resto } = persona;
console.log(nom);   // "Ana"
console.log(resto);  // { edad: 25, ciudad: "Madrid" }
```

### En parametros de funciones

```javascript
function mostrarUsuario({ nombre, edad, ciudad = "Desconocida" }) {
  console.log(`${nombre}, ${edad} anos, ${ciudad}`);
}

mostrarUsuario({ nombre: "Ana", edad: 25 });
// "Ana, 25 anos, Desconocida"
```

## Spread operator con objetos

```javascript
const base = { a: 1, b: 2 };

// Copiar un objeto
const copia = { ...base };

// Combinar objetos
const extra = { c: 3, d: 4 };
const combinado = { ...base, ...extra };
// { a: 1, b: 2, c: 3, d: 4 }

// Sobreescribir propiedades
const actualizado = { ...base, b: 99 };
// { a: 1, b: 99 }
```

## Optional chaining (?.)

Acceder a propiedades anidadas sin que falle si algo es `null`/`undefined`:

```javascript
const usuario = {
  nombre: "Ana",
  direccion: {
    ciudad: "Madrid"
  }
};

console.log(usuario.direccion.ciudad);    // "Madrid"
console.log(usuario.contacto?.telefono);  // undefined (sin error)

// Sin optional chaining:
// console.log(usuario.contacto.telefono); // TypeError!

// Encadenado
console.log(usuario?.direccion?.calle?.numero); // undefined
```

## Objetos anidados

```javascript
const empresa = {
  nombre: "TechCorp",
  direccion: {
    calle: "Gran Via",
    numero: 123,
    ciudad: "Madrid"
  },
  empleados: [
    { nombre: "Ana", puesto: "Dev" },
    { nombre: "Pedro", puesto: "Design" }
  ]
};

console.log(empresa.direccion.ciudad);     // "Madrid"
console.log(empresa.empleados[0].nombre);  // "Ana"
console.log(empresa.empleados.length);     // 2
```

## Comparacion de objetos

Los objetos se comparan **por referencia**, no por contenido:

```javascript
const a = { x: 1 };
const b = { x: 1 };
const c = a;

console.log(a === b);  // false (son objetos diferentes en memoria)
console.log(a === c);  // true (c apunta al mismo objeto que a)

// Para comparar contenido, convierte a string:
JSON.stringify(a) === JSON.stringify(b); // true
```

## Copias superficiales vs profundas

```javascript
const original = {
  nombre: "Ana",
  hobbies: ["leer", "correr"]
};

// Copia superficial (shallow) — los objetos/arrays internos se comparten
const copia = { ...original };
copia.nombre = "Pedro";          // OK, no afecta al original
copia.hobbies.push("nadar");    // CUIDADO: modifica el original tambien!

console.log(original.hobbies);  // ["leer", "correr", "nadar"]

// Copia profunda (deep)
const copiaReal = structuredClone(original);
copiaReal.hobbies.push("yoga");
console.log(original.hobbies);  // No cambia
```

## Patrones comunes

### Objeto como diccionario/mapa

```javascript
const precios = {
  manzana: 1.5,
  pera: 2.0,
  uva: 3.5
};

console.log(precios["manzana"]); // 1.5

// Verificar si existe
if ("kiwi" in precios) {
  console.log(precios["kiwi"]);
}
```

### Contar frecuencias

```javascript
function contarPalabras(texto) {
  const frecuencia = {};
  for (const palabra of texto.split(" ")) {
    frecuencia[palabra] = (frecuencia[palabra] ?? 0) + 1;
  }
  return frecuencia;
}

contarPalabras("hola mundo hola");
// { hola: 2, mundo: 1 }
```

### Shorthand properties

```javascript
const nombre = "Ana";
const edad = 25;

// Sin shorthand
const persona = { nombre: nombre, edad: edad };

// Con shorthand (si variable y propiedad se llaman igual)
const persona2 = { nombre, edad };
```

## Ejercicios

1. Crea un objeto `libro` con propiedades: titulo, autor, paginas, leido (boolean)
2. Escribe una funcion que reciba un objeto persona y devuelva "Nombre tiene X anos"
3. Dado un array de objetos `[{nombre, nota}, ...]`, filtra los que tengan nota >= 6 y devuelve solo los nombres
4. Escribe una funcion que cuente la frecuencia de cada caracter en un string
5. Escribe una funcion `merge(obj1, obj2)` que combine dos objetos (obj2 sobreescribe en caso de conflicto)

<details>
<summary>Respuestas</summary>

1.
```javascript
const libro = {
  titulo: "El principito",
  autor: "Saint-Exupery",
  paginas: 96,
  leido: true
};
```

2.
```javascript
function describir({ nombre, edad }) {
  return `${nombre} tiene ${edad} anos`;
}
```

3.
```javascript
const aprobados = estudiantes
  .filter(e => e.nota >= 6)
  .map(e => e.nombre);
```

4.
```javascript
function frecuenciaCaracteres(str) {
  const freq = {};
  for (const c of str) {
    freq[c] = (freq[c] ?? 0) + 1;
  }
  return freq;
}
```

5. `const merge = (obj1, obj2) => ({ ...obj1, ...obj2 });`

</details>
