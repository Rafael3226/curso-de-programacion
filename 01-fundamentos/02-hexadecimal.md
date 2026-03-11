# Hexadecimal

## Que es el sistema hexadecimal?

El sistema hexadecimal es un sistema de numeracion en **base 16**. Utiliza 16 simbolos:

```
0  1  2  3  4  5  6  7  8  9  A  B  C  D  E  F
```

Donde:

| Hex | Decimal | Binario |
|-----|---------|---------|
| 0   | 0       | 0000    |
| 1   | 1       | 0001    |
| 2   | 2       | 0010    |
| 3   | 3       | 0011    |
| 4   | 4       | 0100    |
| 5   | 5       | 0101    |
| 6   | 6       | 0110    |
| 7   | 7       | 0111    |
| 8   | 8       | 1000    |
| 9   | 9       | 1001    |
| A   | 10      | 1010    |
| B   | 11      | 1011    |
| C   | 12      | 1100    |
| D   | 13      | 1101    |
| E   | 14      | 1110    |
| F   | 15      | 1111    |

## Por que se usa hexadecimal?

Hexadecimal es una forma **compacta** de representar datos binarios. Cada digito hexadecimal equivale exactamente a **4 bits**:

```
Binario:      1111 0010 1010 1100
Hexadecimal:    F    2    A    C

Entonces: 1111001010101100 = 0xF2AC
```

Escribir `0xF2AC` es mucho mas legible que `1111001010101100`.

## Notacion

Diferentes lenguajes usan distintos prefijos para indicar hexadecimal:

```
0xFF      →  C, C++, Java, JavaScript, Python
#FF0000   →  CSS (colores)
\xFF      →  Secuencias de escape en strings
U+0041    →  Unicode (puntos de codigo)
```

## Conversion hexadecimal a decimal

Cada posicion representa una potencia de 16:

```
0x2F3 = 2×16² + F×16¹ + 3×16⁰
      = 2×256 + 15×16  + 3×1
      = 512   + 240    + 3
      = 755
```

## Conversion decimal a hexadecimal

Division sucesiva por 16:

```
755 ÷ 16 = 47  resto 3   (3)    ↑
 47 ÷ 16 = 2   resto 15  (F)    |  leer de abajo hacia arriba
  2 ÷ 16 = 0   resto 2   (2)    |

755 = 0x2F3
```

## Conversion hexadecimal ↔ binario

Esta es la conversion mas directa: cada digito hex = 4 bits.

**Hex a binario:**

```
0xA7 → A = 1010, 7 = 0111 → 10100111
```

**Binario a hex:** agrupar de 4 en 4 (de derecha a izquierda):

```
110101110 → 0001 1010 1110 → 1AE → 0x1AE
```

## Usos comunes en programacion

### 1. Colores (RGB)

Cada color se representa con 2 digitos hex (1 byte, rango 0-255):

```
#FF0000  →  Rojo: FF(255), Verde: 00(0), Azul: 00(0)  →  Rojo puro
#00FF00  →  Verde puro
#0000FF  →  Azul puro
#FFFFFF  →  Blanco (todos al maximo)
#000000  →  Negro (todos en cero)
#808080  →  Gris (128, 128, 128)
```

Con canal alfa (transparencia):

```
#FF000080  →  Rojo con 50% de transparencia
```

### 2. Direcciones de memoria

```
0x7FFF5FBFF8A0   ← direccion tipica en una computadora de 64 bits
```

### 3. Codigos de caracteres

```
0x41 = 65 = 'A' (en ASCII/Unicode)
0x61 = 97 = 'a'
```

### 4. Depuracion y datos raw

Los editores hexadecimales muestran archivos byte por byte:

```
Offset    Hex                                          ASCII
00000000  89 50 4E 47 0D 0A 1A 0A  00 00 00 0D 49 48 44 52  .PNG........IHDR
```

### 5. Direcciones MAC e IPv6

```
MAC:  A4:83:E7:2B:00:1F
IPv6: 2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

## Ejemplos en codigo

```python
# Decimal a hexadecimal
print(hex(255))        # '0xff'
print(f"{255:02X}")    # 'FF' (mayusculas, 2 digitos minimo)
print(f"{255:04X}")    # '00FF' (con padding)

# Hexadecimal a decimal
print(0xFF)              # 255
print(int('FF', 16))     # 255
print(int('0xFF', 16))   # 255

# Hex a binario
print(bin(0xFF))         # '0b11111111'

# Colores: extraer componentes RGB de un color hex
color = 0xE74C3C  # un rojo
rojo  = (color >> 16) & 0xFF   # 231
verde = (color >> 8) & 0xFF    # 76
azul  = color & 0xFF           # 60
print(f"R={rojo}, G={verde}, B={azul}")
```

```javascript
// Decimal a hex
console.log((255).toString(16));     // 'ff'
console.log((255).toString(16).toUpperCase()); // 'FF'

// Hex a decimal
console.log(0xFF);                   // 255
console.log(parseInt('FF', 16));     // 255

// Extraer componentes de color
const color = 0xE74C3C;
const r = (color >> 16) & 0xFF;  // 231
const g = (color >> 8) & 0xFF;   // 76
const b = color & 0xFF;          // 60
```

## Ejercicios

1. Convierte a decimal: `0x1A`, `0xFF`, `0x100`, `0xCAFE`
2. Convierte a hexadecimal: 31, 200, 1024, 65535
3. Convierte `0xDEAD` a binario
4. Que color RGB representa `#1E90FF`? (pista: es un azul conocido)
5. Si una direccion de memoria es `0x0040A000`, cuanto es en decimal?

<details>
<summary>Respuestas</summary>

1. `0x1A` = 26, `0xFF` = 255, `0x100` = 256, `0xCAFE` = 51966
2. 31 = `0x1F`, 200 = `0xC8`, 1024 = `0x400`, 65535 = `0xFFFF`
3. `0xDEAD` = `1101 1110 1010 1101`
4. R=30, G=144, B=255 → Dodger Blue
5. 4,235,264

</details>
