# Condicionales

## Que son?

Los condicionales permiten que tu programa **tome decisiones**. Ejecutan un bloque de codigo solo si se cumple una condicion.

```
Si hace frio → ponte abrigo
Si no        → sal con camiseta
```

## if

La estructura mas basica:

```javascript
let temperatura = 15;

if (temperatura < 20) {
  console.log("Hace frio, ponte abrigo");
}
```

La condicion entre parentesis se evalua como `true` o `false`. Si es `true`, se ejecuta el bloque entre `{}`.

## if...else

Dos caminos posibles:

```javascript
let edad = 16;

if (edad >= 18) {
  console.log("Puedes votar");
} else {
  console.log("No puedes votar todavia");
}
```

## if...else if...else

Multiples condiciones:

```javascript
let nota = 75;

if (nota >= 90) {
  console.log("A - Excelente");
} else if (nota >= 80) {
  console.log("B - Muy bien");
} else if (nota >= 70) {
  console.log("C - Bien");
} else if (nota >= 60) {
  console.log("D - Suficiente");
} else {
  console.log("F - Reprobado");
}
// "C - Bien"
```

JavaScript evalua las condiciones **de arriba hacia abajo** y ejecuta el primer bloque que sea `true`. Los demas se saltan.

## Condiciones compuestas

Usa operadores logicos para combinar condiciones:

```javascript
let edad = 25;
let tieneID = true;
let esMiembro = false;

// AND: ambas deben cumplirse
if (edad >= 18 && tieneID) {
  console.log("Puede entrar");
}

// OR: al menos una debe cumplirse
if (esMiembro || edad >= 21) {
  console.log("Tiene acceso VIP");
}

// NOT: invertir condicion
if (!esMiembro) {
  console.log("No es miembro");
}

// Combinaciones
if ((edad >= 18 && tieneID) || esMiembro) {
  console.log("Acceso permitido");
}
```

## Bloques de una sola linea

Si el bloque tiene una sola instruccion, puedes omitir las llaves:

```javascript
if (edad >= 18) console.log("Mayor de edad");
```

Sin embargo, **se recomienda siempre usar llaves** para evitar errores al agregar mas lineas despues.

## switch

Cuando comparas una variable contra muchos valores posibles, `switch` es mas limpio que muchos `else if`:

```javascript
let dia = "martes";

switch (dia) {
  case "lunes":
    console.log("Inicio de semana");
    break;
  case "martes":
  case "miercoles":
  case "jueves":
    console.log("Entre semana");
    break;
  case "viernes":
    console.log("Casi fin de semana!");
    break;
  case "sabado":
  case "domingo":
    console.log("Fin de semana");
    break;
  default:
    console.log("Dia no valido");
}
// "Entre semana"
```

### break es obligatorio

Sin `break`, la ejecucion "cae" al siguiente `case` (fall-through):

```javascript
let fruta = "manzana";

switch (fruta) {
  case "manzana":
    console.log("Es manzana");
    // sin break! cae al siguiente
  case "pera":
    console.log("Es pera");
    break;
  case "uva":
    console.log("Es uva");
    break;
}
// "Es manzana"
// "Es pera"    ← esto no era la intencion
```

Esto a veces se usa a proposito (como martes/miercoles/jueves en el ejemplo anterior), pero generalmente es un error. Siempre pon `break`.

### switch usa comparacion estricta

```javascript
let x = "5";

switch (x) {
  case 5:
    console.log("Numero 5");
    break;
  case "5":
    console.log("String 5");
    break;
}
// "String 5" (porque switch usa ===)
```

## Operador ternario

Para asignaciones simples basadas en condicion:

```javascript
let edad = 20;
let tipo = edad >= 18 ? "adulto" : "menor";
console.log(tipo); // "adulto"

// Tambien en template literals
console.log(`Es ${edad >= 18 ? "mayor" : "menor"} de edad`);
```

No anides ternarios, se vuelve ilegible:

```javascript
// MAL — dificil de leer
let resultado = a > b ? "mayor" : a < b ? "menor" : "igual";

// BIEN — usa if/else
let resultado;
if (a > b) {
  resultado = "mayor";
} else if (a < b) {
  resultado = "menor";
} else {
  resultado = "igual";
}
```

## Patrones comunes

### Verificar si existe un valor

```javascript
let nombre = "";

// Verificar si no es null/undefined
if (nombre !== null && nombre !== undefined) {
  console.log("Tiene valor (aunque sea vacio)");
}

// Forma corta con != (una de las pocas veces que == es util)
if (nombre != null) {
  console.log("No es null ni undefined");
}

// Verificar si tiene contenido real (truthy)
if (nombre) {
  console.log("Tiene contenido");
} else {
  console.log("Vacio, null, undefined, 0, o false");
}
```

### Asignar valor por defecto

```javascript
let config = null;
let puerto = config ?? 3000;   // 3000 (porque config es null)

let nombre = "";
let saludo = nombre || "Invitado";  // "Invitado" (porque "" es falsy)
```

### Retorno temprano (guard clauses)

En vez de anidar muchos `if`, valida primero y retorna temprano:

```javascript
// MAL — muchos niveles de anidacion
function procesarPedido(pedido) {
  if (pedido) {
    if (pedido.items.length > 0) {
      if (pedido.pagado) {
        // procesar...
      }
    }
  }
}

// BIEN — guard clauses
function procesarPedido(pedido) {
  if (!pedido) return;
  if (pedido.items.length === 0) return;
  if (!pedido.pagado) return;

  // procesar...
}
```

## Ejercicios

1. Escribe un programa que reciba un numero y diga si es positivo, negativo o cero
2. Escribe un programa que reciba una nota (0-100) y muestre la calificacion (A, B, C, D, F)
3. Usando `switch`, escribe un programa que reciba un numero del 1 al 7 y muestre el dia de la semana
4. Escribe una condicion que verifique si una persona puede conducir (edad >= 16 Y tiene licencia)
5. Reescribe con operador ternario: si `puntos > 100`, mensaje es "Ganaste", si no, "Sigue intentando"

<details>
<summary>Respuestas</summary>

1.
```javascript
let numero = -5;
if (numero > 0) {
  console.log("Positivo");
} else if (numero < 0) {
  console.log("Negativo");
} else {
  console.log("Cero");
}
```

2.
```javascript
let nota = 85;
if (nota >= 90) console.log("A");
else if (nota >= 80) console.log("B");
else if (nota >= 70) console.log("C");
else if (nota >= 60) console.log("D");
else console.log("F");
```

3.
```javascript
let num = 3;
switch (num) {
  case 1: console.log("Lunes"); break;
  case 2: console.log("Martes"); break;
  case 3: console.log("Miercoles"); break;
  case 4: console.log("Jueves"); break;
  case 5: console.log("Viernes"); break;
  case 6: console.log("Sabado"); break;
  case 7: console.log("Domingo"); break;
  default: console.log("Numero no valido");
}
```

4. `if (edad >= 16 && tieneLicencia) { console.log("Puede conducir"); }`

5. `let mensaje = puntos > 100 ? "Ganaste" : "Sigue intentando";`

</details>
