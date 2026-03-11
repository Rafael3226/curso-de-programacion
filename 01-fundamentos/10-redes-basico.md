# Redes: Conceptos Basicos

## Que es una red?

Una **red de computadoras** es un conjunto de dispositivos conectados que pueden intercambiar datos entre si. Desde dos computadoras conectadas por un cable hasta internet (miles de millones de dispositivos conectados globalmente).

## Tipos de redes por alcance

```
Tipo  │ Nombre                      │ Alcance
──────┼─────────────────────────────┼──────────────────────
PAN   │ Personal Area Network       │ Tu escritorio (~1 m)
LAN   │ Local Area Network          │ Casa, oficina (~100 m)
WLAN  │ Wireless LAN                │ LAN sin cables (Wi-Fi)
MAN   │ Metropolitan Area Network   │ Una ciudad
WAN   │ Wide Area Network           │ Paises, continentes
      │ Internet                    │ Global
```

## Modelo de capas: como se organiza la comunicacion

Cuando envias un mensaje por internet, los datos pasan por multiples capas, cada una con una responsabilidad distinta. El modelo mas usado en la practica es **TCP/IP** con 4 capas:

```
Capa           │ Que hace                          │ Ejemplos
───────────────┼───────────────────────────────────┼────────────────
4. Aplicacion  │ Protocolos que usan las apps      │ HTTP, HTTPS, DNS,
               │                                   │ FTP, SMTP, SSH
───────────────┼───────────────────────────────────┼────────────────
3. Transporte  │ Entrega confiable o rapida        │ TCP, UDP
               │ entre aplicaciones                │
───────────────┼───────────────────────────────────┼────────────────
2. Red/Internet│ Enrutamiento entre redes          │ IP, ICMP
               │ distintas                         │
───────────────┼───────────────────────────────────┼────────────────
1. Enlace      │ Comunicacion fisica entre         │ Ethernet, Wi-Fi,
               │ dispositivos directos             │ Bluetooth
```

Al enviar datos, cada capa **agrega informacion** (encapsulacion):

```
Datos de la aplicacion:          "Hola mundo"
Capa transporte agrega:          [Puerto origen: 52431 | Puerto destino: 80] + "Hola mundo"
Capa red agrega:                 [IP origen | IP destino] + [puertos] + "Hola mundo"
Capa enlace agrega:              [MAC origen | MAC destino] + [IPs] + [puertos] + "Hola mundo"
```

Al recibir, cada capa quita su parte y pasa el resto hacia arriba.

## Direcciones: como encontrar un dispositivo

### Direccion MAC (capa enlace)

Identificador unico **fisico** de cada tarjeta de red. Viene de fabrica.

```
Formato:  A4:83:E7:2B:00:1F  (6 bytes en hexadecimal)
          ───────  ─────────
          Fabricante  Dispositivo

Alcance: solo funciona en la red local
```

### Direccion IP (capa red)

Identificador **logico** asignado por la red. Permite comunicacion entre redes distintas.

**IPv4:** 32 bits, 4 numeros de 0-255 separados por puntos.

```
192.168.1.100
───┬─── ─┬─ ─┬─ ──┬──
   │     │   │    └── Dispositivo especifico
   └─────┴───┴────── Identificador de red

Representacion binaria:
192     .168     .1       .100
11000000.10101000.00000001.01100100
```

IPs reservadas:
```
127.0.0.1       → localhost (tu propia maquina)
192.168.x.x     → red privada (tu casa/oficina)
10.x.x.x        → red privada
0.0.0.0         → "todas las interfaces"
255.255.255.255 → broadcast (todos en la red local)
```

**IPv6:** 128 bits, para solucionar el agotamiento de direcciones IPv4.

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
(8 grupos de 4 digitos hexadecimales)

