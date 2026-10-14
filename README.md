# Knights-Haxball
A collection of custom HaxBall Headless scripts, maps, and gameplay features, including Jabulani, curve shots, and powerful shots. Includes an unfinished Real Soccer mode prototype, shared for anyone who wants to explore, improve, or continue its development.

---

# KNIGHTS FC — Script de Sala Haxball

Script completo para gestionar una sala de Haxball con sistema de DTs, posiciones personalizadas, múltiples modos de juego, estadísticas, MVP, VIP, uniformes, mapas propios y anti-troles.

---

## 🚀 Configuración inicial

Edita el bloque `HBInit` al inicio del archivo:

```js
const room = HBInit({
    roomName: "𝟱𝘃𝟱 | 𝗗𝗧 | 𝗞𝗡𝗜𝗚𝗛𝗧𝗦 🛟| 𝟮𝟰/𝟳",  // Nombre de la sala
    maxPlayers: 30,                 // Máximo de jugadores
    public: true,                   // true = sala pública, false = privada
    password: null,                 // Contraseña inicial de entrada
    noPlayer: true,                 // Oculta al host de la lista de jugadores
    token: "",                      // ⚠️ Tu token de Haxball Host (obligatorio)
    geo: { code: "CO", lat: 4.7110, lon: -74.0721 },  // País y coordenadas del pin
    // port: 8300                    // Puerto (solo para servidor propio)
});
```

### Campos importantes

| Campo | Descripción |
|-------|-------------|
| `roomName` | Nombre visible de la sala. Hay varias plantillas comentadas en el código; activa la que quieras y comenta las demás. |
| `maxPlayers` | Capacidad máxima de la sala. |
| `public` | `true` aparece en el listado público; `false` solo con contraseña. |
| `token` | **Obligatorio.** Token de tu anfitrión de Haxball. Nunca lo compartas. |
| `geo` | Código de país (`CO`, `AR`, `ES`...) y coordenadas del pin en el buscador. |
| `noPlayer` | `true` mantiene al host oculto (útil para salas con bot). |

---

## 🎮 Variables de modos de juego

Cambia el valor inicial directamente desde la parte superior del script:

```js
let x5 = true;              // Modalidad por defecto (x1 … x6 = cantidad de jugadores)
let dtsMode = true;         // Modo con DT (entrenador)
let verifiedOnlyMode = false; // Solo jugadores verificados
let everyoneMode = false;   // Todos pueden jugar
let powershotmode = false;  // Modo Powershot
let oneshotmode = false;    // Modo Oneshot
let jabulaniMode = 0;       // Modo Jabulani (0 = off, 1 = on)
let RamboMode = false;      // Modo Rambo
let todosjuegan = false;    // Modo Todos Juegan
let futsalMode = false;     // Modo Futsal
let RealSoccer = false;     // Modo Real Soccer
let fifa3dmode = false;     // Modo FIFA 3D
let hentaiMode = false;     // Mapas alternativos
let narrador = 0;           // Narrador por voz (0 = off, 1 = on)
let dtspeakbot = 0;         // Bot de voz del DT
```

Todos estos modos también se pueden activar/desactivar **en tiempo real** con comandos (ver abajo).

---

## 🎨 Personalización

### Colores del chat y roles
Ajusta el objeto `colors`:

```js
const colors = {
    creator: 0xFFFFFF,   // Creador
    admin: 0x7F00FF,     // Admin
    dtRed: 0xFF5D5D,     // DT equipo rojo
    dtBlue: 0x8ED3EE,    // DT equipo azul
    playerRed: 0xFF0052, // Jugadores rojos
    playerBlue: 0x0063FF,// Jugadores azules
    bot: 0xFFBE00,       // Mensajes del bot
    vip: 0x90E8FF,       // VIP
    mvp: 0xFFC600,       // MVP
    referee: 0xFFB300,   // Árbitro
    hoster: 0x95CDC9,    // Hoster
    // ... y más
};
```

### Posiciones de jugadores por `auth`
En `authPositions` asignas una posición fija a cada jugador usando su `auth` (el ID público que Haxball muestra al entrar):

```js
const authPositions = new Map([
    ["TU_AUTH_AQUI", "MC"],    // jugador → posición
    ["OTRO_AUTH", "DC"],
    // ...
]);
```

**Posiciones válidas:** `GK, DFC, DEF, DFD, DFI, LD, LI, MCD, MC, MCO, CAM, MID, INT, EXT, MI, MD, EL, ED, MP, SD, DC, DEL, DT`

### Listas de roles
El script incluye conjuntos de `auth` para distintos roles, edítalos a tu gusto:

- `hosterAuths` — usuarios que pueden obtener permisos de hoster.
- `PossibleHosterAuths` — candidatos a hoster.
- `vipAuths` — VIPs.
- `protectedAuths` / `specialAuths` — protección y perks especiales.
- `AuthDts` — entrenadores (DT).

### Mapas personalizados
- Estadios propios: `ChampionsMap`, `VestuarioMap`, `CapoDorMap` (se cargan con `!cmap`, `!vesmap`, `!cpdo`).
- Mapas de Rambo: se añaden a `RamboStadiums` con un `key` y su `data`, y se cargan con `!mprmb <clave>` (ej. `!mprmb knights_x5`).

### Webhooks de Discord (opcional)
Rellena las constantes vacías para registrar chat, reportes y eventos en tus canales:

```js
const webhookURL     = ""; // Registro general
const webhookReportURL = ""; // Reportes de jugadores
const webhookRoomURL   = ""; // Eventos de sala
```

> ⚠️ **Nunca subas tus webhooks reales al repositorio.** Déjalos vacíos en el código público y relléналos solo en tu sala en vivo.

---

## 🛡️ Contraseñas y permisos

- `ROOM_PASSWORD` — contraseña que se aplica automáticamente cuando la sala supera cierto número de jugadores (`MAX_PLAYERS_BEFORE_PASSWORD`).
- `HOSTER_PASSWORD` — contraseña del comando `!sethosterknights` para asignar hoster a un jugador.

> ⚠️ Al publicar el código, recuerda que estas quedarán visibles. Cámbialas o déjalas vacías si no quieres exponerlas.

---

## 📋 Comandos principales (en el chat)

| Comando | Función |
|---------|---------|
| `!et` / `!time` / `!untime` | Control de tiempo |
| `!narrador` / `!dtspeak` / `!transmision` | Narración y transmisión |
| `!lockpassword` / `!unlockpassword` / `!abrirsala` / `!cerrarsala` | Abrir/cerrar sala |
| `!kick` / `!ban` / `!clear_bans` | Moderación |
| `!red` / `!blue` / `!DTred` / `!DTblue` | Asignar equipos / DTs |
| `!cmap` / `!vesmap` / `!cpdo` / `!mprmb` | Cargar mapas |
| `!powershot` / `!rambogo` / `!futsal` / `!RSmode` | Activar modos |
| `!verificados` / `!svm` / `!antibus` | Verificados y anti-bugs |
| `!cvip` / `!vipkick` / `!report` | VIPs y reportes |
| `!x1` … `!x6` / `!4v4mode` | Cambiar modalidad |

*(La lista completa está en el bloque de manejo de mensajes del chat.)*

---

## ⚙️ Instalación

1. Crea una sala en https://html5.haxball.com/headless
2. Copia todo el contenido de `script.js`.
3. Pégalo en la consola del anfitrión de Haxball y ejecuta.
4. Ajusta el `token` y los valores de configuración.

---
