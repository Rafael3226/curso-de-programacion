# Manejo de Errores

## Por que importa?

Los programas fallan. Un archivo no existe, un servidor no responde, el usuario ingresa datos invalidos. Si no manejas estos errores, tu programa se detiene abruptamente.

El manejo de errores te permite **anticipar problemas**, **reaccionar** de forma controlada, y mantener tu programa funcionando.

## Tipos de errores en JavaScript

```javascript
// SyntaxError — codigo mal escrito (se detecta antes de ejecutar)
// let x = ;   // SyntaxError: Unexpected token ';'

// ReferenceError — variable no existe
// console.log(xyz);  // ReferenceError: xyz is not defined

// TypeError — operacion invalida para el tipo
// null.toString();   // TypeError: Cannot read properties of null
// (5)();             // TypeError: 5 is not a function

// RangeError — valor fuera de rango
// new Array(-1);     // RangeError: Invalid array length

// URIError — uso incorrecto de funciones URI
// decodeURI("%");    // URIError: URI malformed
```

## try...catch

Envuelve codigo que podria fallar y captura el error:

```javascript
try {
  // Codigo que podria fallar
  const datos = JSON.parse("esto no es JSON");
} catch (error) {
  // Se ejecuta si hay un error
  console.log("Hubo un error:", error.message);
}
// El programa sigue ejecutandose normalmente aqui
```

Sin try/catch, el error detendria todo el programa. Con try/catch, lo capturas y decides que hacer.

### El objeto error

```javascript
try {
  undefined.propiedad;
} catch (error) {
  console.log(error.name);     // "TypeError"
  console.log(error.message);  // "Cannot read properties of undefined"
  console.log(error.stack);    // Stack trace completo (util para debugging)
}
```

## finally

Bloque que se ejecuta **siempre**, haya o no error:

```javascript
try {
  console.log("Intentando...");
  // alguna operacion
} catch (error) {
  console.log("Error:", error.message);
} finally {
  console.log("Esto siempre se ejecuta");
  // Limpiar recursos, cerrar conexiones, etc.
}
```

`finally` es util para asegurar que la limpieza ocurra sin importar el resultado.

## throw — lanzar errores propios

Puedes crear y lanzar tus propios errores:

```javascript
function dividir(a, b) {
  if (b === 0) {
    throw new Error("No se puede dividir entre cero");
  }
  return a / b;
}

try {
  console.log(dividir(10, 0));
} catch (error) {
  console.log(error.message); // "No se puede dividir entre cero"
}
```

### Tipos de error personalizados

```javascript
function validarEdad(edad) {
  if (typeof edad !== "number") {
    throw new TypeError("La edad debe ser un numero");
  }
  if (edad < 0 || edad > 150) {
    throw new RangeError("La edad debe estar entre 0 y 150");
  }
  return true;
}

try {
  validarEdad("veinte");
} catch (error) {
  if (error instanceof TypeError) {
    console.log("Error de tipo:", error.message);
  } else if (error instanceof RangeError) {
    console.log("Error de rango:", error.message);
  }
}
```

## Patrones comunes

### Validar entrada de datos

```javascript
function crearUsuario(nombre, email) {
  if (!nombre || nombre.trim() === "") {
    throw new Error("El nombre es obligatorio");
  }
  if (!email || !email.includes("@")) {
    throw new Error("Email invalido");
  }
  return { nombre: nombre.trim(), email: email.trim() };
}

try {
  const usuario = crearUsuario("", "ana@mail.com");
} catch (error) {
  console.log(error.message); // "El nombre es obligatorio"
}
```

### Parsear datos de forma segura

```javascript
function parsearJSON(texto) {
  try {
    return JSON.parse(texto);
  } catch {
    return null;
  }
}

const datos = parsearJSON('{"nombre": "Ana"}');  // { nombre: "Ana" }
const fallo = parsearJSON("esto no es json");     // null
```

### Valor por defecto en caso de error

