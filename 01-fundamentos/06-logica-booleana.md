# Logica Booleana y Compuertas Logicas

## Que es la logica booleana?

La logica booleana es un sistema matematico que trabaja con solo dos valores: **verdadero (1)** y **falso (0)**. Fue creada por George Boole en 1854 y es la base de todo circuito digital y toda decision que toma una computadora.

Cada operacion dentro de un procesador, desde sumar numeros hasta mostrar un pixel en pantalla, se reduce a combinaciones de operaciones booleanas.

## Operaciones fundamentales

### NOT (negacion)

Invierte el valor. Si es 1, devuelve 0. Si es 0, devuelve 1.

```
Entrada │ Salida
────────┼───────
   0    │   1
   1    │   0
```

Simbolo: `¬A` o `!A` o `A'`

Analogia: un interruptor invertido. Si la luz esta encendida, la apaga. Si esta apagada, la enciende.

### AND (conjuncion)

Devuelve 1 **solo si ambas** entradas son 1.

```
  A  │  B  │ A AND B
─────┼─────┼────────
  0  │  0  │    0
  0  │  1  │    0
  1  │  0  │    0
  1  │  1  │    1
```

Simbolo: `A ∧ B` o `A · B` o `A & B`

Analogia: dos interruptores **en serie**. La corriente solo pasa si **ambos** estan cerrados.

```
 ──[A]──[B]──💡
Solo enciende si A Y B estan cerrados.
```

### OR (disyuncion)

Devuelve 1 si **al menos una** entrada es 1.

```
  A  │  B  │ A OR B
─────┼─────┼───────
  0  │  0  │   0
  0  │  1  │   1
  1  │  0  │   1
  1  │  1  │   1
```

Simbolo: `A ∨ B` o `A + B` o `A | B`

Analogia: dos interruptores **en paralelo**. La corriente pasa si **cualquiera** esta cerrado.

```
 ──┬─[A]─┬──💡
   └─[B]─┘
Enciende si A O B (o ambos) estan cerrados.
```

### XOR (disyuncion exclusiva)

Devuelve 1 si **exactamente una** entrada es 1 (pero no ambas).

```
  A  │  B  │ A XOR B
─────┼─────┼────────
  0  │  0  │    0
  0  │  1  │    1
  1  │  0  │    1
  1  │  1  │    0
```

Simbolo: `A ⊕ B` o `A ^ B`

Analogia: un interruptor de escalera (los que hay al inicio y al final de una escalera). La luz cambia de estado cuando mueves **cualquiera** de los dos.

## Compuertas derivadas

### NAND (NOT AND)

Es un AND seguido de NOT. Devuelve 0 solo si ambas entradas son 1.

```
  A  │  B  │ A NAND B
─────┼─────┼─────────
  0  │  0  │    1
  0  │  1  │    1
  1  │  0  │    1
  1  │  1  │    0
```

Dato importante: **NAND es universal**. Con solo compuertas NAND se puede construir cualquier otra compuerta (AND, OR, NOT, XOR). Por eso los circuitos reales se fabrican frecuentemente usando solo NAND.

### NOR (NOT OR)

Es un OR seguido de NOT. Devuelve 1 solo si ambas entradas son 0.

```
  A  │  B  │ A NOR B
─────┼─────┼────────
  0  │  0  │    1
  0  │  1  │    0
  1  │  0  │    0
  1  │  1  │    0
```

NOR tambien es **universal**, igual que NAND.

## Leyes del algebra booleana

Estas leyes permiten simplificar expresiones logicas:

```
Identidad:       A AND 1 = A          A OR 0 = A
Dominacion:      A AND 0 = 0          A OR 1 = 1
Idempotencia:    A AND A = A          A OR A = A
Complemento:     A AND (NOT A) = 0    A OR (NOT A) = 1
Doble negacion:  NOT (NOT A) = A

Conmutativa:     A AND B = B AND A
                 A OR B  = B OR A

Asociativa:      (A AND B) AND C = A AND (B AND C)
                 (A OR B) OR C   = A OR (B OR C)

Distributiva:    A AND (B OR C)  = (A AND B) OR (A AND C)
                 A OR (B AND C)  = (A OR B) AND (A OR C)

Leyes de De Morgan:
    NOT (A AND B) = (NOT A) OR (NOT B)
    NOT (A OR B)  = (NOT A) AND (NOT B)
```

