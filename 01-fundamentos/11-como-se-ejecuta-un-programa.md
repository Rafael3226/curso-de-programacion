# Como se Ejecuta un Programa

## De codigo fuente a ejecucion

Cuando escribes codigo, creas un archivo de texto. Pero la CPU solo entiende instrucciones en binario (codigo maquina). Hay que transformar ese texto en algo que la CPU pueda ejecutar.

Existen dos caminos principales: **compilacion** e **interpretacion**.

## Compilacion

Un **compilador** traduce **todo** el codigo fuente a codigo maquina **antes** de ejecutarlo. El resultado es un archivo ejecutable independiente.

```
                 Compilador
Codigo fuente ──────────────→ Ejecutable (binario) ──→ CPU ejecuta
  (.c, .go,                    (.exe, binario)
   .rs, .cpp)
```

Proceso detallado:

```
1. Preprocesamiento  │ Resuelve includes, macros, constantes
         ↓           │
2. Compilacion       │ Traduce codigo a lenguaje ensamblador
         ↓           │
3. Ensamblado        │ Convierte ensamblador a codigo maquina (archivo objeto .o)
         ↓           │
4. Enlazado (link)   │ Combina archivos objeto + librerias → ejecutable final
         ↓           │
    Ejecutable       │ Archivo binario listo para correr
```

**Ventajas:**
- Ejecucion muy rapida (ya esta en codigo maquina)
- Errores de sintaxis se detectan antes de ejecutar
- No necesitas el compilador para ejecutar el programa

**Desventajas:**
- Hay que recompilar cada vez que cambias el codigo
- El ejecutable es especifico para un sistema operativo y arquitectura
- Ciclo de desarrollo mas lento

**Lenguajes compilados:** C, C++, Go, Rust

## Interpretacion

Un **interprete** lee el codigo fuente linea por linea y lo ejecuta **directamente**, sin generar un ejecutable separado.

```
                 Interprete
Codigo fuente ──────────────→ Ejecucion directa
  (.py, .rb,    (lee y ejecuta
   .js, .php)    al momento)
```

**Ventajas:**
- Desarrollo rapido (cambia y ejecuta inmediatamente)
- Portable (el mismo codigo funciona en cualquier sistema que tenga el interprete)
- Mas facil de depurar interactivamente

**Desventajas:**
- Ejecucion mas lenta (traduce mientras ejecuta)
- Necesitas el interprete instalado para ejecutar
- Errores se descubren al llegar a la linea problematica

**Lenguajes interpretados:** Python, Ruby, PHP, JavaScript (originalmente)

## Modelo hibrido: bytecode + maquina virtual

Muchos lenguajes modernos usan un enfoque intermedio: compilan a un **bytecode** que luego se ejecuta en una **maquina virtual (VM)**.

```
                 Compilador          Maquina Virtual
Codigo fuente ──────────────→ Bytecode ──────────────→ Ejecucion
  (.java, .cs,                (.class,    (JVM, CLR,
   .py, .kt)                  .pyc)       CPython)
```

El bytecode es un codigo intermedio que no es codigo maquina nativo, pero es mas rapido de interpretar que el codigo fuente original.

**Lenguajes con bytecode:** Java (JVM), C# (.NET/CLR), Python (.pyc), Kotlin

### JIT (Just-In-Time Compilation)

Las maquinas virtuales modernas usan **JIT**: mientras el programa se ejecuta, detectan las partes mas usadas ("hot paths") y las compilan a codigo maquina nativo en ese momento. Esto combina la portabilidad del bytecode con la velocidad del codigo nativo.

```
Primera ejecucion:   bytecode → interprete (lento)
Deteccion:           "esta funcion se llama 10,000 veces"
Compilacion JIT:     bytecode → codigo maquina nativo
Siguientes veces:    codigo nativo directo (rapido)
```

