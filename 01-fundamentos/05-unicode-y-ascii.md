# Unicode y ASCII

## El problema: como representar texto en una computadora?

Las computadoras solo entienden numeros (bits). Para representar texto, necesitamos una **tabla de correspondencia** que asigne un numero a cada caracter. A esto se le llama **codificacion de caracteres** (character encoding).

## ASCII (1963)

### Que es?

**ASCII** (American Standard Code for Information Interchange) fue uno de los primeros estandares. Usa **7 bits** para representar **128 caracteres**:

```
0-31:     Caracteres de control (no imprimibles)
32-126:   Caracteres imprimibles
127:      DEL (borrar)
```

### Tabla de caracteres imprimibles

```
Dec  Hex   Char  │  Dec  Hex   Char  │  Dec  Hex   Char
──────────────────┼───────────────────┼──────────────────
 32  0x20  (esp) │   64  0x40   @    │   96  0x60   `
 33  0x21   !    │   65  0x41   A    │   97  0x61   a
 34  0x22   "    │   66  0x42   B    │   98  0x62   b
 35  0x23   #    │   67  0x43   C    │   99  0x63   c
 36  0x24   $    │   68  0x44   D    │  100  0x64   d
 37  0x25   %    │   69  0x45   E    │  101  0x65   e
 38  0x26   &    │   70  0x46   F    │  102  0x66   f
 39  0x27   '    │   71  0x47   G    │  103  0x67   g
 40  0x28   (    │   72  0x48   H    │  104  0x68   h
 41  0x29   )    │   73  0x49   I    │  105  0x69   i
 42  0x2A   *    │   74  0x4A   J    │  106  0x6A   j
 43  0x2B   +    │   75  0x4B   K    │  107  0x6B   k
 44  0x2C   ,    │   76  0x4C   L    │  108  0x6C   l
 45  0x2D   -    │   77  0x4D   M    │  109  0x6D   m
 46  0x2E   .    │   78  0x4E   N    │  110  0x6E   n
 47  0x2F   /    │   79  0x4F   O    │  111  0x6F   o
 48  0x30   0    │   80  0x50   P    │  112  0x70   p
 49  0x31   1    │   81  0x51   Q    │  113  0x71   q
 50  0x32   2    │   82  0x52   R    │  114  0x72   r
 51  0x33   3    │   83  0x53   S    │  115  0x73   s
 52  0x34   4    │   84  0x54   T    │  116  0x74   t
 53  0x35   5    │   85  0x55   U    │  117  0x75   u
 54  0x36   6    │   86  0x56   V    │  118  0x76   v
 55  0x37   7    │   87  0x57   W    │  119  0x77   w
 56  0x38   8    │   88  0x58   X    │  120  0x78   x
 57  0x39   9    │   89  0x59   Y    │  121  0x79   y
 58  0x3A   :    │   90  0x5A   Z    │  122  0x7A   z
 59  0x3B   ;    │   91  0x5B   [    │  123  0x7B   {
 60  0x3C   <    │   92  0x5C   \    │  124  0x7C   |
 61  0x3D   =    │   93  0x5D   ]    │  125  0x7D   }
 62  0x3E   >    │   94  0x5E   ^    │  126  0x7E   ~
 63  0x3F   ?    │   95  0x5F   _    │
```

### Patrones utiles en ASCII

```
'A' a 'Z':  65 a 90   (0x41 a 0x5A)
'a' a 'z':  97 a 122  (0x61 a 0x7A)
'0' a '9':  48 a 57   (0x30 a 0x39)

Diferencia entre mayuscula y minuscula: 32
'A' (65) + 32 = 'a' (97)
'B' (66) + 32 = 'b' (98)

En binario, la unica diferencia es el bit 5:
'A' = 0100 0001
'a' = 0110 0001
      ^^── este bit
```

### Caracteres de control importantes

```
 0   NUL  - Caracter nulo (terminador de strings en C)
 9   TAB  - Tabulacion horizontal
10   LF   - Line Feed (nueva linea en Unix/Linux/Mac)
13   CR   - Carriage Return
          - Windows usa CR+LF (13,10) para nueva linea
