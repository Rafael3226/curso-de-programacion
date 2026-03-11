# Numeros Binarios

## Que es el sistema binario?

El sistema binario es un sistema de numeracion que utiliza solo dos digitos: **0** y **1**. Es la base de toda la computacion moderna porque los circuitos electronicos trabajan con dos estados: encendido (1) y apagado (0).

Se le llama sistema **base 2**, a diferencia del sistema decimal que usamos a diario (base 10).

## Como contar en binario

En decimal, cada posicion representa una potencia de 10:

```
  345 = 3×10² + 4×10¹ + 5×10⁰
      = 300   + 40    + 5
```

En binario, cada posicion representa una potencia de 2:

```
  1101 = 1×2³ + 1×2² + 0×2¹ + 1×2⁰
       = 8    + 4    + 0    + 1
       = 13 en decimal
```

Tabla de referencia de potencias de 2:

```
Posicion:   7     6     5     4     3    2    1    0
Valor:     128   64    32    16    8    4    2    1
```

## Conversion de decimal a binario

**Metodo: division sucesiva por 2**

Para convertir 25 a binario:

```
25 ÷ 2 = 12  resto 1  ↑
12 ÷ 2 = 6   resto 0  |
 6 ÷ 2 = 3   resto 0  |  (leer de abajo hacia arriba)
 3 ÷ 2 = 1   resto 1  |
 1 ÷ 2 = 0   resto 1  |

25 en decimal = 11001 en binario
```

Verificacion: `1×16 + 1×8 + 0×4 + 0×2 + 1×1 = 16 + 8 + 1 = 25` ✓

## Conversion de binario a decimal

Multiplicar cada digito por su potencia de 2 correspondiente y sumar:

```
10110 = 1×2⁴ + 0×2³ + 1×2² + 1×2¹ + 0×2⁰
      = 16   + 0    + 4    + 2    + 0
      = 22
```

## Operaciones aritmeticas en binario

### Suma binaria

Las reglas son simples:

```
0 + 0 = 0
0 + 1 = 1
1 + 0 = 1
1 + 1 = 10  (0 y "llevo" 1)
```

Ejemplo: `1011 + 1101`

```
  Acarreo:  1 1 1
              1 0 1 1   (11)
          +   1 1 0 1   (13)
          -----------
          1 1 0 0 0     (24)
```

### Resta binaria

```
0 - 0 = 0
1 - 0 = 1
1 - 1 = 0
0 - 1 = 1  (y "pido prestado" 1)
```

## Numeros binarios negativos (Complemento a 2)

Las computadoras representan numeros negativos con **complemento a 2**:

1. Escribir el numero en binario
2. Invertir todos los bits (0→1, 1→0)
3. Sumar 1

Ejemplo: representar -5 en 8 bits:

```
 5 en binario:     0000 0101
 Invertir bits:    1111 1010
 Sumar 1:          1111 1011  ← esto es -5
```

Regla clave: si el bit mas significativo (el de la izquierda) es **1**, el numero es negativo.

Con 8 bits en complemento a 2, el rango es: **-128 a 127**.

## Por que importa el binario en programacion?

- **Operaciones a nivel de bits** (bitwise): AND, OR, XOR, NOT, shifts
- **Permisos en archivos** (ej: `chmod 755` en Linux usa representacion octal/binaria)
- **Mascaras de red** en networking (ej: `255.255.255.0`)
- **Flags y estados** compactos usando bits individuales
- **Comprender los limites** de los tipos de datos numericos

## Ejemplos en codigo

```python
# Decimal a binario
print(bin(25))        # '0b11001'
print(f"{25:08b}")    # '00011001' (con 8 digitos)

# Binario a decimal
print(int('11001', 2))  # 25

# Operaciones bitwise
a = 0b1100  # 12
b = 0b1010  # 10

print(bin(a & b))  # AND:  0b1000 (8)
print(bin(a | b))  # OR:   0b1110 (14)
print(bin(a ^ b))  # XOR:  0b0110 (6)
print(bin(~a))     # NOT: -0b1101 (-13, complemento a 2)
print(bin(a << 1)) # Shift izq: 0b11000 (24)
print(bin(a >> 1)) # Shift der: 0b110 (6)
```

```javascript
// En JavaScript
console.log((25).toString(2));    // '11001'
console.log(parseInt('11001', 2)); // 25

// Operaciones bitwise
console.log(12 & 10);  // AND: 8
console.log(12 | 10);  // OR:  14
console.log(12 ^ 10);  // XOR: 6
```

## Ejercicios

1. Convierte los siguientes numeros decimales a binario: 7, 42, 100, 255
2. Convierte los siguientes numeros binarios a decimal: `1010`, `11111`, `10000000`
3. Suma en binario: `1011 + 0110`
4. Cual es el numero mas grande que puedes representar con 8 bits (sin signo)?
5. Representa -10 en complemento a 2 usando 8 bits

<details>
<summary>Respuestas</summary>

1. 7 = `111`, 42 = `101010`, 100 = `1100100`, 255 = `11111111`
2. `1010` = 10, `11111` = 31, `10000000` = 128
3. `1011 + 0110 = 10001` (11 + 6 = 17)
4. 255 (`11111111`)
5. 10 = `00001010` → invertir: `11110101` → +1: `11110110`

</details>