```javascript
function obtenerConfiguracion() {
  try {
    const datos = leerArchivo("config.json"); // podria fallar
    return JSON.parse(datos);
  } catch {
    return { puerto: 3000, debug: false }; // configuracion por defecto
  }
}
```

### Re-lanzar errores

Captura solo los errores que sabes manejar, re-lanza los demas:

```javascript
try {
  // alguna operacion
} catch (error) {
  if (error instanceof TypeError) {
    console.log("Manejando TypeError");
  } else {
    throw error; // re-lanzar errores que no sabemos manejar
  }
}
```

## Errores en funciones de array

Los metodos de arrays no lanzan errores con datos invalidos, simplemente dan resultados inesperados. Valida antes:

```javascript
function promediar(numeros) {
  if (!Array.isArray(numeros)) {
    throw new TypeError("Se esperaba un array");
  }
  if (numeros.length === 0) {
    throw new Error("El array no puede estar vacio");
  }
  const suma = numeros.reduce((acc, n) => acc + n, 0);
  return suma / numeros.length;
}
```

## Buenas practicas

### 1. Se especifico con lo que capturas

```javascript
// MAL — captura TODO, incluyendo bugs de programacion
try {
  // 100 lineas de codigo
} catch (error) {
  console.log("Algo fallo");
}

// BIEN — envuelve solo lo que puede fallar
const texto = leerEntrada();
let datos;
try {
  datos = JSON.parse(texto);
} catch {
  console.log("JSON invalido, usando valores por defecto");
  datos = {};
}
procesarDatos(datos);
```

### 2. No uses try/catch para control de flujo normal

```javascript
// MAL — usar excepciones para algo previsible
try {
  const valor = objeto.propiedad;
} catch {
  const valor = "default";
}

// BIEN — verificar antes
const valor = objeto?.propiedad ?? "default";
```

### 3. Incluye informacion util en los errores

```javascript
// MAL
throw new Error("Error");

// BIEN
throw new Error(`Usuario con ID ${id} no encontrado en la base de datos`);
```

### 4. No silencies errores sin razon

```javascript
// MAL — el error desaparece y nadie se entera
try {
  operacionImportante();
} catch {
  // vacio
}

// BIEN — al menos registra el error
try {
  operacionImportante();
} catch (error) {
  console.error("Error en operacion:", error.message);
}
```

## Ejercicios

1. Escribe una funcion `dividirSeguro(a, b)` que lance un error si `b` es 0, y manejalo con try/catch
2. Escribe una funcion que intente parsear JSON y devuelva un valor por defecto si falla
3. Escribe una funcion `validarPassword(pass)` que lance errores si: es muy corta (< 8), no tiene numeros, no tiene mayusculas
4. Que pasa si un `throw` ocurre dentro de un `try` que no tiene `catch`?
5. Escribe un try/catch/finally que simule: abrir archivo, leer contenido, cerrar archivo (en finally)

<details>
<summary>Respuestas</summary>

1.
```javascript
function dividirSeguro(a, b) {
  if (b === 0) throw new Error("Division entre cero");
  return a / b;
}

try {
  console.log(dividirSeguro(10, 0));
} catch (error) {
  console.log(error.message);
}
```

2.
```javascript
function parsearJSON(texto, defecto = null) {
  try {
    return JSON.parse(texto);
  } catch {
    return defecto;
  }
}
```

3.
```javascript
function validarPassword(pass) {
  if (pass.length < 8) throw new Error("Minimo 8 caracteres");
  if (!/[0-9]/.test(pass)) throw new Error("Debe contener un numero");
  if (!/[A-Z]/.test(pass)) throw new Error("Debe contener una mayuscula");
  return true;
}
```

4. El error se propaga hacia arriba. Si ningun try/catch lo atrapa, el programa se detiene con un error no capturado ("unhandled exception").

5.
```javascript
let archivo = null;
try {
  archivo = "contenido del archivo";
  console.log("Leyendo:", archivo);
  // podria fallar aqui
} catch (error) {
  console.log("Error al leer:", error.message);
} finally {
  console.log("Cerrando archivo");
  archivo = null;
}
```

</details>