27   ESC  - Escape (inicio de secuencias ANSI para colores en terminal)
```

### Limitaciones de ASCII

ASCII solo cubre el **ingles**. No incluye:

- Letras con tildes o diacriticos (a, n, u)
- Caracteres de otros alfabetos (cirílico, arabe, chino, japones)
- Simbolos matematicos, emojis, etc.

## La crisis de las codificaciones

Despues de ASCII, muchos paises crearon sus propias extensiones de 8 bits (256 caracteres):

```
Latin-1 (ISO 8859-1):  Europa occidental (n, u, e, a)
Latin-2 (ISO 8859-2):  Europa central
Windows-1252:          Extension de Microsoft para Latin-1
Shift_JIS:            Japones
GB2312:               Chino simplificado
KOI8-R:               Ruso
```

Problema: un archivo codificado en Shift_JIS se veia como basura si lo abrias con Latin-1. No habia un estandar universal.

## Unicode (1991 - presente)

### Que es?

**Unicode** es un estandar universal que asigna un numero unico (llamado **code point**) a cada caracter de cada idioma del mundo, mas simbolos, emojis, y caracteres historicos.

A la fecha, Unicode contiene mas de **154,000 caracteres** de **168 sistemas de escritura**.

### Code points

Cada caracter tiene un **code point** escrito como `U+XXXX`:

```
U+0041  →  A
U+00F1  →  n (ene)
U+4E16  →  世 (mundo en chino)
U+1F600 →  😀
U+2764  →  ❤
```

Los code points van de `U+0000` a `U+10FFFF` (1,114,112 posiciones posibles).

### Planos de Unicode

```
Plano 0:   U+0000  a U+FFFF    BMP (Basic Multilingual Plane) - mayoria de idiomas
Plano 1:   U+10000 a U+1FFFF   Emojis, escrituras historicas, simbolos musicales
Plano 2:   U+20000 a U+2FFFF   Caracteres CJK adicionales
...
Plano 16:  U+100000 a U+10FFFF Uso privado
```

Los primeros 128 code points de Unicode (U+0000 a U+007F) son **identicos a ASCII**. Es retrocompatible.

## Codificaciones de Unicode (UTF)

Unicode define los numeros. Las **codificaciones** definen como se almacenan esos numeros en bytes.

### UTF-8 (la mas usada)

Codificacion de **longitud variable** que usa de 1 a 4 bytes:

```
Code point          Bytes   Patron binario
U+0000 a U+007F    1 byte  0xxxxxxx                              (ASCII!)
U+0080 a U+07FF    2 bytes 110xxxxx 10xxxxxx
U+0800 a U+FFFF    3 bytes 1110xxxx 10xxxxxx 10xxxxxx
U+10000 a U+10FFFF 4 bytes 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

Ejemplo: como se codifica `n` (U+00F1) en UTF-8:

```
U+00F1 = 1111 0001 en binario (cabe en 2 bytes: 110xxxxx 10xxxxxx)

110 00011  10 110001
 C    3      B   1
= 0xC3 0xB1 (2 bytes)
```

Ventajas de UTF-8:
- Compatible con ASCII (textos ASCII son identicos en UTF-8)
- No tiene problemas de endianness
- Es la codificacion dominante en la web (98%+ de paginas web)

### UTF-16

Codificacion de **longitud variable** que usa 2 o 4 bytes:

```
BMP (U+0000 a U+FFFF):     2 bytes directos
Fuera del BMP:              4 bytes (par surrogado)
```

Es la codificacion interna de **JavaScript** y **Java**.

```javascript
// JavaScript usa UTF-16 internamente
"😀".length;     // 2 (porque usa un par surrogado)
[..."😀"].length; // 1 (usando iterador Unicode)
```

### UTF-32

Codificacion de **longitud fija**: siempre 4 bytes por caracter.

- Simple pero desperdicia espacio para texto en idiomas occidentales
- Usada internamente en algunos sistemas para facilitar el acceso por indice

### Comparacion

```
Caracter   UTF-8           UTF-16          UTF-32
'A'        41              00 41           00 00 00 41
'n'        C3 B1           00 F1           00 00 00 F1
'世'       E4 B8 96        4E 16           00 00 4E 16
'😀'       F0 9F 98 80     D8 3D DE 00     00 01 F6 00
```

## Problemas comunes

### Mojibake (texto corrupto)

Cuando abres un archivo con la codificacion equivocada:

```
Texto original (UTF-8):     "Cancion"
Abierto como Latin-1:       "CancioÌn"
```

### BOM (Byte Order Mark)

Algunos editores agregan un caracter invisible `U+FEFF` al inicio del archivo para indicar la codificacion. Puede causar errores en scripts.

### Normalizacion

Un mismo caracter puede representarse de multiples formas:

```
'n' puede ser:
  - U+00F1              (un solo code point: "n con tilde")
  - U+006E + U+0303     (dos code points: "n" + "tilde combinante")

Ambos se ven igual pero son secuencias de bytes diferentes!
```

Por eso es importante **normalizar** antes de comparar strings:

