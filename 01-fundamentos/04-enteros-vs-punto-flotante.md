# Enteros vs Punto Flotante

## Dos formas de representar numeros

Las computadoras usan dos sistemas fundamentales para almacenar numeros:

- **Enteros (integers):** numeros sin parte decimal → `42`, `-7`, `0`, `1000000`
- **Punto flotante (floating point):** numeros con parte decimal → `3.14`, `-0.001`, `2.0`

Cada uno tiene reglas, limitaciones y usos diferentes.

## Numeros enteros

### Representacion en memoria

Un entero se almacena como una secuencia directa de bits:

```
Entero 42 en 8 bits:  0010 1010
Entero -1 en 8 bits:  1111 1111  (complemento a 2)
```

### Tipos comunes de enteros

| Tipo         | Bits | Rango (con signo)                          | Rango (sin signo)       |
|--------------|------|--------------------------------------------|-------------------------|
| int8 / byte  | 8    | -128 a 127                                 | 0 a 255                 |
| int16 / short| 16   | -32,768 a 32,767                           | 0 a 65,535              |
| int32 / int  | 32   | -2,147,483,648 a 2,147,483,647             | 0 a 4,294,967,295       |
| int64 / long | 64   | -9.2×10¹⁸ a 9.2×10¹⁸                      | 0 a 1.8×10¹⁹            |

### Con signo vs sin signo

- **Con signo (signed):** usa el primer bit para indicar signo. Puede ser positivo o negativo.
- **Sin signo (unsigned):** todos los bits representan magnitud. Solo valores positivos, pero el doble de rango.

```
8 bits con signo:   -128 a 127      (un bit se "gasta" en el signo)
8 bits sin signo:      0 a 255      (todo el rango es positivo)
```

### Overflow (desbordamiento)

Cuando un entero supera su rango, ocurre un **overflow**:

```c
// En C con uint8_t (8 bits sin signo)
uint8_t x = 255;
x = x + 1;  // x ahora es 0 (volvio al inicio)

// En C con int8_t (8 bits con signo)
int8_t y = 127;
y = y + 1;  // y ahora es -128
```

```
255 = 1111 1111
+1  = 0000 0000  (el bit extra se pierde)
```

Lenguajes como Python evitan esto usando enteros de precision arbitraria.

### Aritmetica entera

La aritmetica con enteros es **exacta** (mientras no haya overflow):

```
7 + 3 = 10     ← exacto
7 * 3 = 21     ← exacto
7 / 3 = 2      ← division entera (trunca la parte decimal)
7 % 3 = 1      ← modulo (el resto)
```

## Numeros de punto flotante

### El problema

No se pueden representar todos los numeros reales con un numero finito de bits. El punto flotante es un compromiso: permite un **rango enorme** a cambio de **precision limitada**.

### Notacion cientifica: la analogia

El punto flotante funciona como la notacion cientifica:

```
Notacion cientifica:    6.022 × 10²³
                        ↑       ↑
                     mantisa  exponente

Punto flotante (base 2): 1.0101 × 2¹⁰
                          ↑        ↑
                       mantisa   exponente
```

### Estandar IEEE 754

Los numeros de punto flotante siguen el estandar **IEEE 754**:

```
Un numero = (-1)^signo × mantisa × 2^exponente
```

**Float (32 bits) - precision simple:**

```
[1 bit signo] [8 bits exponente] [23 bits mantisa]
 S             EEEEEEEE           MMMMMMMMMMMMMMMMMMMMMMM
```

- Precision: ~7 digitos decimales
- Rango: ±1.18×10⁻³⁸ a ±3.4×10³⁸

**Double (64 bits) - precision doble:**

```
[1 bit signo] [11 bits exponente] [52 bits mantisa]
 S             EEEEEEEEEEE         MMMM...52 bits...MMMM
```

- Precision: ~15-16 digitos decimales
- Rango: ±2.23×10⁻³⁰⁸ a ±1.8×10³⁰⁸

### Valores especiales

IEEE 754 define valores especiales:

```
+0.0 y -0.0    →  Cero positivo y negativo (si, son diferentes)
Infinity       →  Resultado de dividir por cero (1.0 / 0.0)
-Infinity      →  Negativo infinito
NaN            →  Not a Number (0.0 / 0.0, sqrt(-1))
```

### El problema de la precision

Este es el aspecto mas importante de entender:

```python
print(0.1 + 0.2)           # 0.30000000000000004
print(0.1 + 0.2 == 0.3)    # False!
```

