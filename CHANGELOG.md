# Changelog — dbv-specs-ops

All notable changes to this framework are documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [Sin publicar] / [Unreleased]

---

## [2.8.0] — 2026-09-01

Integración curada del [AI-Native SDLC Playbook (Anthropic)](https://claude.com/blog/the-ai-native-sdlc-playbook): cierre autónomo del loop, revisión de código por pases con severidad y guardarraíles deterministas — todo agnóstico de proveedor y opcional salvo el gate de revisión que ya existía. Incluye además un fix de usabilidad reportado en el uso real del framework: los comandos de fase (`/build`, etc.) escritos sin texto adicional no se interpretaban de forma fiable.

### Added
- **`docs/MAINTAIN.md` — Fase 7 (opcional, desactivada por defecto).** Cierra el ciclo Spec→Ship sin
  disparo humano: un detector determinista (CI/cron sobre una métrica) invoca a la IA en modo solo lectura
  para diagnosticar una desviación y redactarla como entrada `[Detectado automáticamente]` en
  `SPECIFICATIONS.md`, que reentra el ciclo por `/plan`. Nunca despliega ni hace merge por sí sola.
- **`docs/REVIEW.md`.** Tres pases de revisión con severidad (Bugs / Seguridad / Cumplimiento,
  Crítico/Importante/Nit) enganchados a `/code-simplify`; lo Crítico bloquea `/ship`. El pase Cumplimiento
  audita explícitamente, función por función, los `<coding_standards>` de `MASTER_PROMPT.md` (un solo
  `return` + guard clauses, patrón Result, tipado estricto) — hasta ahora esa regla estaba declarada
  "obligatoria" pero ningún paso de revisión la comprobaba, y en la práctica se seguía viendo código con
  varios `return` dispersos.
- **`docs/GUARDRAILS.md`.** Distingue instrucciones *advisory* (`MASTER_PROMPT.md`) de guardarraíles
  deterministas a nivel de git/CI (pre-commit, branch protection) que se mantienen aunque el modelo olvide
  una regla. Incluye un ejemplo concreto de heurística pre-commit para la regla de un solo `return`.
- **`docs/PARALLEL_WORK.md`.** Formaliza el Modo Orquestador ya existente con mecánica concreta de
  `git worktree` para 2-3 sesiones de IA independientes en paralelo.
- **`docs/SOURCE_OF_TRUTH.md`.** Patrón de convivencia cuando el proyecto ya usa Jira/ServiceNow/etc.
  antes de adoptar dbv-specs-ops.
- **`docs/METRICS.md`.** Indicadores leading/lagging opcionales por fase, legibles solo desde git.
- **`evals/` + `scripts/run-evals.sh`.** Suite de regresión opcional para la propia configuración del
  agente (`MASTER_PROMPT.md`, ficheros de activación) — no valida el código del proyecto, valida que el
  proceso se sigue produciendo igual tras un cambio de configuración.
- **`.claude/commands/{spec,plan,build,test,code-simplify,ship,maintain}.md`.** Comandos nativos de
  Claude Code (con autocompletado) para cada fase del ciclo.

### Fixed
- **Comandos de fase escuetos mal interpretados.** Escribir solo `/build` (sin texto adicional) podía
  responderse como si el código no existiera, obligando a reformular como frase completa. Corregido con una
  regla explícita en `docs/MASTER_PROMPT.md` (`<workflow>`): un comando de fase escueto siempre significa
  "ejecuta ya esa fase", cascadeando automáticamente por las fases previas que falten en vez de rechazar la
  petición; reforzado en Claude Code con comandos nativos reales (`.claude/commands/`).

### Changed
- **`docs/MASTER_PROMPT.md`** — enganches a los 6 documentos nuevos en `<workflow>` (`/plan` Modo
  Orquestador → `PARALLEL_WORK.md`; `/code-simplify` → `REVIEW.md`; `/ship` → gate de hallazgos Crítico;
  Fase 7 documentada al final), `<boundaries>` (→ `GUARDRAILS.md`) y `<context_management>` (→
  `SOURCE_OF_TRUTH.md`).
- **`README.md` dividido en dos ficheros independientes de un solo idioma** — `README.md` (español,
  principal) y `README.en.md` (inglés), en vez de un único fichero bilingüe con secciones EN/ES mezcladas.
  Cada uno incluye: badge de versión, Key Features, Origin & Inspiration (nueva entrada citando el playbook
  de Anthropic), diagrama `mermaid` de flujo actualizado con el nodo opcional de Maintain (Fase 7) y el
  pase de revisión por capas en Simplify, tabla de fases con recuadro de Fase 7 opcional, tabla de ficheros
  de `docs/`, mención de `evals/`/`scripts/`, nota de `.claude/commands/` para usuarios de Claude Code, y un
  enlace cruzado de cambio de idioma en la cabecera de cada fichero.
- **`docs/README.md`** — tabla e índice de flujo con los 6 documentos nuevos.
- **`docs/UPGRADE_PROMPT.md`** — manifest v2.8.0, nuevas URLs de descarga y mensaje de cierre actualizado.

---

## [2.7.0] — 2026-08-28

Consolidación de lecciones, patrones y trampas técnicas contrastadas en múltiples aplicaciones nativas publicadas en tiendas oficiales de distribución (Microsoft Store, Apple App Store, Uptodown, etc.). Todo lo que sigue surge de horas de depuración en entornos reales de producción y pruebas multiplataforma.

### Added
- **`docs/NATIVE_DESKTOP_APPS.md` §7 — Definición de Hecho (DoD) de Experiencia de Escritorio.** Una app
  compilada con Tauri todavía no es una app de escritorio: es una web dentro de un marco. La diferencia son
  siempre los mismos seis detalles (diálogos de archivo nativos, iconografía completa generada desde un
  único `app-icon.svg`, atajos universales que funcionen **también con el foco dentro de un input**, menú
  nativo en macOS, scrollbars tematizadas y layout fluido, tooltips que anuncian los atajos), y se elevan a
  criterios de aceptación en vez de pulido opcional. Incluye dos reglas de verificación: lanzar el
  ejecutable real (no dar por buena una compilación) y mantener la versión sincronizada en `package.json`,
  `tauri.conf.json`, `Cargo.toml` y el "Acerca de" de la UI. **Esta sección se dio por escrita en un ADR de
  proyecto en agosto y nunca llegó al framework** — el mismo fallo de backport que ya se corrigió con el
  menú de macOS.
- **`docs/WEB_TO_DESKTOP_MIGRATION.md` §9 — la ruta con bundler (React/Vue/Svelte + Vite).** El patrón sin
  bundler de `NATIVE_DESKTOP_APPS.md` §3 no aplica ahí, y aparecen tres problemas propios: listeners de
  eventos nativos suscritos en un `useEffect` con dependencias vacías que capturan handlers obsoletos (el
  menú nativo acaba guardando contenido antiguo), el linter recorriendo el JS generado por Cargo dentro de
  `src-tauri/target/`, y código muerto de detección de entorno duplicando la capa de adaptación de §3.1.
- **`docs/NATIVE_APPS_RELEASE_CI.md` §6bis — los inputs de una Action de terceros cambian entre versiones y
  un input inválido no rompe el build.** GitHub Actions solo avisa, así que una opción mal nombrada queda
  sin efecto en silencio. La fuente fiable es el aviso `Unexpected input(s)` del primer run real, que
  enumera los inputs válidos de la versión que de verdad se ejecutó — no la documentación.

### Changed
- **`docs/NATIVE_DESKTOP_APPS.md` §6 — cuatro trampas nuevas** (de 10 a 14): `zoomHotkeysEnabled` viene
  desactivado por defecto en Tauri v2 (`Ctrl`+rueda no hace nada aunque funcione en el navegador); un build
  que dice `Finished` sin haber dicho `Compiling` conserva los assets del frontend anteriores, porque
  `generate_context!` los embebe en tiempo de compilación; `document.title` **no** sirve como sonda para
  saber si el JS se ejecuta (el título de la ventana nativa lo fija `tauri.conf.json`), y la sonda que sí
  funciona es un `window.addEventListener('error', ...)` inline en el `<head>`; y cambiar solo un recurso
  incrustado (un `.ico`) no invalida la caché de Cargo, hace falta `cargo clean -p <crate> --release`.
- **`docs/NATIVE_DESKTOP_APPS.md` §4 punto 4 — quién genera la clave de firma del updater.** El comando
  `tauri signer generate` lo ejecuta el usuario en su propia terminal, nunca la IA, para que la password no
  pase por el contexto ni por los logs del agente; y esa password va a un gestor de contraseñas, nunca a un
  fichero junto a la clave.
- **`docs/MARKETPLACE_PUBLISHING.md` §3 — la carpeta de empaquetado generada se trackea entera.** El
  instinto de gitignorar `src-tauri/gen/windows/` y conservar solo la configuración es justo el error: sus
  assets pueden necesitar corrección manual (el placeholder negro que provocó un rechazo real de Microsoft),
  y gitignorados esa corrección se pierde en silencio y el asset roto vuelve.
- **`docs/NATIVE_APPS_RELEASE_CI.md` §6 — la variante local del fallo de firma.** Encadenar
  `tauri build && <paso siguiente>` hace que el paso siguiente nunca se ejecute en un build sin variables de
  firma, sin ningún error que lo explique porque el instalador sí se generó. Un orquestador `spawnSync` que
  ejecute siempre ambos pasos y combine los códigos de salida es más fiable que `&&`.
- **`docs/WEB_TO_DESKTOP_MIGRATION.md` §8 — checklist de migración** ampliada con la DoD de escritorio, la
  verificación sobre el ejecutable real y la sincronización de versión en los cuatro ficheros.

---

## [2.6.0] — 2026-08-22

### Added
- **Estrategia de Migración de Apps Web Existentes a Escritorio Nativo**:
  - Nueva guía [docs/WEB_TO_DESKTOP_MIGRATION.md](./docs/WEB_TO_DESKTOP_MIGRATION.md), que cubre las decisiones **previas** a `NATIVE_DESKTOP_APPS.md` cuando el código web ya existe y ya tiene usuarios: clasificación en 4 arquetipos (estática pura / SPA con bundler / servidor local ligero / servidor local pesado) como paso 0 que determina coste y estrategia; dirección de la adopción (la plantilla viaja hacia el repo existente, nunca al revés, para no perder historial, issues y URLs); un repo por app frente a monorepo; modo dual escritorio+web como opción por defecto con el patrón de **capa de adaptación única** (`api.js` con detección `window.__TAURI__` y enrutado a `invoke()` o `fetch`) como el único punto donde el coste del modo dual se concentra; regla de decisión **Rust vs sidecar aplicada por función, no por aplicación**; montaje del sidecar (`bundle.externalBin`, cierre explícito del proceso hijo, puerto no cableado, congelado en el runner de CI de cada plataforma); el coste oculto del sidecar de ML sobre el tamaño del instalador con 3 estrategias de provisionamiento; auditoría de licencias copyleft como decisión arquitectónica previa a invertir; orden de migración por **riesgo de tubería** en portfolios de varias apps; y checklist de migración de 12 puntos.
  - `docs/README.md`: índice y diagrama de flujo de documentos actualizados con el nuevo documento, situado **antes** de `NATIVE_DESKTOP_APPS.md` en el flujo.
- **`docs/NATIVE_DESKTOP_APPS.md` §3 — la IIFE es obligatoria en TODOS los ficheros JS propios**, no solo
  el principal, incluidos los "ficheros de utilidades que solo definen funciones": los scripts clásicos
  comparten un único ámbito global, así que dos ficheros que declaren el mismo identificador en su nivel
  superior hacen morir al segundo entero con un `SyntaxError` de **parseo** — y al ser de parseo, ninguna
  línea de ese fichero llega a ejecutarse, ni sus listeners ni sus handlers de error. El síntoma (página
  que renderiza perfecta con la interfaz completamente muerta y sin ningún error visible) cuesta horas si
  no se sabe buscar. Añadida además la técnica de depuración: registrar `window.onerror` /
  `unhandledrejection` en un `<script>` inline sin `defer` en el `<head>`, antes de cualquier script
  externo — un capturador definido dentro del fichero que falla nunca llega a registrarse.
- **`docs/WEB_TO_DESKTOP_MIGRATION.md` — dos gotchas reales contrastados en migraciones completas a producción:**
  - **§1 (Arquetipo A), aviso sobre `frontendDist`:** "apunta a la carpeta y ya" deja de ser cierto si
    `src-tauri/` vive dentro de esa misma carpeta (migración in-place, típico cuando la raíz del repo ya la
    publica GitHub Pages). Tauri embebe entonces recursivamente `src-tauri/target/...`: build roto por lock
    de Cargo, o peor, ventana en negro con `ERR_CONNECTION_REFUSED` sin relación aparente con la causa.
    Documentado el patrón `scripts/sync-frontend.mjs` (copia el frontend a `src-tauri/frontend/` gitignored,
    enganchado a `beforeDevCommand`/`beforeBuildCommand`) como solución.
  - **§3.1, ejemplo de capa de adaptación renombrado de `isTauri` a `runningInTauri`:** con
    `withGlobalTauri: true` (obligatorio para el patrón sin bundler de `NATIVE_DESKTOP_APPS.md` §3), Tauri
    v2 ya declara un global `isTauri` propio; declarar `const isTauri = ...` en un script clásico choca con
    él y mata el fichero entero con el mismo `SyntaxError` de parseo silencioso que ya advierte §3 — solo
    que aquí el segundo declarante es el propio runtime de Tauri, no un fichero propio.
- **Integración de Phase Gates en el Master Prompt**:
  - `docs/MASTER_PROMPT.md`: Bootstrap §7 obliga a resolver las 4 decisiones previas de `WEB_TO_DESKTOP_MIGRATION.md` antes de proponer stack cuando ya existe código web funcionando.
  - `docs/MASTER_PROMPT.md`: Nuevo **Gate de migración web → escritorio** en `/plan` (Paso 3), que exige registrar por escrito arquetipo, repositorio de destino, modo dual vs sustitución y decisión Rust/sidecar por función — más estrategia de provisionamiento y auditoría de licencias si hay sidecar, **antes** de escribir código.
- **Actualización de Índices y Herramientas de Migración**:
  - `README.md`: tablas de documentos (EN y ES) actualizadas.
  - `docs/UPGRADE_PROMPT.md`: Manifest actualizado a v2.6.0.
  - `project.config.md`: Versión incrementada a `2.6.0`.

---

## [2.5.1] — 2026-08-21

### Added
- **`docs/NATIVE_DESKTOP_APPS.md`**:
  - Nueva §6 "Trampas concretas de Tauri v2 — permisos, WebView y threading": 8 gotchas reales de permisos (`core:window:allow-destroy` para `onCloseRequested`, glob de `capabilities.*.windows` para ventanas creadas en tiempo de ejecución), `window.confirm()`/`window.alert()` asíncronos y rotos en `tauri-plugin-dialog` 2.7.2, caché agresiva de WebView2 entre relanzamientos del proceso, reentrancia de `run_on_main_thread()` llamado ya desde el hilo principal, orden no garantizado entre la respuesta de un `invoke()` y un evento de watcher en segundo plano, `label` obligatorio por ventana, y `RunEvent::Opened` para asociación de ficheros en macOS (no vale leer `argv`).
  - Ítem 9 añadido: `core:webview:allow-print` exigido por WKWebView (macOS) para `window.print()`, sin equivalente en WebView2 (Windows) — un permiso probado en una plataforma no cubre necesariamente las otras dos.
  - §5 ampliada con la diferencia de comportamiento de asociación de archivos entre `.deb` (declarativo vía `fileAssociations`, no verificable sin hardware Linux real) y `.AppImage` (portátil por diseño, sin integración automática sin `AppImageLauncher` — limitación del formato, no un bug).
- **`docs/MARKETPLACE_PUBLISHING.md`**:
  - Nueva §6 "NSIS (instalador Windows) — trampas reales de personalización": qué es alcanzable por configuración declarativa (`sidebarImage`/`headerImage`, `installerHooks`, `bundle.publisher`) frente a lo que exige forkear la plantilla `.nsi` completa (texto de páginas, checkboxes de componentes propios), `XPStyle` para temas activos de Windows, `fileAssociations` sin opt-in nativo, falta de `SHChangeNotify` tras registrar la asociación, ProgId huérfano entre versiones, y caché de build que no reincrusta un icono regenerado.
  - Nueva §7 "MSIX / identidad de paquete para tiendas": coincidencia obligatoria entre el nombre de binario compilado y el nombre visible del manifiesto, y reserva de nombres técnicos adicionales en la consola de la tienda cuando difieren del nombre comercial.
- **`docs/NATIVE_APPS_RELEASE_CI.md`**:
  - Nueva sección con plantillas completas y copiables de workflow de GitHub Actions (`release-windows.yml`, `release-linux.yml`, `release-macos.yml`) validadas contra ejecuciones reales — no solo fragmentos sueltos como en versiones anteriores del documento.
- **Actualización de Herramientas de Migración**:
  - `docs/UPGRADE_PROMPT.md`: Manifest actualizado a v2.5.1.
  - `project.config.md`: Versión incrementada a `2.5.1`.

---

## [2.5.0] — 2026-08-13

### Added
- **Soporte Nativo para Aplicaciones de Escritorio Compiladas (Rust + Tauri v2)**:
  - Nueva guía técnica y de arquitectura [docs/NATIVE_DESKTOP_APPS.md](file:///d:/Programacion/github-davidbuenov/dbv-specs-ops/docs/NATIVE_DESKTOP_APPS.md) con Tauri v2 como stack de referencia, arquitectura Core Rust vs Frontend Web, patrón "sin bundler" y 8 lecciones de diseño transferibles (sanitización en capa final, file watching por directorio, instancia única multi-ventana, par de claves de auto-actualizador, comprobación no bloqueante en arranque, desactivación de updater en ejecuciones desde tienda/`WindowsApps`, i18n ligero y persistencia JSON).
- **Patrones de CI/CD Multiplataforma para Binarios Nativos (GitHub Actions)**:
  - Nueva guía técnica [docs/NATIVE_APPS_RELEASE_CI.md](file:///d:/Programacion/github-davidbuenov/dbv-specs-ops/docs/NATIVE_APPS_RELEASE_CI.md) con patrones para compilar en runners específicos por SO (`ubuntu-*`, `macos-*`, `windows-*`), patrón de Releases como borrador acumulativo (`releaseDraft: true`), lectura de versión desde config JSON, permisos explícitos de `GITHUB_TOKEN` (`contents: write`), resolución del runner Apple Silicon (`aarch64`) vs. target `universal-apple-darwin` y gestión de artefactos sin firma como deuda técnica consciente.
- **Guía y Checklist para Publicación en Marketplaces**:
  - Nueva guía operativa [docs/MARKETPLACE_PUBLISHING.md](file:///d:/Programacion/github-davidbuenov/dbv-specs-ops/docs/MARKETPLACE_PUBLISHING.md) con comparativa de canales de distribución (Self-hosted vs. Tienda curada con auto-firma MSIX en Microsoft Store vs. Firma Authenticode/Apple vs. Catálogos de terceros como Uptodown), auditoría de empaquetadores de terceros, checklist pre-certificación contra placeholders silenciosos de color sólido y formularios por tienda.
- **Integración de Phase Gates en el Master Prompt**:
  - `docs/MASTER_PROMPT.md`: Añadida la 4ª opción de stack en Bootstrap §7 (Rust + Tauri v2).
  - `docs/MASTER_PROMPT.md`: Añadido Phase Gate de app nativa compilada en `/plan` (Paso 3) para exigir la definición de la matriz de CI multiplataforma.
  - `docs/MASTER_PROMPT.md`: Añadido Phase Gate de publicación en marketplace en `/ship` (Paso 6) para exigir la ejecución del checklist de `MARKETPLACE_PUBLISHING.md` antes de cerrar la entrega.
- **Actualización de Índices y Herramientas de Migración**:
  - `docs/README.md` y `README.md`: Tablas e índices de documentación actualizados en español e inglés con las 3 nuevas guías condicionales.
  - `docs/UPGRADE_PROMPT.md`: Manifest actualizado a v2.5.0.
  - `project.config.md`: Versión incrementada a `2.5.0`.

### Added
- **Soporte Nativo de Agent Plugins 1.0.0**:
  - Integración completa del estándar universal **Agent Plugins 1.0.0** (Google, Amazon, Microsoft, OpenAI, Vercel) para empaquetar Agent Skills y servidores MCP.
  - Nueva guía técnica detallada [docs/AGENT_PLUGINS.md](file:///d:/Programacion/github-davidbuenov/dbv-specs-ops/docs/AGENT_PLUGINS.md) que explica el diseño, manifiestos (`plugin.json`), transporte (`mcp.json`), aislamiento y autodescubrimiento.
  - Actualización del checklist de `Agent Readiness` en la fase `/spec` para recomendar y estructurar la interfaz externa de descubrimiento web bajo el directorio estandarizado `.well-known/agent-plugin/`.
  - Integración de directivas en `/test` para validar sintáctica y semánticamente que `plugin.json` y `mcp.json` cumplan estrictamente sus respectivos esquemas oficiales y reglas de portabilidad (uso de `${PLUGIN_ROOT}` y `${PLUGIN_DATA}`).
- **Asistente de Migración Activa en Upgrade Prompt**:
  - Incorporada una fase opcional en [docs/UPGRADE_PROMPT.md](file:///d:/Programacion/github-davidbuenov/dbv-specs-ops/docs/UPGRADE_PROMPT.md) que escanea el proyecto en busca de skills (`agent-skills/` o locales sueltas) o tarjetas (`agent.json`/`mcp.json`) antiguas y ofrece migrarlas automáticamente al estándar unificado de Agent Plugins, asegurando portabilidad de rutas MCP.
- **Actualización de Plantillas**:
  - `docs/SPECIFICATIONS.md`: Checklist de Agent Readiness v2.4.0 actualizado para usar Agent Plugins.
  - `docs/ARCHITECTURE.md`: Sección de Arnés del Agente alineada a la especificación de Agent Plugins.
  - `project.config.md`: Versión de framework actualizada a `2.4.0`.
  - `README.md`: Documentadas las aportaciones de la v2.4.0 y añadidas referencias al estándar Agent Plugins en español e inglés.

---

## [2.3.0] — 2026-07-29

### Added
- **Soporte para Enriquecimiento y Auditoría de Diseño (Design Enrichment)**:
  - Nueva guía técnica [docs/DESIGN_ENRICHMENT.md](file:///d:/Programacion/github-davidbuenov/dbv-specs-ops/docs/DESIGN_ENRICHMENT.md) detallando el flujo de trabajo con las herramientas de diseño **Impeccable** y **SkillUI**.
  - Paso de decisión interactivo en `/spec` para ofrecer la instalación acotada de Impeccable (filtrando proveedores activos) y extracción de tokens con SkillUI.
  - Gestión automatizada y sincronización del archivo `DESIGN.md` en la raíz del proyecto durante la fase `/ship`, manteniendo la fuente de verdad aislada en `dbv-specs-ops/docs/DESIGN.md`.
  - Integración de directivas en `/test` para recomendar auditorías de diseño con `/impeccable critique` (para verificar contraste WCAG AA, heurísticas y usabilidad).
  - Integración de directivas en `/code-simplify` para sugerir comandos de pulido visual y robustez de UI con `/impeccable polish` e `/impeccable harden`.
  - Referencias cruzadas en `docs/SPECIFICATIONS.md` y soporte en `docs/UPGRADE_PROMPT.md` para migrar de forma automática a v2.3.0.

---

## [2.2.0] — 2026-07-26

### Added
- **Soporte de instalación y empaquetado para proyectos de Python y Node.js**:
  - **Python (pip)**: El ciclo de vida en la fase `/build` genera automáticamente un archivo `pyproject.toml` (cumpliendo PEP 621) o `setup.py` mínimo para permitir la instalación del proyecto del usuario localmente (`pip install .` o `pip install -e .`).
  - **Node.js (npm)**: El ciclo de vida en la fase `/build` genera un archivo `package.json` funcional con scripts de inicio (`start`) y pruebas (`test`) para permitir `npm install`.
- **Plantilla de README (`README.template.md`) renovada**:
  - Sección `Installation` mejorada que divide la instalación local del proyecto (tanto de Python como de Node.js) y la instalación desde un repositorio git remoto (`pip install git+https://...` y `npm install git+https://...`).
  - Nueva sección `Publishing` que guía paso a paso sobre cómo publicar los paquetes en registros oficiales (PyPI con `twine` y npm registry con `npm publish`).
- **Empotrado de directrices de buenas prácticas de código**: Para garantizar su cumplimiento incluso en entornos offline o aislados, se integraron las reglas clave de `ai-coding-best-practices` (UN SOLO return + Guard Clauses, Result Pattern, Tipado Estricto, Validación en Fronteras y Excepciones Específicas) directamente en `<coding_standards>`.
- **Recomendaciones de tecnologías por defecto en bootstrap**: Se añadió un paso en la Fase 0 (Bootstrap) para proponer stacks estándar profesionales preseleccionados por el framework (FastAPI en Python, Express/TypeScript en Node, React/Tailwind, PostgreSQL/SQLite y uv/pnpm) si el usuario no tiene preferencia.
- **Reglas de modularidad en Frontend**: Añadidas directivas obligatorias para impedir que la IA vuelque todo el código de React en un solo fichero `App.js`/`App.tsx`, exigiéndole estructurar componentes de única responsabilidad en subcarpetas (`components`, `hooks`, etc.).

### Changed
- **Unificación del método de instalación (Solo Subcarpeta)**:
  - Se eliminan las opciones A y B de instalación/adopción en la documentación de `README.md`. A partir de ahora, el framework se integra siempre dentro de un subdirectorio dedicado `dbv-specs-ops/` para evitar colisiones y mantener limpia la raíz del proyecto.
  - Se detallan las instrucciones y el contenido exacto de los archivos de activación en la raíz (`CLAUDE.md`, `.github/copilot-instructions.md`, `.windsurfrules`, `GEMINI.md`) para forzar a los asistentes de IA a leer la configuración del subdirectorio.
- **Diferenciación estricta de contexto en `docs/MASTER_PROMPT.md` y `docs/ADOPTION_PROMPT.md`**:
  - Se han añadido directivas explícitamente claras en el prompt para obligar a los agentes de IA a que todas las especificaciones, arquitectura, backlog de tareas y READMEs que generen hagan referencia **exclusivamente a la aplicación del usuario** y nunca detallen la estructura interna, fases o tecnologías del propio framework de `dbv-specs-ops`.

## [2.1.0] — 2026-06-17

### Added
- **Integración de Agent Readiness (Preparación para Agentes)**: Soporte nativo en el framework para proyectos web y APIs públicas. El ciclo de desarrollo ahora guía de forma guiada la creación de:
  - `robots.txt` con directiva de exclusión y Content-Signals (`ai-train=no, search=yes, ai-input=yes`).
  - Mapa de navegación semántica (`llms.txt` y `auth.md` para flujos de registro anónimo/OAuth).
  - Metadatos de descubrimiento OIDC/OAuth y firmas en `.well-known/` (`api-catalog`, `oauth-protected-resource`, `oauth-authorization-server` e `http-message-signatures-directory`).
  - Tarjetas de bot y servidor MCP (`agent.json` y `mcp.json`).
  - Estructura y repositorio de habilidades (`agent-skills/` con su índice `index.json` y guías `SKILL.md`).
  - Mecanismos de negociación de contenido Markdown (`Accept: text/markdown`) y cabeceras `Link` HTTP.
- **Actualización de Plantillas**:
  - `project.config.md`: Campo de configuración `Agent Readiness (Web)` añadido a la identidad de proyecto.
  - `docs/SPECIFICATIONS.md`: Checklist integrado en la sección de propuesta técnica y nuevo riesgo de consumo de contexto.
  - `docs/ARCHITECTURE.md`: Sección de Interfaz Externa para Agentes en el Arnés del Agente.
- **Actualización del Ciclo de Vida (`MASTER_PROMPT.md`)**:
  - Fase `/spec` evalúa viabilidad de Agent Readiness.
  - Fase `/build` guía la creación estructurada de ficheros.
  - Fase `/ship` verifica cabeceras Link inyectadas.

### Changed
- **`README.md`**: Añadido apartado inicial con la descripción detallada y diferencial de las características principales del framework en inglés y español.

### Fixed
- **`README.md`**: Corregida la contradicción en el Quickstart donde se sugería el comando `/plan` como primer mensaje en vez de `/spec` (que inicia el ciclo SDD de forma correcta).

---

## [2.0.0] — 2026-06-15

### Added
- **Ingeniería Agéntica (Agentic Engineering)**: Integración completa de los conceptos del libro blanco de Google *"The New SDLC With Vibe Coding"*.
- **Modos de Trabajo Implícitos**: Clasificación automática e invisible entre modo *Conductor* (IDE interactivo de loops cortos) y modo *Orquestador* (ejecución asíncrona de fondo) durante la fase `/plan`.
- **Evals Unificados en `/test`**: La fase `/test` ahora cubre tanto tests deterministas clásicos como Evals probabilísticos (rúbricas de output, trayectoria y verificación de alucinaciones).
- **Auditoría de Seguridad en `/code-simplify`**: Fase obligatoria de revisión del código generado por IA para prevenir dependencias falsas (slopsquatting), inyección de secretos e inputs vulnerables.
- **Agent Harness (Arnés del Agente)**: Transición de la antigua sección de MCP a una especificación de arnés en `docs/ARCHITECTURE.md` (definiendo contexto estático vs dinámico, sandboxing y hooks de seguridad).
- **Guía de Fundamento**: Nuevo documento [docs/AGENTIC_ENGINEERING.md](file:///d:/Programacion/github-davidbuenov/dbv-specs-ops/docs/AGENTIC_ENGINEERING.md) detallando el razonamiento teórico detrás de v2.0.0.
- **Model Routing Guidelines**: Directivas de asignación óptima de modelos de IA en `project.config.md` para reducir el coste operativo (OpEx/Token Burn).
- **Sugerencia Activa de MCPs y Skills**: Mandato en `/spec` y `/plan` para proponer la creación de servidores MCP y habilidades dinámicas locales en los proyectos.
- **Upgrade Prompt Actualizado**: Se actualizó [docs/UPGRADE_PROMPT.md](file:///d:/Programacion/github-davidbuenov/dbv-specs-ops/docs/UPGRADE_PROMPT.md) para incluir la versión `2.0.0` y permitir que los proyectos existentes se actualicen automáticamente sin quedar bloqueados en la versión v1.5.0.

---

## [1.5.2] — 2026-05-12

### Added
- **Trust Boundary (`<trust_boundary>`):** Nueva sección en `MASTER_PROMPT.md` que declara explícitamente la separación entre directivas válidas (etiquetas XML del prompt) y datos del proyecto (`SPECIFICATIONS.md`, `task.md`, `memory.md`). Previene la ejecución silenciosa de texto imperativo inyectado en ficheros de datos.
- **Memory Triggers Granulares:** En `/plan`, si el Adversarial Review acepta un riesgo conscientemente, debe registrarse en `memory.md` de inmediato. En `/build`, si se contradice `ARCHITECTURE.md`. En `/test`, si un test invalida un supuesto de `SPECIFICATIONS.md`. No es necesario esperar a `/ship`.
- **Política de Mantenimiento en `memory.md`:** Nueva sección `🧹 Política de Mantenimiento` con el objetivo de mantener el fichero por debajo de 200 líneas activas, instrucciones de consolidación y uso de `memory.archive.md`.

### Fixed
- **Adversarial Review anti-plantilla:** El bloque `<adversary>` ahora debe citar al menos un sustantivo concreto presente en `docs/SPECIFICATIONS.md`. Se eliminan respuestas genéricas del tipo «¿qué pasa si falla la red?».
- **Bootstrap sin auto-ejecución de git:** El flujo de bootstrap ya no ejecuta `git init` automáticamente. Muestra el comando y solicita confirmación explícita del usuario.
- **Frontera memory/task clarificada:** `memory.md` ahora distingue explícitamente que `task.md` es operativo (paso exacto siguiente) y `memory.md` es temático (contexto cualitativo, área del producto en foco).

---

## [1.5.1] — 2026-05-12

### Fixed
- **Inconsistencia documental EN en `README.md`:** La sección "Adopting an Existing Project" describía el flujo de entrevista antiguo ("6 questions, one at a time"). Actualizado al nuevo flujo de borrador con asunciones marcadas.
- **Inconsistencia en `project.config.md`:** El callout de descripción del archivo decía "the AI will ask 3 quick questions". Actualizado para reflejar el flujo real (borrador completo con `[ASSUMPTION: ...]`).
- **Ficheros de evaluación interna en `.gitignore`:** Añadidos `docs/EvaluacionChatGPT.md`, `docs/EvaluacionCopilot.md` y `docs/EvaluaciondeClaude.md` al `.gitignore` para que no se versionen.

---

## [1.5.0] — 2026-05-12

### Added
- **Enforcement Layer (Hard-Law):** Transición de metodologías sugeridas a contratos estrictos.
- **Phase Gates en`/plan`**: `implementation_plan.md` ahora exige OBLIGATORIAMENTE un Frontmatter YAML definiendo dependencias, riesgos y estrategia de rollback.
- **Memory Gate en `/ship`**: Añadido paso obligatorio de impresión de `<memory_update_proposal>` XML antes de cerrar tareas, forzando la persistencia de contexto cualitativo en `memory.md`.
- **Adversarial Review (`/plan`)**: Implementado un debate interno forzado (`<builder>` vs `<adversary>`) en formato XML para evaluar casos límite antes de que la IA desglose tareas.

### Fixed
- **Redundancia Cognitiva:** Refactorización DRY extrema en los adaptadores (`CLAUDE.md`, `GEMINI.md`, `.windsurfrules`, `.github/copilot-instructions.md`, `ANTIGRAVITY.md`). Se han eliminado las reglas duplicadas sobre estado del proyecto, ciclo de vida y bootstrap. Ahora actúan como punteros ligeros hacia `docs/MASTER_PROMPT.md` para reducir carga cognitiva en el LLM.
- **Fricción en Entrevista Inicial:** El proceso de `bootstrap` y adopción (`MASTER_PROMPT.md`, `ADOPTION_PROMPT.md`) ya no hace preguntas una a una de forma tediosa. Ahora la IA propone un borrador completo con asunciones marcadas (`[ASSUMPTION: ...]`) que el usuario valida o corrige en un solo paso.
- **Limpieza de template:** `task.md` restaurado a un estado limpio de plantilla (antes arrastraba meta-historia del framework).
- **Consistencia:** Añadido `memory.md` a las tablas de lectura inicial de todos los adaptadores.
- **Prevención de ruido:** Añadidas instrucciones explícitas en `memory.md` para que la IA borre los ejemplos al crear contenido real.
- Corrección del encabezado de `MASTER_PROMPT.md` para reflejar la versión actual.

---

## [1.4.0] — 2026-05-12

### Added
- **`memory.md`** — nuevo archivo estándar para separar el contexto cualitativo (decisiones técnicas, lecciones aprendidas, mapa de relaciones) del progreso cuantitativo (`task.md`), previniendo la deriva arquitectónica y pérdida de contexto en la IA.
- **Architect Review en fase `/plan`** — al invocar `/plan`, la IA ahora hace una validación previa asumiendo el rol de Software Architect para detectar *edge cases* y vulnerabilidades lógicas antes de desglosar tareas.

### Changed
- **XML Prompts** — `docs/MASTER_PROMPT.md`, `docs/ADOPTION_PROMPT.md` y `docs/UPGRADE_PROMPT.md` han sido completamente reescritos utilizando etiquetas XML semánticas (`<workflow>`, `<boundaries>`, etc.) para mejorar la obediencia en modelos como Claude 3.5, Gemini 1.5 y GPT-4o.
- **`README.md`** — actualizado para incluir `memory.md` en la estructura de archivos e incluir el *Architect Review* en la definición de la fase `/plan`.
- **`GEMINI.md`** — actualizado para incluir la lectura de `memory.md` al inicio de la sesión.
- **`.gitignore`** — actualiza para incluir la ignorancia de `implementation_plan.md` y `walkthrough.md`.

---

## [1.3.0] — 2026-05-05

### Added
- **`docs/DESIGN.md`** — plantilla de sistema de diseño visual (design tokens YAML + prosa) siguiendo el estándar [design.md de Google Labs](https://github.com/google-labs-code/design.md). Incluye soporte nativo de **Dark Mode** como aportación propia al estándar.
- **`docs/UPGRADE_PROMPT.md`** — agente de actualización del framework: detecta la versión actual del proyecto, calcula el delta de cambios, descarga los ficheros de framework desde GitHub (con fallback manual si no hay red) y nunca toca los ficheros de proyecto.
- **`framework_version`** en `project.config.md` — nuevo campo para rastrear con qué versión del framework fue inicializado o actualizado el proyecto por última vez.

### Changed
- **`GEMINI.md`, `CLAUDE.md`, `ANTIGRAVITY.md`, `.windsurfrules`, `.github/copilot-instructions.md`** — todos actualizados para leer `docs/DESIGN.md` al inicio de sesión (si existe) y mencionar el sistema de diseño en la sección de adaptación.
- **`docs/MASTER_PROMPT.md`** — fase `/spec` actualizada: si el proyecto tiene UI y `docs/DESIGN.md` no existe, crearlo en esta fase.
- **`docs/SPECIFICATIONS.md`** — sección 4 enlaza a `docs/DESIGN.md` cuando el proyecto tiene interfaz de usuario.
- **`README.md`** — nuevas secciones "Upgrading an Existing Project" (EN) y "Actualizar el Framework" (ES) con flujo de descarga única de `UPGRADE_PROMPT.md`. Secciones de estructura de ficheros, origen e inspiración, adopción y referencias actualizadas en ambos idiomas.
- **`docs/README.md`** — `DESIGN.md` y `UPGRADE_PROMPT.md` añadidos al índice y al diagrama de flujo de documentos.

## [1.2.1] — 2026-04-30

### Added
- **`README.template.md`** — base skeleton for user projects.
- **Bootstrap Language question** — a 5th question added to the initial interview to choose the project's documentation language (ES, EN, or Bilingual).
- **Project README auto-generation** — the AI now generates a customized `README.md` for the user project using the template and then deletes the `.template` file.
- **Reference Section for Phases** — detailed table in the main README explaining the 6 development phases (/spec, /plan, /build, /test, /code-simplify, /ship) and their trigger commands.

---

## [1.2.0] — 2026-04-30

### Added
- **`project.config.md`** — new root file for project identity: name, author/company, license and file header templates for all languages (JS, Python, HTML, CSS, Java, C#, Go).
- **Bootstrap interview** — at session start, the AI detects placeholders in `project.config.md` and asks 4 questions one by one before the Engineering Interview:
  1. Project name
  2. Author / company (+ optional URL)
  3. License (MIT by default)
  4. Git version control (⭐ HIGHLY RECOMMENDED, enabled by default)
- **File headers** — every new source file must include a copyright header adapted to the language. The `dbv-specs-ops` credit line is mandatory in all headers.
- **`LICENSE` auto-generation** — the AI generates the `LICENSE` file after the bootstrap interview based on the chosen license.
- **Git integration** — if the user accepts Git in bootstrap: `git init`, `.gitignore` generation (stack-aware), and first commit (`chore: project initialized with dbv-specs-ops`). On each `/ship`: Conventional Commits message, version tag, and push suggestion (not auto-executed).
- **`CHANGELOG.md` auto-management** — the AI accumulates entries in `[Unreleased]` during `/build` and `/test`, and publishes them under a new versioned section on each `/ship`.
- **Semantic versioning on `/ship`** — the AI presents a clear 4-option menu: Patch / Minor ✅ / Major / No change.
- **Multiplatform startup scripts on `/ship`** — always generates `start.cmd` + `stop.cmd` (Windows) and `start.sh` + `stop.sh` (macOS/Linux), with automatic `venv` activation for Python projects.
- **Python `venv`** — Python projects always get a local virtual environment (`venv/`) created before installing dependencies in `/build`.

### Changed
- **Quick Start (EN + ES)** completely rewritten: per-platform table with exact first message to type (`/plan`), explicit Antigravity guidance, clear new vs. existing project paths.
- **Step 1** clarified: two safe options (GitHub Template button and Download ZIP), with explicit warning not to clone directly.
- **`GEMINI.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, `.windsurfrules`** — all updated with `project.config.md` in the startup reading list and two-step state detection (bootstrap check → specs check).
- **`MASTER_PROMPT.md`** — bootstrap section, `/build` CHANGELOG rule, `/test` CHANGELOG rule, and full `/ship` protocol added.
- **README file structure tables (EN + ES)** — `project.config.md` and `CHANGELOG.md` added to the root table.

---

## [1.1.0] — 2026-04-26

### Added
- **Bilingual README** (English / Spanish) with full Table of Contents and HTML anchors for navigation.
- **`ANTIGRAVITY.md`** — dedicated setup file for Antigravity (VS Code · Google DeepMind): Planning Mode artifacts, Knowledge Items, and Context Snapshot instructions.
- **Windsurf support** — `.windsurfrules` file for automatic context loading in Windsurf.
- **Visual Workflow diagram** (Mermaid) added to README.
- **`docs/ADOPTION_PROMPT.md`** — flow for adopting SDD in existing projects without specs.
- **`docs/README.md`** — index of all files in the `/docs` folder.

### Changed
- Core documentation files renamed to English for universal AI compatibility: `ESPECIFICACIONES.md` → `SPECIFICATIONS.md`, `ARQUITECTURA.md` → `ARCHITECTURE.md`.
- Artifact names standardized: `task.md`, `implementation_plan.md`, `walkthrough.md` — compatible with Antigravity's native Planning Mode.
- `GEMINI.md` updated with Antigravity-specific behavior section (Planning Mode + Knowledge Items).
- Credit line for David Bueno Vallejo added consistently across all platform files.

---

## [1.0.0] — 2026-04-15

Initial public release of the **dbv-specs-ops** SDD framework.

### Added
- **`docs/MASTER_PROMPT.md`** — the brain of the system: Senior Engineer rules, Spec→Plan→Build→Test→Simplify→Ship cycle, development standards and boundaries.
- **`docs/SPECIFICATIONS.md`** — template for project requirements: context, objectives, users, features, out-of-scope, risks and open questions.
- **`docs/ARCHITECTURE.md`** — template for technical decisions: stack, directory structure, key decisions, integrations and MCP section.
- **`task.md`** — logbook template with backlog, in-progress tracking and Context Snapshot for session continuity.
- **`CLAUDE.md`** — automatic context loading for Claude Code, Claude Desktop and Cursor.
- **`GEMINI.md`** — automatic context loading for Gemini CLI and Antigravity.
- **`.github/copilot-instructions.md`** — automatic context loading for GitHub Copilot.
- **`project.config.md`** placeholder for project identity.
- **`README.md`** — project documentation with origin, workflow and usage instructions.
- **`LICENSE`** — MIT License.

---

## How to read this file

- **Added** — new features or files.
- **Changed** — changes to existing functionality or documentation.
- **Deprecated** — features that will be removed in a future release.
- **Removed** — features removed in this release.
- **Fixed** — bug fixes.
- **Security** — security vulnerability fixes.

---

[Sin publicar]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.8.0...HEAD
[2.8.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.7.0...v2.8.0
[2.7.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.6.0...v2.7.0
[2.6.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.5.1...v2.6.0
[2.5.1]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.5.0...v2.5.1
[2.5.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.4.0...v2.5.0
[2.4.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.3.0...v2.4.0
[2.3.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.2.0...v2.3.0
[2.2.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.1.0...v2.2.0
[2.1.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.5.2...v2.0.0
[1.5.2]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.5.1...v1.5.2
[1.5.1]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.5.0...v1.5.1
[1.5.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.4.0...v1.5.0
[1.4.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.3.0...v1.4.0
[1.3.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.2.1...v1.3.0
[1.2.1]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/davidbuenov/dbv-specs-ops/releases/tag/v1.0.0