```python
import unicodedata

s1 = '\u00f1'           # n como un solo caracter
s2 = 'n\u0303'          # n + tilde combinante

print(s1 == s2)          # False!
print(s1, s2)            # se ven igual

# Normalizar
s1_norm = unicodedata.normalize('NFC', s1)
s2_norm = unicodedata.normalize('NFC', s2)
print(s1_norm == s2_norm)  # True
```

### Longitud de strings

```python
# Un emoji puede ocupar multiples bytes pero ser "un caracter"
texto = "Hola 😀"
print(len(texto))                              # 6 (code points)
print(len(texto.encode('utf-8')))              # 10 (bytes)
print(len(texto.encode('utf-16-le')))          # 14 (bytes)

# Grafemas vs code points
familia = "👨‍👩‍👧‍👦"  # 1 "caracter" visual, pero...
print(len(familia))        # 11 (7 code points + 4 zero-width joiners... depende del lenguaje)
```

## Ejemplos en codigo

```python
# ASCII
print(ord('A'))          # 65
print(chr(65))           # 'A'
print(ord('a') - ord('A'))  # 32

# Verificar si es ASCII
print('Hello'.isascii())      # True
print('Hola!'.isascii())      # False (ñ no es ASCII)... wait
# Nota: "Hola!" SI es ASCII. "Hola niño" NO seria ASCII por la ñ.

# Unicode
print(ord('n'))                    # 241 (U+00F1)
print('\u00F1')                     # n
print('n'.encode('utf-8'))    # b'\xc3\xb1' (2 bytes)
print('A'.encode('utf-8'))          # b'A' (1 byte, igual que ASCII)
print('😀'.encode('utf-8'))       # b'\xf0\x9f\x98\x80' (4 bytes)

# Decodificar bytes
datos = b'\xc3\xb1'
print(datos.decode('utf-8'))    # n
print(datos.decode('latin-1'))  # Ã± (mojibake!)

# Listar propiedades de un caracter
import unicodedata
print(unicodedata.name('n'))    # LATIN SMALL LETTER N WITH TILDE
print(unicodedata.name('😀'))  # GRINNING FACE
print(unicodedata.category('A'))  # Lu (Letter, uppercase)
print(unicodedata.category('3'))  # Nd (Number, decimal digit)
```

```javascript
// Code points
console.log('A'.codePointAt(0));       // 65
console.log(String.fromCodePoint(65)); // 'A'
console.log('😀'.codePointAt(0));     // 128512

// Longitud real con iterador
console.log('😀'.length);          // 2 (UTF-16 units)
console.log([...'😀'].length);     // 1 (code points)

// Codificar/decodificar
const encoder = new TextEncoder();
const decoder = new TextDecoder();

const bytes = encoder.encode('Hola');        // UTF-8 bytes
console.log(bytes);                           // Uint8Array [72, 111, 108, 97]
console.log(decoder.decode(bytes));           // 'Hola'

const bytesN = encoder.encode('n');
console.log(bytesN);                          // Uint8Array [195, 177] (0xC3 0xB1)
```

## Resumen rapido

```
ASCII    →  128 caracteres, 7 bits, solo ingles
Unicode  →  154,000+ caracteres, todos los idiomas
UTF-8    →  Codificacion variable (1-4 bytes), dominante en la web
UTF-16   →  Codificacion variable (2-4 bytes), usada en JS/Java
UTF-32   →  Codificacion fija (4 bytes), simple pero ineficiente
```

Regla de oro: **usa siempre UTF-8** a menos que tengas una razon especifica para no hacerlo.

## Ejercicios

1. Cual es el valor ASCII de 'Z'? Y de 'z'?
2. Cuantos bytes ocupa la palabra "cafe" en UTF-8? (pista: la 'e' con tilde no es ASCII)
3. Por que `"😀".length` da 2 en JavaScript?
4. Un archivo de texto se ve asi: `CafÃ©`. Que paso?
5. Escribe codigo que convierta una cadena de mayusculas a minusculas usando solo operaciones de bits

<details>
<summary>Respuestas</summary>

1. 'Z' = 90 (0x5A), 'z' = 122 (0x7A)
2. 'c' = 1 byte, 'a' = 1 byte, 'f' = 1 byte, 'e' (U+00E9) = 2 bytes → **5 bytes** total
3. Porque JavaScript usa UTF-16 internamente. El emoji U+1F600 esta fuera del BMP y necesita un **par surrogado** (2 unidades de 16 bits).
4. El archivo fue guardado en UTF-8 pero abierto con Latin-1. 'e' en UTF-8 es `0xC3 0xA9`, que en Latin-1 se lee como 'Ã' y '©'.
5. `caracter | 0x20` convierte mayuscula a minuscula (activa el bit 5): `'A' | 32 = 'a'`

</details>