Por que? Porque `0.1` en binario es una fraccion periodica infinita:

```
0.1 en decimal = 0.0001100110011001100110011... en binario (se repite infinitamente)
```

Como solo tenemos un numero finito de bits, el valor se **redondea**, y los errores se acumulan.

### Como comparar numeros flotantes

Nunca compares flotantes con `==`. Usa un **epsilon** (margen de tolerancia):

```python
# MAL
if a == b:
    ...

# BIEN
epsilon = 1e-9
if abs(a - b) < epsilon:
    ...

# En Python tambien puedes usar math.isclose
import math
print(math.isclose(0.1 + 0.2, 0.3))  # True
```

### Otros problemas de precision

**Perdida de significancia:** restar numeros muy cercanos destruye precision.

```python
a = 1.000000001
b = 1.000000000
print(a - b)  # 1.0000000827403709e-09 (deberia ser 1e-09)
```

**Absorcion:** sumar numeros de magnitudes muy diferentes.

```python
x = 1e16
print(x + 1 == x)  # True! El 1 se "pierde"
```

## Cuando usar cual?

| Usa enteros cuando...                  | Usa punto flotante cuando...            |
|----------------------------------------|-----------------------------------------|
| Necesitas precision exacta             | Necesitas numeros fraccionarios         |
| Cuentas cosas (items, indices)         | Haces calculos cientificos              |
| Manejas dinero (en centavos)           | Trabajas con mediciones fisicas         |
| Trabajas con IDs                       | Necesitas rangos muy grandes            |
| Haces operaciones a nivel de bits      | Necesitas funciones matematicas (sin, cos, sqrt) |

### El caso especial del dinero

Nunca uses punto flotante para dinero:

```python
# MAL - punto flotante
precio = 0.10
total = precio * 3
print(total)  # 0.30000000000000004

# BIEN - enteros (centavos)
precio_centavos = 10
total_centavos = precio_centavos * 3
print(total_centavos)        # 30
print(total_centavos / 100)  # 0.3

# MEJOR - libreria Decimal
from decimal import Decimal
precio = Decimal('0.10')
total = precio * 3
print(total)  # 0.30  (exacto)
```

## Comparacion lado a lado

```python
# Enteros: precision exacta
print(2 ** 100)  # 1267650600228229401496703205376 (exacto en Python)

# Flotantes: precision limitada
print(2.0 ** 100)  # 1.2676506002282294e+30 (redondeado)

# Division entera vs flotante
print(7 // 3)    # 2 (division entera)
print(7 / 3)     # 2.3333333333333335 (division flotante)

# Rango
import sys
print(sys.maxsize)          # 9223372036854775807 (int64 max)
print(sys.float_info.max)   # 1.7976931348623157e+308

# Tamano en memoria (en C/Java/Go)
# int32: siempre 4 bytes
# int64: siempre 8 bytes
# float: siempre 4 bytes
# double: siempre 8 bytes
# Python int: variable (crece segun necesite)
```

```javascript
// JavaScript: todos los numeros son float64 por defecto
console.log(0.1 + 0.2);          // 0.30000000000000004
console.log(Number.MAX_SAFE_INTEGER);  // 9007199254740991 (2^53 - 1)

// BigInt para enteros de precision arbitraria
console.log(2n ** 100n);  // 1267650600228229401496703205376n

// Enteros seguros
console.log(Number.isSafeInteger(9007199254740991));  // true
console.log(Number.isSafeInteger(9007199254740992));  // false
```

## Ejercicios

1. Cual es el rango de un entero de 16 bits con signo?
2. Por que `0.1 + 0.2 != 0.3` en la mayoria de lenguajes?
3. Tienes una tienda online. Como almacenarias un precio de $29.99?
4. Que ocurre si sumas 1 a un `uint8` que vale 255?
5. Por que `1e16 + 1 == 1e16` es `true` en punto flotante?

<details>
<summary>Respuestas</summary>

1. -32,768 a 32,767
2. Porque 0.1 y 0.2 no se pueden representar exactamente en binario (son fracciones periodicas en base 2). Los errores de redondeo se acumulan.
3. Como entero en centavos: `2999`. O usando una libreria de precision decimal (`Decimal('29.99')`). Nunca como float.
4. Overflow: el valor vuelve a 0 (`255 + 1 = 0` en 8 bits sin signo).
5. `float64` tiene ~15-16 digitos de precision. `1e16` tiene 17 digitos cuando le sumas 1, asi que el 1 se pierde por redondeo.

</details>
