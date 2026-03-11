# Jerarquia de Memoria

## El problema fundamental

La CPU es extremadamente rapida, pero la memoria principal (RAM) es comparativamente lenta. Si la CPU tuviera que esperar por la RAM en cada operacion, desperdiciaria la mayor parte del tiempo sin hacer nada.

La solucion: una **jerarquia de memorias** donde las mas rapidas (y caras) estan cerca de la CPU, y las mas lentas (y baratas) estan mas lejos.

## La piramide de memoria

```
         Velocidad        Capacidad        Costo
          maxima            minima         maximo
            ▲                 ▲              ▲
            │    ┌────────┐   │              │
            │    │Registros│  │              │
            │    │ ~1 KB  │   │              │
            │    ├────────┤   │              │
            │    │Cache L1│   │              │
            │    │ ~64 KB │   │              │
            │    ├──────────┤ │              │
            │    │ Cache L2 │ │              │
            │    │ ~256 KB  │ │              │
            │    ├────────────┤              │
            │    │  Cache L3  │              │
            │    │  ~8-64 MB  │              │
            │    ├──────────────┤            │
            │    │     RAM      │            │
            │    │   8-64 GB    │            │
            │    ├────────────────┤          │
            │    │    SSD/NVMe    │          │
            │    │  256 GB - 4 TB │          │
            │    ├──────────────────┤        │
            │    │   Disco duro HDD  │      │
            │    │    1 TB - 20 TB   │      │
            │    ├────────────────────┤      │
            │    │ Almacenamiento en nube │  │
            │    │      "Ilimitado"       │  │
            ▼    └────────────────────────┘  ▼
         Velocidad        Capacidad        Costo
          minima            maxima         minimo
```

## Tiempos de acceso comparados

```
Tipo de memoria    │ Tiempo de acceso │ Analogia humana
───────────────────┼──────────────────┼──────────────────────────
Registro CPU       │ ~0.3 ns          │ Recordar tu nombre
Cache L1           │ ~1 ns            │ Leer algo en tu escritorio
Cache L2           │ ~3-5 ns          │ Buscar en un cajon
Cache L3           │ ~10-20 ns        │ Ir a la estanteria del cuarto
RAM                │ ~50-100 ns       │ Ir a la cocina por algo
SSD NVMe           │ ~25,000 ns       │ Ir a la tienda de la esquina
SSD SATA           │ ~100,000 ns      │ Ir al supermercado
HDD                │ ~5,000,000 ns    │ Viajar a otra ciudad
Red (internet)     │ ~10-100 ms       │ Pedir algo por correo
```

Para poner en perspectiva: si un acceso a registro fuera **1 segundo**, un acceso a disco duro seria **6 meses**.

ns = nanosegundos (milmillonesimas de segundo)
ms = milisegundos (milesimas de segundo)

## Registros

- **Ubicacion:** dentro de la CPU
- **Capacidad:** decenas a centenas de valores (tipicamente 16-32 registros de 64 bits)
- **Velocidad:** acceso en ~1 ciclo de reloj
- **Funcion:** almacenan los datos con los que la CPU opera directamente

```
Ejemplo de registros en x86-64:
RAX, RBX, RCX, RDX     → proposito general
RSP                      → puntero de pila (stack pointer)
RBP                      → puntero de base
RIP                      → puntero de instruccion (program counter)
RFLAGS                   → flags de estado
```

## Cache

La cache es memoria SRAM (Static RAM) muy rapida, ubicada dentro o muy cerca de la CPU. Se organiza en **niveles**:

### Cache L1

- **Capacidad:** ~32-64 KB por nucleo
- **Velocidad:** ~1 ns (~4 ciclos)
- **Particularidad:** generalmente dividida en dos:
  - **L1i:** cache de instrucciones
  - **L1d:** cache de datos

### Cache L2

- **Capacidad:** ~256 KB - 1 MB por nucleo
- **Velocidad:** ~3-5 ns (~10 ciclos)
- **Funcion:** respaldo para L1, almacena datos que L1 no puede contener

### Cache L3

- **Capacidad:** ~8-64 MB compartida entre todos los nucleos
- **Velocidad:** ~10-20 ns (~30-50 ciclos)
- **Funcion:** compartida, reduce accesos a RAM

### Como funciona la cache?

Cuando la CPU necesita un dato:

```
1. Busca en L1 → Si esta ahi: "cache hit" (rapido!)
                  Si no:
2. Busca en L2 → Si esta ahi: cache hit, lo copia a L1
                  Si no:
3. Busca en L3 → Si esta ahi: cache hit, lo copia a L2 y L1
                  Si no:
4. Va a la RAM → "cache miss" (lento), trae el dato y lo copia
                  a L3, L2 y L1
```

### Localidad

La cache es efectiva porque los programas tienden a seguir dos patrones:

**Localidad temporal:** si usaste un dato hace poco, probablemente lo usaras de nuevo pronto.

```
Ejemplo: una variable contador en un bucle.
Se accede en cada iteracion → siempre estara en cache.
```

**Localidad espacial:** si usaste un dato en una direccion, probablemente usaras datos en direcciones cercanas.

```
Ejemplo: recorrer un array elemento por elemento.
Al traer un elemento a cache, se traen los vecinos tambien ("cache line", tipicamente 64 bytes).
```

## RAM (Random Access Memory)

