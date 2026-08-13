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

Para el patrón de CI que compila cada plataforma y las particularidades de cada tienda de apps, ver
[`NATIVE_APPS_RELEASE_CI.md`](./NATIVE_APPS_RELEASE_CI.md) y [`MARKETPLACE_PUBLISHING.md`](./MARKETPLACE_PUBLISHING.md).