Las **leyes de De Morgan** son especialmente utiles. Dicen que:
- Negar un AND es lo mismo que hacer OR de las negaciones
- Negar un OR es lo mismo que hacer AND de las negaciones

## Compuertas logicas en circuitos reales

Dentro de un procesador hay **miles de millones** de transistores. Un transistor es basicamente un interruptor microscopico que se abre o cierra con una senal electrica.

```
Transistor:
  - Voltaje alto (~5V o ~3.3V) → 1 (encendido)
  - Voltaje bajo (~0V)         → 0 (apagado)
```

Combinando transistores se forman compuertas:

```
NOT  → 1 transistor
AND  → 2 transistores en serie + inversor
OR   → 2 transistores en paralelo + inversor
NAND → 2 transistores (la mas simple de fabricar)
```

## Como se construye un sumador con compuertas

Un **half adder** (medio sumador) suma dos bits:

```
Entradas: A, B
Salidas:  Suma (S), Acarreo (C)

A │ B │ S │ C
──┼───┼───┼──
0 │ 0 │ 0 │ 0
0 │ 1 │ 1 │ 0
1 │ 0 │ 1 │ 0
1 │ 1 │ 0 │ 1

S = A XOR B
C = A AND B
```

Un **full adder** (sumador completo) suma dos bits MAS un acarreo de entrada. Encadenando 8 full adders puedes sumar dos numeros de 8 bits. Encadenando 64, sumas numeros de 64 bits. Asi funciona la ALU de tu procesador.

## Tablas de verdad con multiples variables

Para 3 variables hay 2³ = 8 combinaciones. Para n variables, 2ⁿ combinaciones.

Ejemplo: `(A AND B) OR C`

```
  A  │  B  │  C  │ A AND B │ (A AND B) OR C
─────┼─────┼─────┼─────────┼────────────────
  0  │  0  │  0  │    0    │       0
  0  │  0  │  1  │    0    │       1
  0  │  1  │  0  │    0    │       0
  0  │  1  │  1  │    0    │       1
  1  │  0  │  0  │    0    │       0
  1  │  0  │  1  │    0    │       1
  1  │  1  │  0  │    1    │       1
  1  │  1  │  1  │    1    │       1
```

## Aplicaciones

- **Circuitos digitales:** todo el hardware de una computadora esta hecho de compuertas logicas
- **Condiciones en programacion:** `if (edad >= 18 AND tieneID)` es logica booleana
- **Busquedas:** Google usa AND, OR, NOT para filtrar resultados
- **Bases de datos:** las consultas SQL usan WHERE con AND, OR, NOT
- **Permisos y seguridad:** sistemas de acceso basados en condiciones logicas
- **Diseno de circuitos:** desde calculadoras hasta procesadores

## Ejercicios

1. Completa la tabla de verdad para `(A OR B) AND (NOT C)`
2. Simplifica usando leyes de De Morgan: `NOT (A AND B AND C)`
3. Demuestra que NAND es universal construyendo NOT, AND y OR solo con NAND
4. Disena un circuito (usando AND, OR, NOT) que devuelva 1 solo cuando exactamente 2 de 3 entradas sean 1
5. Si `A = 1`, `B = 0`, `C = 1`, evalua: `(A XOR B) AND (B OR C)`

<details>
<summary>Respuestas</summary>

1.
```
A │ B │ C │ NOT C │ A OR B │ Resultado
0 │ 0 │ 0 │   1   │   0    │     0
0 │ 0 │ 1 │   0   │   0    │     0
0 │ 1 │ 0 │   1   │   1    │     1
0 │ 1 │ 1 │   0   │   1    │     0
1 │ 0 │ 0 │   1   │   1    │     1
1 │ 0 │ 1 │   0   │   1    │     0
1 │ 1 │ 0 │   1   │   1    │     1
1 │ 1 │ 1 │   0   │   1    │     0
```

2. `NOT(A AND B AND C) = (NOT A) OR (NOT B) OR (NOT C)`

3.
- NOT con NAND: `NOT A = A NAND A`
- AND con NAND: `A AND B = NOT(A NAND B) = (A NAND B) NAND (A NAND B)`
- OR con NAND: `A OR B = (A NAND A) NAND (B NAND B)`

4. `(A AND B AND NOT C) OR (A AND NOT B AND C) OR (NOT A AND B AND C)`

5. `A XOR B = 1 XOR 0 = 1`, `B OR C = 0 OR 1 = 1`, `1 AND 1 = 1`

</details>
