# 🖥️ Aplicaciones de Escritorio Nativas — Tauri v2 como Stack de Referencia

> Este documento define el patrón de arquitectura recomendado por defecto para cualquier proyecto dbv-specs-ops
> cuyo objetivo sea una aplicación de escritorio nativa multiplataforma (Windows, Linux, macOS).
> Extraído de un proyecto real llevado hasta publicación en Microsoft Store y Uptodown.

## 1. Por qué Tauri v2 como opción por defecto

Frente a Electron, Tauri v2 usa el motor WebView **ya instalado en el sistema operativo** (WebView2 en
Windows, WebKitGTK en Linux, WKWebView en macOS) en vez de empaquetar un Chromium completo. Consecuencias
medibles en un proyecto real:

- Instalador final: **~15-20 MB** (con WebView2 offline embebido) o **~10 MB** sin él, frente a >100 MB típico de Electron.
- RAM en reposo: bajo 64 MB, frente a 150-300 MB típico de Electron.
- Arranque en frío: <200 ms percibido, gracias a evitar un bundler de JS (ver §3).

**Cuándo NO elegir Tauri:** si el proyecto ya tiene una base de código Electron grande, si el equipo no
tiene ninguna experiencia con Rust y el plazo no permite curva de aprendizaje, o si se necesitan APIs de
Node.js muy específicas del lado del proceso principal que no tengan equivalente en el ecosistema de
plugins de Tauri.

## 2. Arquitectura de referencia

```
Sistema Operativo (Windows / Linux / macOS)
        │
        ▼
   CORE (Rust) ── lee args CLI, expone comandos vía #[tauri::command],
        │          gestiona ficheros/red/watchers, nunca renderiza HTML
        │ Tauri IPC Bridge (window.__TAURI__)
        ▼
   FRONTEND (WebView nativo del SO) ── HTML/CSS/JS, toda la lógica de
                                        presentación y sanitización de salida
```

Regla de oro: **el backend Rust nunca debería tener lógica condicional por sistema operativo** si se puede
evitar (`cfg(windows)`, registro de Windows, etc.) — todo lo que difiere por plataforma debería resolverse
en la capa de **empaquetado**, no en el código de aplicación (ver §5).

## 3. Patrón "sin bundler" (IIFE + vendor scripts locales)

Si el frontend no necesita un framework reactivo complejo, evitar Vite/Webpack por completo:

- Vendorizar cada librería de terceros como script UMD/IIFE en `src/vendor/` (descargado una vez, sin CDN).
- Encapsular el código propio en una IIFE clásica (`app.js`), **no** `<script type="module">` — los ES
  Modules dan fallos silenciosos en algunos WebViews embebidos bajo `tauri://` / protocolo custom.
- **La IIFE es obligatoria en TODOS los ficheros JS propios, no solo el principal — incluso "ficheros de
  utilidades que solo definen funciones".** Los scripts clásicos comparten un único ámbito global: si dos
  ficheros declaran el mismo identificador en su top-level (p. ej. un `i18n.js` que define `function t()`
  y un `app.js` que hace `const { t } = window.miI18n`), el segundo fichero muere entero con
  `SyntaxError: Identifier already declared` — y al ser un error de *parseo*, ninguna línea de ese fichero
  llega a ejecutarse: ni listeners, ni handlers de error definidos dentro de él. El síntoma resultante
  (página que renderiza perfectamente pero con la interfaz completamente muerta, sin ningún error visible)
  cuesta horas si no se sabe buscar. Cada fichero se envuelve en su propia IIFE y expone su API por una
  única asignación a `window.<nombre>`.
- **Para depurar este tipo de muerte silenciosa de un script:** los capturadores
  `window.onerror`/`unhandledrejection` deben registrarse en un `<script>` inline (sin `defer`) en el
  `<head>` del HTML, antes de cualquier script externo — un capturador definido dentro del fichero que
  falla nunca llega a registrarse. En un WebView de escritorio sin DevTools abiertos, pintar el error en
  un banner dentro de la propia página es el equivalente práctico de la consola.
- Activar `"withGlobalTauri": true` en `tauri.conf.json` para que `window.__TAURI__` esté disponible sin
  necesidad de `import` — imprescindible para que este patrón sin bundler funcione con los plugins de Tauri.

Resultado: 100% offline, sin paso de build de frontend, carga instantánea.