JavaScript moderno (V8 en Chrome/Node.js, SpiderMonkey en Firefox) usa JIT agresivamente. Por eso JS es mucho mas rapido de lo que esperarias de un "lenguaje interpretado".

## Lenguaje ensamblador (Assembly)

Es una representacion legible del codigo maquina. Cada instruccion assembly corresponde (casi) directamente a una instruccion de la CPU.

```
Codigo C:           Ensamblador x86:        Codigo maquina:

int suma = a + b;   mov eax, [a]            8B 05 xx xx xx xx
                    add eax, [b]            03 05 xx xx xx xx
                    mov [suma], eax         89 05 xx xx xx xx
```

El ensamblador es especifico para cada arquitectura de CPU:
- x86/x86-64 (Intel, AMD)
- ARM (celulares, Apple Silicon)
- RISC-V (emergente, open source)

## El proceso completo: de doble click a ejecucion

Cuando haces doble click en un programa (o lo ejecutas desde la terminal):

```
1. El sistema operativo (SO) recibe la solicitud de ejecucion

2. El SO lee la cabecera del archivo ejecutable
   - Formato PE en Windows (.exe)
   - Formato ELF en Linux
   - Formato Mach-O en macOS

3. El SO crea un nuevo proceso:
   - Asigna un espacio de memoria virtual
   - Configura el stack (pila) y el heap (monton)
   - Carga las librerias compartidas necesarias (.dll, .so, .dylib)

4. El SO carga el codigo del programa en memoria

5. El SO configura el Program Counter de la CPU para
   apuntar al punto de entrada del programa (main)

6. La CPU comienza a ejecutar instruccion por instruccion
   siguiendo el ciclo Fetch-Decode-Execute

7. Cuando el programa termina (return 0 o exit):
   - El SO libera la memoria asignada
   - Cierra archivos y conexiones abiertas
   - Elimina el proceso
```

## Memoria de un proceso en ejecucion

Cada programa en ejecucion tiene su espacio de memoria organizado asi:

```
Direcciones altas
┌─────────────────────┐
│       Stack         │ ← Crece hacia abajo
│  (variables locales,│   Automatica: se crea/destruye al entrar/salir
│   llamadas a        │   de funciones
│   funciones)        │
├─────────────────────┤
│         ↓           │
│   Espacio libre     │
│         ↑           │
├─────────────────────┤
│       Heap          │ ← Crece hacia arriba
│  (memoria dinamica, │   Manual o con garbage collector
│   objetos, arrays)  │   Se pide y se libera durante la ejecucion
├─────────────────────┤
│       Data          │ ← Variables globales y estaticas
│  (variables         │
│   globales)         │
├─────────────────────┤
│       Text          │ ← El codigo del programa (instrucciones)
│  (codigo del        │   Solo lectura
│   programa)         │
└─────────────────────┘
Direcciones bajas
```

### Stack (Pila)

- Almacena variables locales y datos de llamadas a funciones
- Funciona como una pila de platos: LIFO (Last In, First Out)
- Cada llamada a funcion crea un "stack frame" con sus variables
- Al retornar de la funcion, el frame se elimina automaticamente
- Tamano limitado (tipicamente 1-8 MB) — excederlo causa "stack overflow"

### Heap (Monton)

- Memoria dinamica: se solicita y libera durante la ejecucion
- Donde viven los objetos, arrays dinamicos, strings largos
- Mas lento que el stack pero mucho mas grande
- En lenguajes como C, el programador debe liberar la memoria manualmente
- En lenguajes como Python/Java/JS, un **garbage collector** la libera automaticamente

## Procesos e hilos

### Proceso

Un programa en ejecucion. Cada proceso tiene su propio espacio de memoria aislado.

```
┌──────────────┐    ┌──────────────┐
│  Proceso A   │    │  Proceso B   │
│              │    │              │
│ Su memoria   │    │ Su memoria   │
│ Sus archivos │    │ Sus archivos │
│ Su estado    │    │ Su estado    │
└──────────────┘    └──────────────┘
   Aislados: A no puede ver la memoria de B
```

