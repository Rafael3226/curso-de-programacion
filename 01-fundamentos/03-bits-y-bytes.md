# Bits y Bytes

## Que es un bit?

Un **bit** (binary digit) es la unidad minima de informacion en computacion. Solo puede tener uno de dos valores:

```
0  o  1
```

Un bit por si solo no dice mucho, pero combinando bits podemos representar cualquier tipo de dato: numeros, texto, imagenes, sonido, video.

## Que es un byte?

Un **byte** son **8 bits** agrupados. Es la unidad fundamental con la que trabajan las computadoras.

```
1 byte = 8 bits

Ejemplo de un byte: 0110 1001
```

Un byte puede representar **256 valores diferentes** (2⁸ = 256), desde `0000 0000` (0) hasta `1111 1111` (255).

## Nibble

Un **nibble** (o nybble) son **4 bits**, es decir, medio byte. Puede representar 16 valores (0-15), lo cual corresponde exactamente a un digito hexadecimal.

```
1 nibble = 4 bits = 1 digito hex
1 byte   = 2 nibbles = 2 digitos hex

Byte:    1010 1100
Nibbles: 1010  1100
Hex:       A     C    →  0xAC
```

## Unidades de almacenamiento

| Unidad    | Equivalencia         | Valor aproximado        |
|-----------|----------------------|-------------------------|
| 1 Byte    | 8 bits               | Un caracter             |
| 1 KB      | 1,024 bytes          | Un parrafo de texto     |
| 1 MB      | 1,024 KB             | Una foto de baja res    |
| 1 GB      | 1,024 MB             | Una pelicula corta      |
| 1 TB      | 1,024 GB             | ~500 horas de video     |
| 1 PB      | 1,024 TB             | Datos de una gran empresa |

### KB vs kB: la diferencia importa

Existen dos convenciones:

```
Sistema binario (IEC):          Sistema decimal (SI):
1 KiB = 1,024 bytes             1 kB = 1,000 bytes
1 MiB = 1,048,576 bytes         1 MB = 1,000,000 bytes
1 GiB = 1,073,741,824 bytes     1 GB = 1,000,000,000 bytes
```

Por eso un disco duro de "500 GB" (SI) aparece como ~465 GiB en tu sistema operativo. Los fabricantes usan base 10, los sistemas operativos suelen usar base 2.

## Cuantos bits necesito?

| Bits | Valores posibles | Rango (sin signo) | Uso comun              |
|------|------------------|--------------------|------------------------|
| 1    | 2                | 0-1                | Booleano (true/false)  |
| 4    | 16               | 0-15               | Un digito hexadecimal  |
| 8    | 256              | 0-255              | Caracter ASCII, pixel  |
| 16   | 65,536           | 0-65,535           | Entero corto           |
| 32   | ~4.3 mil millones| 0-4,294,967,295    | Entero, IP address     |
| 64   | ~1.8×10¹⁹       | 0-18,446,744,073,709,551,615 | Entero largo |

Formula general: con **n** bits puedes representar **2ⁿ** valores distintos.

## Orden de bytes (Endianness)

Cuando un dato ocupa mas de un byte, hay dos formas de ordenarlos en memoria:

**Big-endian:** el byte mas significativo va primero (como leemos numeros).

```
El numero 0x0A0B:
Direccion:  [0]  [1]
Contenido:  0A   0B
```

**Little-endian:** el byte menos significativo va primero.

```
El numero 0x0A0B:
Direccion:  [0]  [1]
Contenido:  0B   0A
```

La mayoria de las computadoras modernas (x86, ARM) usan **little-endian**. Las redes usan **big-endian** (por eso se llama "network byte order").

## Operaciones a nivel de bits (bitwise)

Las operaciones bitwise trabajan directamente sobre los bits individuales:

### AND (`&`) - Ambos deben ser 1

```
  1010
& 1100
------
  1000
```

Uso: **mascaras** para extraer bits especificos.

### OR (`|`) - Al menos uno debe ser 1

```
  1010
| 1100
------
  1110
```

Uso: **activar** bits especificos.

### XOR (`^`) - Exactamente uno debe ser 1

```
  1010
^ 1100
------
  0110
```

