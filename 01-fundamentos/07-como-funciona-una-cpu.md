# Como Funciona una CPU

## Que es una CPU?

La **CPU** (Central Processing Unit / Unidad Central de Procesamiento) es el "cerebro" de la computadora. Es el chip que ejecuta las instrucciones de los programas.

Una CPU moderna puede ejecutar **miles de millones de instrucciones por segundo**, pero cada instruccion individual es extremadamente simple: sumar dos numeros, mover un dato, comparar dos valores.

## Componentes principales

```
┌─────────────────────────────────────────────┐
│                     CPU                      │
│                                              │
│  ┌──────────┐  ┌───────────┐  ┌──────────┐  │
│  │ Unidad   │  │   ALU     │  │Registros │  │
│  │ de       │  │(Aritmetica│  │          │  │
│  │ Control  │  │ y Logica) │  │ R1  R2   │  │
│  │          │  │           │  │ R3  R4   │  │
│  │          │  │  + - × /  │  │ ...      │  │
│  │          │  │  AND OR   │  │ PC  IR   │  │
│  └──────────┘  └───────────┘  └──────────┘  │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │           Cache (L1, L2)             │    │
│  └──────────────────────────────────────┘    │
└──────────────────────┬──────────────────────┘
                       │ Bus de datos
                 ┌─────┴─────┐
                 │    RAM    │
                 └───────────┘
```

### Unidad de Control (CU)

Dirige todo el proceso. Lee las instrucciones, las decodifica y coordina los demas componentes para ejecutarlas. Es como un director de orquesta.

### ALU (Unidad Aritmetico-Logica)

Realiza las operaciones matematicas (suma, resta, multiplicacion, division) y logicas (AND, OR, NOT, comparaciones). Es la parte que realmente "calcula".

### Registros

Pequenas memorias ultrarapidas dentro de la CPU. Almacenan los datos con los que la CPU esta trabajando en ese preciso momento.

Registros especiales:
- **PC (Program Counter):** contiene la direccion de la proxima instruccion a ejecutar
- **IR (Instruction Register):** contiene la instruccion que se esta ejecutando
- **Acumulador:** almacena resultados temporales de la ALU
- **Flags/Status:** bits que indican condiciones (resultado fue cero, fue negativo, hubo overflow)

### Cache

Memoria pequena y rapida que guarda copias de los datos mas usados de la RAM para no tener que ir a buscarlos cada vez.

## El ciclo Fetch-Decode-Execute

Todo lo que hace una CPU se reduce a repetir este ciclo miles de millones de veces por segundo:

### 1. Fetch (Buscar)

La CPU lee la siguiente instruccion de la memoria usando la direccion almacenada en el **Program Counter (PC)**.

```
PC = 0x0040 → Va a la memoria, lee la instruccion en esa direccion
               La guarda en el Instruction Register (IR)
               Incrementa PC para apuntar a la siguiente instruccion
```

### 2. Decode (Decodificar)

La Unidad de Control interpreta la instruccion. Determina:
- Que operacion hay que hacer
- Que datos necesita
- Donde estan esos datos

```
Instruccion: ADD R1, R2, R3
Decodifica:  "Sumar el contenido de R2 y R3, guardar resultado en R1"
```

### 3. Execute (Ejecutar)

La CPU realiza la operacion. Si es una operacion aritmetica, la ALU hace el calculo. Si es un acceso a memoria, se lee o escribe.

```
R2 contiene: 5
R3 contiene: 3
ALU calcula: 5 + 3 = 8
Resultado:   R1 = 8
```

Despues de ejecutar, el ciclo vuelve a **Fetch** con la siguiente instruccion.

```
┌───────┐     ┌────────┐     ┌─────────┐
│ FETCH │────→│ DECODE │────→│ EXECUTE │──┐
└───────┘     └────────┘     └─────────┘  │
    ↑                                      │
    └──────────────────────────────────────┘
```

## Conjunto de instrucciones (ISA)

El **ISA** (Instruction Set Architecture) es el "idioma" de la CPU. Define todas las instrucciones que puede ejecutar.

### Tipos de instrucciones

```
Aritmeticas:     ADD, SUB, MUL, DIV
Logicas:         AND, OR, XOR, NOT
Movimiento:      MOV (copiar datos entre registros/memoria)
Comparacion:     CMP (comparar dos valores, actualizar flags)
Salto:           JMP (ir a otra instruccion)
                 JZ  (saltar si el resultado fue cero)
                 JNZ (saltar si no fue cero)
Memoria:         LOAD (leer de memoria a registro)
                 STORE (escribir de registro a memoria)
```

### CISC vs RISC

Dos filosofias de diseno:

**CISC (Complex Instruction Set Computer)**
- Instrucciones complejas que hacen mucho en un paso
- Ejemplo: x86 (Intel, AMD) - la mayoria de PCs y laptops
- Una instruccion puede acceder a memoria y calcular al mismo tiempo

**RISC (Reduced Instruction Set Computer)**
- Instrucciones simples que hacen poco, pero son muy rapidas
- Ejemplo: ARM - celulares, tablets, Apple Silicon (M1/M2/M3/M4)
- Cada instruccion hace una sola cosa