- **Tipo:** DRAM (Dynamic RAM)
- **Capacidad:** tipicamente 8-64 GB en una PC
- **Velocidad:** ~50-100 ns
- **Volatil:** pierde su contenido al apagar la computadora
- **Funcion:** almacena los programas en ejecucion y sus datos

### Como funciona la RAM?

Cada celda de DRAM almacena un bit como una carga electrica en un tiny capacitor. Estos capacitores se descargan con el tiempo, asi que la RAM necesita **refrescarse** constantemente (por eso se llama "dinamica").

### DDR (Double Data Rate)

La RAM moderna es DDR:

```
DDR3:  2007, hasta ~17 GB/s
DDR4:  2014, hasta ~25 GB/s
DDR5:  2020, hasta ~50 GB/s

Cada generacion mejora velocidad y eficiencia energetica.
```

### Canales de memoria

Las computadoras pueden usar multiples canales para acceder a la RAM en paralelo:

```
Single channel: 1 modulo, ancho de banda base
Dual channel:   2 modulos, ~2x ancho de banda
Quad channel:   4 modulos, ~4x ancho de banda (servidores)
```

## Almacenamiento persistente

A diferencia de la RAM, el almacenamiento persistente conserva datos cuando se apaga la computadora.

### SSD (Solid State Drive)

- **Tecnologia:** memoria flash (NAND)
- **Sin partes moviles:** resistente a golpes, silencioso
- **Velocidad:**
  - SATA: ~500 MB/s
  - NVMe: ~3,500-7,000 MB/s (conectado directamente al bus PCIe)
- **Desgaste:** cada celda tiene un numero limitado de escrituras (~3,000-100,000)

### HDD (Hard Disk Drive)

- **Tecnologia:** discos magneticos giratorios con un cabezal de lectura
- **Velocidad:** ~100-200 MB/s secuencial
- **Latencia alta:** el cabezal debe moverse fisicamente ("seek time" ~5-10 ms)
- **Ventaja:** costo por GB mucho menor que SSD
- **Fragil:** sensible a golpes y vibraciones

```
Componentes de un HDD:
  ┌─── Plato magnetico (gira a 5400-7200 RPM)
  │    ┌─── Cabezal de lectura/escritura
  │    │
  ▼    ▼
 ╭────────╮
 │ ○○○○○○ │  ← pistas concentricas
 │ ○○○○○○ │
 │ ○○○○○○ │  El cabezal se mueve hacia
 │ ○○○○○○ │  adentro/afuera para acceder
 ╰────────╯  a diferentes pistas
```

## Memoria virtual

Los sistemas operativos usan **memoria virtual** para dar a cada programa la ilusion de tener toda la memoria para si mismo.

```
Programa A cree que tiene:         En realidad:
┌──────────────────┐
│ 0x0000 - 0xFFFF  │ ──────→  Parte en RAM, parte en disco
└──────────────────┘

Programa B cree que tiene:
┌──────────────────┐
│ 0x0000 - 0xFFFF  │ ──────→  Parte en RAM, parte en disco
└──────────────────┘
```

Cuando la RAM se llena, el sistema operativo mueve datos poco usados al disco (**swap/paging**). Si necesita esos datos de nuevo, los trae de vuelta. Esto es mucho mas lento que RAM directa, por eso cuando una computadora empieza a usar mucho swap, se siente lenta.

## Resumen comparativo

```
                Velocidad    Capacidad     Volatil    Costo/GB
Registros       ~0.3 ns      ~1 KB         Si         $$$$$
Cache L1        ~1 ns        ~64 KB        Si         $$$$
Cache L2        ~5 ns        ~256 KB       Si         $$$$
Cache L3        ~15 ns       ~8-64 MB      Si         $$$
RAM             ~75 ns       ~8-64 GB      Si         $$
SSD NVMe        ~25 μs       ~0.5-4 TB     No         $
HDD             ~5 ms        ~1-20 TB      No         ¢
```

## Ejercicios

1. Por que no podemos hacer toda la memoria de un computador con la tecnologia de los registros?
2. Si un programa recorre un array de 1 millon de elementos en orden, sera eficiente con la cache? Por que?
3. Cuantas veces mas lento es acceder a un HDD comparado con la cache L1?
4. Que pasa cuando un programa necesita mas memoria RAM de la disponible?
5. Por que un SSD NVMe es mas rapido que un SSD SATA?

<details>
<summary>Respuestas</summary>

1. Porque seria extremadamente costoso y generaria demasiado calor. La SRAM usada en registros y cache es mucho mas cara que la DRAM de la RAM. Ademas, mas memoria rapida = mas transistores = mas calor = mas consumo.
2. Si, muy eficiente. Recorrer un array en orden tiene excelente **localidad espacial**: cada cache line (64 bytes) trae multiples elementos consecutivos. Casi todos los accesos seran cache hits.
3. Cache L1 ~ 1 ns, HDD ~ 5,000,000 ns → el HDD es ~5,000,000 veces mas lento.
4. El sistema operativo usa **memoria virtual/swap**: mueve paginas poco usadas de la RAM al disco para hacer espacio. Si necesita esos datos, los trae de vuelta (page fault). Esto causa una degradacion significativa del rendimiento.
5. SATA es una interfaz disenada para discos duros, con un protocolo (AHCI) que limita la velocidad y paralelismo. NVMe usa el bus PCIe directamente, con un protocolo disenado para almacenamiento flash, con muchas mas colas de comandos paralelas y menos latencia.

</details>