**Cuándo SÍ usar un bundler:** si el frontend crece más allá de una pantalla y se necesita un framework
como React — en ese caso usar Vite normalmente, Tauri lo soporta de forma nativa (`tauri.conf.json` →
`build.beforeDevCommand`/`beforeBuildCommand`).

## 4. Ocho lecciones de arquitectura transferibles

1. **Sanitiza en la capa correcta, no en la primera posible.** Si el pipeline es "texto plano → HTML"
   (Markdown, plantillas, etc.), sanitizar el **HTML ya renderizado** en el frontend (p. ej. con DOMPurify),
   no el texto plano de entrada en el backend con un parser HTML — un sanitizador HTML aplicado sobre texto
   plano re-escapa cualquier `<`/`&` suelto (código con genéricos, comparadores), corrompiendo cualquier
   bloque de código técnico. Regla general: sanitiza el formato final, no un formato intermedio.

2. **Vigila el directorio padre, no el fichero, para file watching.** La mayoría de editores guardan con
   escritura a fichero temporal + `rename()` atómico. Un watcher apuntando directamente al path del fichero
   puede perder el watch tras el primer rename (especialmente en Windows). Patrón robusto: vigilar el
   directorio contenedor en modo no recursivo y filtrar en el callback por nombre de fichero, con un
   pequeño debounce (~150ms) antes de reaccionar (un solo guardado suele disparar varios eventos seguidos).

3. **Instancia única multi-ventana ≠ pestañas.** Si el requisito real es "un solo proceso en el
   Administrador de Tareas" (no necesariamente "una sola ventana"), un plugin de instancia única que abra
   una `WebviewWindow` nueva **en el mismo proceso** por cada apertura externa resuelve el problema real con
   una fracción del coste de implementar pestañas de verdad. No sobre-construir hacia pestañas si nadie lo
   ha pedido explícitamente.

4. **El auto-actualizador necesita un par de claves fuera del repo desde el primer commit.** Si se va a
   añadir actualización automática (`tauri-plugin-updater` o equivalente), generar el par de claves de firma
   al principio y documentar desde el día uno dónde vive la clave privada (nunca en el repo) y qué pasa si
   se pierde (ninguna versión futura podrá firmarse de forma compatible con instalaciones ya existentes).
   Avisar explícitamente al usuario de hacer copia de seguridad de esa clave/password.

5. **La comprobación de actualizaciones nunca debe bloquear el arranque.** Vivir exclusivamente detrás de
   una acción explícita del usuario (botón "Buscar actualizaciones"), nunca en el flujo de arranque — un
   requisito de rendimiento (arranque <200ms) no es compatible con una llamada de red síncrona o incluso
   asíncrona-pero-bloqueante-de-UI al iniciar.

6. **Si el mismo binario se distribuye por dos canales (tienda + self-hosted), detecta desde qué canal se
   ejecuta y desactiva el actualizador propio en el canal de tienda.** Una app instalada vía Microsoft
   Store/Mac App Store se actualiza por la propia tienda — si el botón de "Buscar actualizaciones" propio
   sigue activo y apunta al manifiesto de GitHub Releases, puede crear una instalación paralela desconectada
   de la de la tienda. Patrón usado: detectar en Rust si el ejecutable actual vive bajo el directorio de
   instalación de la tienda (p. ej. `WindowsApps` en Windows) y ocultar la UI de actualización manual si es así.

7. **i18n sin librería es válido para apps pequeñas.** Con pocas decenas de strings, dos objetos planos
   (`es`/`en`, clave→string con sustitución simple de placeholders) más una función que recorra atributos
   `data-i18n` del DOM cubre el caso de uso sin añadir una dependencia (i18next y similares) desproporcionada
   para el tamaño real del problema. Reevaluar solo si el número de idiomas o de strings crece mucho.

8. **Persistencia simple no necesita una base de datos embebida.** Para listas cortas (recientes, favoritos,
   configuración de usuario), un JSON plano en el directorio de datos de la app (`app_data_dir()`) con
   `std::fs` + `serde_json` es suficiente y evita añadir SQLite/sled solo para eso.

## 5. Empaquetado multiplataforma sin lógica condicional en el código

Tauri v2 fusiona automáticamente `tauri.<platform>.conf.json` sobre `tauri.conf.json` según el sistema
operativo donde se ejecuta el build, sin necesidad de flags ni lógica condicional propia:

- `tauri.windows.conf.json` → `bundle.targets: ["nsis"]` (instalador NSIS).
- `tauri.linux.conf.json` → `bundle.targets: ["appimage", "deb"]`.
- `tauri.macos.conf.json` → `bundle.targets: ["dmg", "app"]`.

Antes de dar por buena esta separación, verificar (no asumir) que el código Rust de aplicación no tiene
ninguna dependencia real de plataforma — buscar `cfg(windows)`/registro de Windows/rutas hardcodeadas antes
de prometer soporte multiplataforma.

**Asociación de archivos por formato de paquete Linux — no todos los formatos se comportan igual:**

- **`.deb`**: la asociación de tipo de archivo se registra de forma declarativa vía `bundle.fileAssociations`
  — el bundler genera la entrada `.desktop` correspondiente al instalar el paquete. Esto típicamente **no se
  puede verificar de extremo a extremo sin una máquina Linux real** (doble clic desde el gestor de archivos,
  integración distinta entre entornos de escritorio GNOME/KDE) — si el proyecto no tiene esa máquina
  disponible, documentarlo como riesgo aceptado ("el workflow de CI compila y empaqueta sin error" es una
  verificación distinta de "la asociación funciona en un escritorio real"), no darlo por validado solo porque
  el build pasó.
- **`.AppImage`**: es portátil por diseño — **no se integra con el sistema ni asocia tipos de archivo
  automáticamente**, con o sin `fileAssociations` configurado. No es un bug del proyecto ni algo que el
  bundler pueda resolver: es una limitación del propio formato, que requiere una herramienta externa
  (`AppImageLauncher` o similar) instalada por el usuario para integrarse con el escritorio. Documentar esto
  explícitamente de cara al usuario final (README/instrucciones de instalación) en vez de tratarlo como una
  asociación de archivo rota.

## 6. Trampas concretas de Tauri v2 — permisos, WebView y threading

Los 8 principios de la sección anterior son necesarios pero no suficientes: estas son trampas *concretas*
de la API de Tauri v2 que cuestan horas reales de depuración la primera vez que aparecen, porque fallan en
silencio o con un error que no apunta a la causa real.

1. **`onCloseRequested` exige el permiso `core:window:allow-destroy`, aunque nunca llames a `.destroy()` a
   mano.** Si el handler no hace `event.preventDefault()`, la propia librería `@tauri-apps/api/window`
   invoca `this.destroy()` internamente para completar el cierre. Sin ese permiso en `capabilities/*.json`
   (no incluido en `core:default`, que solo trae lecturas de estado), la ventana se queda **permanentemente
   sin poder cerrarse** por ningún medio (ni la X, ni Alt+F4) — no un error de consola, un bug de UX severo
   y silencioso. Concede el permiso *antes* de escribir el handler, no después de que la ventana se quede
   bloqueada.

2. **`window.confirm()`/`window.alert()` no son síncronos en un WebView de Tauri con `tauri-plugin-dialog`
   registrado — y en algunas versiones el de `confirm` está directamente roto.** El script de inicialización
   del plugin redefine esos globales para invocar comandos IPC asíncronos (`plugin:dialog|confirm`/
   `|message`); `window.confirm()` devuelve una **promesa**, no un booleano — tratarlo como síncrono no
   lanza ningún error, simplemente evalúa la promesa como verdadera siempre. Además, en `tauri-plugin-dialog`
   2.7.2 concretamente, el comando `confirm` no está registrado en el lado Rust (se fusionó con `message` en
   algún punto y el script JS del plugin nunca se actualizó) — cualquier permiso que concedas es irrelevante,
   el comando no existe. Si necesitas confirmación bloqueante de verdad, construye un modal propio en
   HTML/CSS — no dependas de los diálogos nativos del navegador reescritos por un plugin, y verifica el
   comportamiento real leyendo el código fuente del crate instalado
   (`~/.cargo/registry/src/.../<crate>-<version>/src/lib.rs`, buscar `generate_handler!`), no solo la
   documentación.