Uso: **alternar** bits, encriptacion simple.

### NOT (`~`) - Invertir todos los bits

```
~ 1010
------
  0101
```

### Shift izquierdo (`<<`) - Desplazar bits a la izquierda

```
1010 << 1 = 10100    (equivale a multiplicar por 2)
1010 << 2 = 101000   (equivale a multiplicar por 4)
```

### Shift derecho (`>>`) - Desplazar bits a la derecha

```
1010 >> 1 = 0101     (equivale a dividir entre 2)
1010 >> 2 = 0010     (equivale a dividir entre 4)
```

## Aplicaciones practicas

### Flags con bits

En lugar de usar multiples booleanos, puedes empaquetar flags en un solo numero:

```python
# Permisos de archivo (estilo Unix)
READ    = 0b100  # 4
WRITE   = 0b010  # 2
EXECUTE = 0b001  # 1

# Asignar permisos
permisos = READ | WRITE        # 0b110 = 6

# Verificar un permiso
puede_leer = permisos & READ   # != 0, si puede
puede_exec = permisos & EXECUTE  # == 0, no puede

# Agregar un permiso
permisos = permisos | EXECUTE  # 0b111 = 7

# Quitar un permiso
permisos = permisos & ~WRITE   # 0b101 = 5
```

### Verificar si un numero es par o impar

```python
# El ultimo bit determina si es par (0) o impar (1)
def es_par(n):
    return (n & 1) == 0

print(es_par(4))   # True  (100 & 001 = 000)
print(es_par(7))   # False (111 & 001 = 001)
```

### Swap sin variable temporal

```python
a = 5  # 0101
b = 3  # 0011

a = a ^ b  # a = 0110
b = a ^ b  # b = 0101 (valor original de a)
a = a ^ b  # a = 0011 (valor original de b)

print(a, b)  # 3, 5
```

## Ejemplos en codigo

```python
import sys

# Tamano en bytes de distintos objetos
print(sys.getsizeof(0))         # int en Python
print(sys.getsizeof("hola"))    # string
print(sys.getsizeof([1,2,3]))   # lista

# Convertir entre unidades
archivo_bytes = 5_242_880  # 5 MB
print(f"{archivo_bytes} bytes")
print(f"{archivo_bytes / 1024} KB")
print(f"{archivo_bytes / 1024 / 1024} MB")

# Contar bits necesarios para un numero
print(42 .bit_length())  # 6 (porque 42 = 101010)
```

```javascript
// Tamano en bytes de tipos tipicos
// JavaScript no expone esto directamente, pero los TypedArrays si:
const buffer = new ArrayBuffer(4);        // 4 bytes
const vista8 = new Uint8Array(buffer);    // 4 elementos de 1 byte
const vista32 = new Uint32Array(buffer);  // 1 elemento de 4 bytes

vista32[0] = 0xDEADBEEF;
console.log(vista8);  // [239, 190, 173, 222] (little-endian)
```

## Ejercicios

1. Cuantos bytes se necesitan para almacenar un numero de 0 a 1,000,000?
2. Si tienes 2 GB de RAM, cuantos bits son?
3. Realiza las siguientes operaciones: `0b1100 & 0b1010`, `0b1100 | 0b1010`, `0b1100 ^ 0b1010`
4. Usando flags de bits, representa un usuario con permisos de lectura y ejecucion (pero no escritura)
5. Un archivo pesa 3,145,728 bytes. Cuantos MB son? Cuantos MiB?

<details>
<summary>Respuestas</summary>

1. 3 bytes (2²⁴ = 16,777,216 > 1,000,000). Con 2 bytes solo llegas a 65,535.
2. 2 GB = 2 × 1,073,741,824 bytes × 8 = 17,179,869,184 bits (~17.2 mil millones)
3. `1100 & 1010 = 1000`, `1100 | 1010 = 1110`, `1100 ^ 1010 = 0110`
4. `READ | EXECUTE = 0b100 | 0b001 = 0b101 = 5`
5. 3,145,728 / 1,000,000 = 3.146 MB (SI), 3,145,728 / 1,048,576 = 3.0 MiB (IEC)

</details>