### Hilo (Thread)

Un "sub-proceso" dentro de un proceso. Los hilos de un mismo proceso **comparten memoria**.

```
┌──────────────────────────────────┐
│           Proceso A              │
│                                  │
│  ┌────────┐  ┌────────┐         │
│  │ Hilo 1 │  │ Hilo 2 │         │
│  │        │  │        │         │
│  │ Stack  │  │ Stack  │         │
│  │ propio │  │ propio │         │
│  └────┬───┘  └────┬───┘         │
│       └──────┬────┘             │
│         Comparten:              │
│         - Heap                  │
│         - Codigo                │
│         - Archivos abiertos     │
└──────────────────────────────────┘
```

## El sistema operativo como intermediario

Los programas no interactuan directamente con el hardware. El **sistema operativo** actua como intermediario:

```
Programa: "Quiero leer un archivo"
    ↓
System call (llamada al sistema)
    ↓
Sistema operativo:
    - Verifica permisos
    - Encuentra el archivo en el disco
    - Lee los datos
    - Los copia al espacio de memoria del programa
    ↓
Programa: recibe los datos
```

Funciones que requieren system calls:
- Leer/escribir archivos
- Enviar/recibir datos de red
- Mostrar algo en pantalla
- Crear nuevos procesos
- Asignar mas memoria

## Comparacion de modelos

```
              │ Compilado        │ Interpretado      │ Bytecode + VM
──────────────┼──────────────────┼───────────────────┼──────────────────
Velocidad     │ Muy rapida       │ Mas lenta         │ Rapida (con JIT)
Portabilidad  │ Recompilar       │ Alta (necesita    │ Alta (necesita
              │ por plataforma   │ interprete)       │ la VM)
Desarrollo    │ Compilar y       │ Ejecutar directo  │ Compilar rapido
              │ ejecutar         │                   │ + ejecutar
Deteccion de  │ Al compilar      │ Al ejecutar       │ Mixto
errores       │                  │                   │
Ejemplos      │ C, C++, Go,     │ Python, Ruby,     │ Java, C#,
              │ Rust             │ PHP               │ Kotlin
```

## Ejercicios

1. Cual es la diferencia fundamental entre compilacion e interpretacion?
2. Que es JIT y por que es importante para lenguajes como JavaScript?
3. Que pasa con la memoria del stack cuando una funcion termina?
4. Por que los procesos estan aislados entre si pero los hilos no?
5. Por que un programa compilado para Windows no funciona directamente en Linux?

<details>
<summary>Respuestas</summary>

1. La **compilacion** traduce todo el codigo a codigo maquina antes de ejecutar, generando un ejecutable. La **interpretacion** lee y ejecuta el codigo linea por linea sin generar ejecutable. La compilacion es mas rapida en ejecucion; la interpretacion es mas flexible.
2. **JIT (Just-In-Time)** compila las partes mas usadas del codigo a maquina nativa mientras el programa se ejecuta. Es importante porque le da a JavaScript velocidades cercanas a lenguajes compilados, especialmente en funciones que se ejecutan muchas veces.
3. Se **libera automaticamente**. El stack pointer retrocede, y el stack frame de esa funcion (con todas sus variables locales) deja de existir. Es una de las razones por las que el stack es tan rapido.
4. Los **procesos** tienen espacios de memoria separados por seguridad y estabilidad (un proceso que falla no afecta a otros). Los **hilos** de un mismo proceso comparten memoria porque estan disenados para cooperar en la misma tarea. Compartir memoria es mas eficiente que copiar datos entre procesos.
5. Porque el codigo maquina es especifico de la CPU, y ademas el formato del ejecutable es diferente (PE en Windows, ELF en Linux). Las system calls tambien son diferentes: un programa Windows llama a la API de Windows, no a system calls de Linux.

</details>