IPv4 permite:  ~4.3 mil millones de direcciones
IPv6 permite:  ~340 sextillones de direcciones (3.4 × 10³⁸)
```

### Puerto (capa transporte)

Un numero (0-65535) que identifica una **aplicacion especifica** dentro de un dispositivo. Un dispositivo tiene una IP, pero puede tener muchos servicios ejecutandose.

```
IP:Puerto

192.168.1.100:80    → servidor web (HTTP)
192.168.1.100:443   → servidor web (HTTPS)
192.168.1.100:22    → SSH
192.168.1.100:3306  → base de datos MySQL
```

Puertos conocidos:
```
22    → SSH (acceso remoto seguro)
25    → SMTP (envio de correo)
53    → DNS (resolucion de nombres)
80    → HTTP (web)
443   → HTTPS (web segura)
3000  → Express/Node.js (convencion de desarrollo)
3306  → MySQL
5432  → PostgreSQL
8080  → Servidor web alternativo
```

## Protocolos clave

### IP (Internet Protocol)

Se encarga de **enrutar paquetes** de un origen a un destino a traves de multiples redes. Cada paquete viaja de forma independiente y puede tomar rutas diferentes.

IP es **"best effort"**: no garantiza que los paquetes lleguen, ni en orden, ni sin duplicados. Esa responsabilidad es de la capa de transporte.

### TCP (Transmission Control Protocol)

Protocolo de transporte **confiable y ordenado**.

```
Caracteristicas:
✓ Garantiza que los datos llegan
✓ Los datos llegan en orden
✓ Detecta y retransmite paquetes perdidos
✓ Control de flujo (no satura al receptor)
✗ Mas lento por toda esta verificacion

Usos: web (HTTP), email, transferencia de archivos, SSH
```

Antes de enviar datos, TCP establece una conexion (**three-way handshake**):

```
Cliente                    Servidor
   │                          │
   │── SYN ──────────────────→│  "Quiero conectarme"
   │                          │
   │←──────────── SYN-ACK ────│  "OK, acepto"
   │                          │
   │── ACK ──────────────────→│  "Perfecto, empecemos"
   │                          │
   │←────── datos ───────────→│  (comunicacion bidireccional)
```

### UDP (User Datagram Protocol)

Protocolo de transporte **rapido pero sin garantias**.

```
Caracteristicas:
✓ Muy rapido, baja latencia
✓ Simple, poco overhead
✗ No garantiza entrega
✗ No garantiza orden
✗ No detecta paquetes perdidos

Usos: videollamadas, streaming, juegos online, DNS
```

En una videollamada, es mejor perder un frame que pausar para esperar un paquete retrasado. Por eso se usa UDP.

### DNS (Domain Name System)

Traduce nombres de dominio a direcciones IP. Es como la "agenda telefonica" de internet.

```
Tu navegador:  "Quiero ir a www.ejemplo.com"
DNS responde:  "Eso esta en 93.184.216.34"
Tu navegador:  Se conecta a 93.184.216.34
```

Proceso de resolucion:

```
1. Cache local del navegador     → Sabe la IP? → Si: listo
2. Cache del sistema operativo   → Sabe la IP? → Si: listo
3. Servidor DNS del ISP          → Sabe la IP? → Si: listo
4. Servidor DNS raiz             → Pregunta a .com
5. Servidor .com                 → Pregunta a ejemplo.com
6. Servidor de ejemplo.com       → "La IP es 93.184.216.34"
```

### HTTP/HTTPS (HyperText Transfer Protocol)

El protocolo de la web. Funciona con un modelo **peticion-respuesta** (request-response):

```
Navegador (cliente)                    Servidor
       │                                  │
       │── GET /pagina.html ─────────────→│  "Dame esta pagina"
       │                                  │
       │←── 200 OK + <html>...</html> ────│  "Aqui tienes"
       │                                  │

Metodos HTTP comunes:
GET    → Obtener un recurso
POST   → Enviar datos (crear algo)
PUT    → Actualizar un recurso completo
DELETE → Eliminar un recurso