3. **WebView2 (Windows) cachea agresivamente entre relanzamientos del *proceso*, no solo en memoria.** Si
   editas frontend y `tauri dev`/el `.exe` de debug sigue mostrando la versión anterior tras recompilar,
   sospecha primero de esto antes que de un bug de código: cierra la app y borra únicamente
   `EBWebView\Default\Cache` y `EBWebView\Default\Code Cache` bajo el directorio de datos de la app
   (`%LOCALAPPDATA%\<identifier>\EBWebView\`) — nunca la carpeta `EBWebView` completa, ahí vive también
   `localStorage` con las preferencias reales del usuario si compartes `identifier` con la build de
   producción instalada.

4. **`capabilities/*.json` → `"windows"` filtra por *label* con un patrón glob, no da permisos a toda la
   app.** Cualquier ventana creada dinámicamente en tiempo de ejecución (`WebviewWindowBuilder`, p. ej. con
   labels `doc-0`, `doc-1`...) con una etiqueta que no case con los patrones declarados se queda **sin
   ningún permiso** — `event:listen` incluido — y falla en silencio o con `Command ... not allowed by ACL`
   la primera vez que intenta usar cualquier capability. Si generas labels dinámicos con un prefijo, añade
   el glob correspondiente (`"windows": ["main", "doc-*"]`) desde el principio, no tras el primer error de
   ACL.

5. **`run_on_main_thread()` llamado ya desde el hilo principal se ejecuta de forma reentrante e inline —
   puede colgar la creación de una ventana nueva.** Si un `#[tauri::command]` síncrono ya se despacha sobre
   el hilo principal (depende de la versión/configuración de Tauri — verificarlo con
   `std::thread::current().id()`, no asumirlo), llamar a `run_on_main_thread()` desde dentro de ese comando
   no produce un salto de hilo real: el cierre se ejecuta anidado dentro del propio despacho del mensaje IPC
   que lo originó. Crear una `WebviewWindowBuilder` ahí cuelga `.build()` indefinidamente (su inicialización
   asíncrona necesita que el bucle de mensajes siga bombeando, y no puede mientras ese mismo hilo procesa el
   mensaje exterior). Si necesitas de verdad un hilo distinto desde un comando (a diferencia de un callback
   de plugin, que sí llega en una iteración nueva del bucle), despacha explícitamente desde
   `tauri::async_runtime::spawn(async move { ... })` antes de llamar a `run_on_main_thread()`.

6. **Dos caminos async independientes desde Rust hacia el mismo frontend no garantizan orden de llegada.**
   Si un comando `invoke()` y un evento disparado por un watcher/observador en segundo plano (file watcher,
   etc.) pueden ambos notificar al frontend sobre el mismo cambio, el evento del observador puede llegar
   *antes* de que se resuelva la promesa del `invoke` que lo causó. Cualquier estado que dependa de "ya
   terminé esta operación" (p. ej. una ventana de supresión para no reaccionar a tu propio cambio) debe
   fijarse de forma optimista en el punto donde se *inicia* la operación, no en el callback de éxito —
   revertirlo en el `.catch()` si la operación falla de verdad.

7. **Cada entrada de `"windows"` en `tauri.conf.json` exige `"label"` explícito.** Sin él, la aplicación se
   cierra inmediatamente al arrancar, sin mensaje de error obvio que apunte a la causa.

8. **En macOS, "Abrir con" desde Finder no pasa por `argv` — solo por `RunEvent::Opened`.** Leer
   `std::env::args()` para saber qué archivo abrir funciona en Windows (el Explorador lo pasa como argumento
   literal) pero no existe ese mecanismo en macOS: Finder entrega la apertura como un Apple Event
   `kAEOpenDocuments`, expuesto en Tauri v2 exclusivamente vía `tauri::RunEvent::Opened { urls }` (requiere
   `.build(...)?.run(closure)` en vez de `.run(...)` directo para poder interceptarlo). El evento llega
   *antes* de que exista cualquier ventana, así que hay que guardar la ruta en un estado gestionado y
   recogerla al crear la ventana principal, no asumir que ya habrá una ventana lista para recibirla.

9. **Un mismo permiso web puede exigirse en un motor WebView y no en otro.** `window.print()` funciona sin
   permiso adicional en WebView2 (Windows), pero WKWebView (macOS) exige explícitamente
   `core:webview:allow-print` en `capabilities/*.json` — sin él, `Cmd+P`/el botón de imprimir falla en
   silencio solo en Mac. No asumas que un permiso probado en una plataforma cubre las otras dos: revisa la
   tabla de diferencias de motores (WebView2/WebKitGTK/WKWebView) contra cada API nueva que uses, no solo al
   final.

Para el patrón de CI que compila cada plataforma y las particularidades de cada tienda de apps, ver
[`NATIVE_APPS_RELEASE_CI.md`](./NATIVE_APPS_RELEASE_CI.md) y [`MARKETPLACE_PUBLISHING.md`](./MARKETPLACE_PUBLISHING.md).