## Ejemplo: como se ejecuta una suma simple

Imaginemos el calculo `x = 5 + 3` a nivel de CPU:

```
Paso  │ Instruccion        │ Que pasa
──────┼────────────────────┼──────────────────────────────
  1   │ LOAD R1, [dir_5]   │ Carga el valor 5 en registro R1
  2   │ LOAD R2, [dir_3]   │ Carga el valor 3 en registro R2
  3   │ ADD R3, R1, R2     │ ALU suma R1+R2, resultado en R3 (8)
  4   │ STORE [dir_x], R3  │ Guarda el 8 en la direccion de memoria de x
```

Cada linea pasa por el ciclo Fetch → Decode → Execute.

## Velocidad de la CPU

### Frecuencia del reloj

La CPU tiene un **reloj** que marca el ritmo de ejecucion. Se mide en **hertzios (Hz)**.

```
1 MHz  = 1 millon de ciclos por segundo
1 GHz  = 1,000 millones de ciclos por segundo

Una CPU a 4 GHz ejecuta 4,000,000,000 ciclos por segundo
```

No todas las instrucciones toman un solo ciclo. Algunas toman 1, otras 3, otras 10+. La division es mas lenta que la suma, por ejemplo.

### Pipelining

En lugar de esperar a terminar una instruccion antes de empezar la siguiente, la CPU trabaja como una linea de ensamblaje:

```
Sin pipeline:
Tiempo:    1    2    3    4    5    6    7    8    9
Instr 1:  [F]  [D]  [E]
Instr 2:                 [F]  [D]  [E]
Instr 3:                                [F]  [D]  [E]

Con pipeline:
Tiempo:    1    2    3    4    5
Instr 1:  [F]  [D]  [E]
Instr 2:       [F]  [D]  [E]
Instr 3:            [F]  [D]  [E]
```

Con pipeline, aunque cada instruccion sigue tardando 3 ciclos, se completa una instruccion por ciclo.

### Nucleos (Cores)

Las CPUs modernas tienen **multiples nucleos**, cada uno capaz de ejecutar instrucciones independientemente:

```
CPU de 8 nucleos:
  Core 0: ejecutando programa A
  Core 1: ejecutando programa B
  Core 2: ejecutando programa A (otra parte)
  Core 3: ejecutando programa C
  ...
```

Mas nucleos = mas tareas en paralelo. Pero un programa debe estar disenado para aprovechar multiples nucleos (programacion paralela/concurrente).

### Hilos (Threads)

Algunos procesadores soportan **hyper-threading** (Intel) o **SMT** (AMD), donde cada nucleo puede ejecutar 2 hilos simultaneamente compartiendo recursos.

```
CPU: 8 nucleos, 16 hilos
= Puede trabajar en 16 tareas "al mismo tiempo"
```

## El reloj y los ciclos

Todo dentro de la CPU esta sincronizado por una senal de reloj:

```
Senal:  _|-|_|-|_|-|_|-|_|-|_|-|_
        Cada pulso es un "ciclo de reloj"
```

El **overclock** consiste en aumentar esta frecuencia mas alla de lo especificado por el fabricante. Esto da mas rendimiento pero genera mas calor y puede ser inestable.

## Que NO hace la CPU

La CPU es solo una parte de la computadora. No hace todo:

- **GPU (Graphics Processing Unit):** procesa graficos y calculos paralelos masivos
- **Controlador de disco:** gestiona lectura/escritura de almacenamiento
- **Controlador de red:** maneja las comunicaciones de red
- **Chipset:** coordina la comunicacion entre CPU, memoria y perifericos

## Ejercicios

1. Describe con tus palabras las tres fases del ciclo Fetch-Decode-Execute
2. Si una CPU trabaja a 3 GHz y una instruccion toma 2 ciclos, cuantas de esas instrucciones puede ejecutar por segundo?
3. Que ventaja tiene el pipelining? Puede causar algun problema?
4. Una CPU de 4 nucleos con hyper-threading, cuantos hilos puede ejecutar simultaneamente?
5. Por que los registros son mas rapidos que la RAM?

<details>
<summary>Respuestas</summary>

1. **Fetch:** la CPU busca la siguiente instruccion en memoria. **Decode:** interpreta que operacion debe hacer y con que datos. **Execute:** realiza la operacion (calculo, movimiento de datos, etc.)
2. 3,000,000,000 ciclos/s ÷ 2 ciclos/instruccion = 1,500,000,000 instrucciones por segundo
3. Ventaja: mayor rendimiento porque se solapan instrucciones. Problema: si una instruccion depende del resultado de la anterior, hay que esperar ("pipeline stall/hazard"). Tambien los saltos condicionales pueden invalidar instrucciones ya en el pipeline.
4. 4 nucleos × 2 hilos = 8 hilos simultaneos
5. Los registros estan fisicamente dentro de la CPU, hechos de la misma tecnologia ultrarapida. No necesitan viajar por el bus de datos. Acceder a un registro toma ~1 ciclo, acceder a RAM toma ~100+ ciclos.

</details>