Codigos de respuesta:
200 → OK (todo bien)
301 → Redireccion permanente
404 → No encontrado
500 → Error interno del servidor
```

**HTTPS** es HTTP con encriptacion (TLS/SSL). El candado en tu navegador indica HTTPS.

## Como viajan los datos por internet

Ejemplo: abrir `www.ejemplo.com` desde tu casa.

```
1. Tu navegador pide al DNS la IP de www.ejemplo.com
2. DNS responde: 93.184.216.34
3. Tu computadora crea un paquete HTTP (GET /index.html)
4. TCP divide los datos en segmentos y agrega puertos
5. IP agrega las direcciones IP de origen y destino
6. El paquete sale de tu computadora al router de tu casa
7. Tu router lo envia al router de tu ISP
8. Pasa por multiples routers intermedios (pueden estar en otros paises)
9. Llega al servidor de destino
10. El servidor procesa la peticion y envia la respuesta
11. La respuesta viaja de vuelta por la misma (o diferente) ruta
12. Tu navegador recibe los datos y muestra la pagina
```

Cada paso intermedio (router) decide cual es el siguiente salto, como un paquete postal que pasa por multiples centros de distribucion.

## Conceptos adicionales

### NAT (Network Address Translation)

Tu router de casa tiene una sola IP publica, pero multiples dispositivos. NAT permite que todos compartan esa IP traduciendo las direcciones:

```
PC (192.168.1.2)     ─┐
Celular (192.168.1.3) ─┼── Router (IP publica: 85.50.10.20) ── Internet
Tablet (192.168.1.4)  ─┘
```

Desde afuera, todo el trafico parece venir de `85.50.10.20`.

### Firewall

Un sistema que filtra trafico de red segun reglas:

```
Permitir: trafico HTTP/HTTPS saliente
Permitir: respuestas a conexiones que yo inicie
Bloquear: conexiones entrantes no solicitadas
Bloquear: trafico al puerto 3306 desde fuera
```

### VPN (Virtual Private Network)

Crea un "tunel" encriptado entre tu dispositivo y un servidor VPN. Todo tu trafico pasa por ese tunel, oculto de tu ISP y de la red local.

## Ejercicios

1. Cual es la diferencia entre una direccion MAC y una direccion IP?
2. Por que se usa UDP en lugar de TCP para videollamadas?
3. Que pasa cuando escribes `www.google.com` en tu navegador? (describe los pasos principales)
4. Si tu computadora tiene la IP 192.168.1.50 y un servidor web corre en el puerto 8080, como accedes desde tu red?
5. Cuantas direcciones IP unicas puede tener IPv4? Por que no son suficientes?

<details>
<summary>Respuestas</summary>

1. **MAC** es una direccion fisica, unica de hardware, de 48 bits, solo funciona en la red local. **IP** es una direccion logica, asignada por la red, que permite enrutar datos entre redes distintas a traves de internet.
2. Porque en video en tiempo real, la **velocidad importa mas que la perfeccion**. Perder un frame es imperceptible, pero esperar a que TCP retransmita un paquete causaria lag notable. UDP envia datos sin esperar confirmacion.
3. (1) DNS traduce `www.google.com` a una IP. (2) Tu navegador abre una conexion TCP a esa IP en el puerto 443 (HTTPS). (3) Realiza el handshake TLS para encriptar. (4) Envia un `GET /`. (5) Google responde con el HTML. (6) Tu navegador descarga recursos adicionales (CSS, JS, imagenes). (7) Renderiza la pagina.
4. Abriendo en el navegador: `http://192.168.1.50:8080`
5. 2³² = 4,294,967,296 (~4.3 mil millones). No son suficientes porque hay mas dispositivos conectados que eso en el mundo (celulares, IoT, servidores, etc.). Por eso se creo IPv6 con 2¹²⁸ direcciones y se usa NAT como solucion temporal.

</details>
