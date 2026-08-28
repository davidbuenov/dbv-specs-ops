# 🧠 Memory & Context

> **Frontera de uso (Memory vs. Tasks):**
> - `task.md` → progreso **operativo**: checklist de tareas, Snapshot de Contexto (el paso exacto siguiente), y estado de la sesión.
> - `memory.md` → contexto **cualitativo y temático**: conocimiento persistente, decisiones técnicas profundas, lecciones, y el área del producto en foco (no el paso específico).
> Si hay info que sirva para los dos, prioriza: datos con fecha/paso exacto → `task.md`; razonamiento/por-qué/lecciones → `memory.md`.
>
> *Instrucción para la IA: Consulta este archivo al inicio de cada sesión para recuperar el hilo técnico. Actualiza las secciones correspondientes cuando el workflow lo indique (triggers en `/plan`, `/build`, `/test` y gate en `/ship`).*

## 🎯 Contexto Activo
- **Estado actual del desarrollo:** Release v2.7.0 finalizado y verificado. Sincronización documental completa, generalización de lecciones aprendidas de producción y listo para cierre de versión y publicación.
- **Foco inmediato:** Publicación y etiquetado (`v2.7.0`), y sincronización en proyectos que consuman el framework.

## 🏗️ Log de Decisiones Técnicas (ADR Ligero)
*Registro de por qué se tomaron ciertas rutas (ej. cambios en librerías, arquitectura o patrones).*

- **2026-06-15 - Transición a dbv-specs-ops v2.0.0 (Agentic Engineering):** Implementación de los principios del libro blanco de Google. Se unificaron los Evals no deterministas de IA en la fase `/test` para simplificar el flujo, y se añadió la auditoría de seguridad en `/code-simplify` para evitar la fuga de credenciales o de paquetes alucinados (*slopsquatting*). Se transicionó la sección MCP en la arquitectura a una definición explícita de Arnés (Harness) del Agente.
- **2026-07-29 - Integración de Enriquecimiento de Diseño (v2.3.0):** Adición opcional de Impeccable y SkillUI. Para mantener la subcarpeta como única fuente de verdad sin romper compatibilidad con herramientas de raíz, `dbv-specs-ops/docs/DESIGN.md` sigue siendo la fuente de verdad, y se copia a la raíz como un archivo derivado. Se automatizó la sincronización del archivo de la raíz en la fase `/ship` para evitar desajustes o ediciones inconsistentes.
- **2026-08-07 - Adopción de Agent Plugins 1.0.0 (v2.4.0):** Integración completa del estándar universal de empaquetado para herramientas MCP y Agent Skills. Se unificaron los directorios de autodescubrimiento web bajo `.well-known/agent-plugin/` y se implementó un asistente de migración en `UPGRADE_PROMPT.md` para trasladar automáticamente proyectos antiguos con configuraciones ad-hoc a esta estructura portable, traduciendo rutas locales absolutas a los placeholders `${PLUGIN_ROOT}` y `${PLUGIN_DATA}`.
- **2026-08-13 - Integración de Apps de Escritorio Nativas, CI Multiplataforma y Marketplaces (v2.5.0):** Incorporación de guías operativas (`NATIVE_DESKTOP_APPS.md`, `NATIVE_APPS_RELEASE_CI.md`, `MARKETPLACE_PUBLISHING.md`) basadas en la experiencia real con Tauri v2, GitHub Actions y Microsoft Store / Uptodown. Se añadieron opciones de stack de escritorio nativo en el Bootstrap §7 de `MASTER_PROMPT.md` y dos Phase Gates (verificación de CI multiplataforma en `/plan` y checklist de publicación pre-envío en `/ship`) manteniendo el framework 100% modular y no invasivo para proyectos web.
- **2026-08-21 - Endurecimiento de Desktop Apps y Plantillas de CI Completas (v2.5.1):** Generalización de lecciones aprendidas reales en producción: 9 gotchas concretos de Tauri v2 (permisos `allow-destroy`/`allow-print`, `confirm` asíncrono, caché WebView2, glob en `capabilities`, reentrancia `run_on_main_thread`, `RunEvent::Opened` en macOS), guía de personalización de instaladores NSIS vs. forkeo de plantillas, requisitos de identidad MSIX, y adición de 3 plantillas YAML completas y operativas para GitHub Actions (`release-{windows,linux,macos}.yml`).
- **2026-08-22 - Estrategia de Migración Web → Desktop y Phase Gates (v2.6.0):** Creación de `WEB_TO_DESKTOP_MIGRATION.md` para sistematizar las decisiones previas a envolver aplicaciones web existentes con Tauri v2: clasificación en 4 arquetipos (estática, SPA con bundler, backend ligero, backend pesado/ML), dirección de adopción hacia el repo existente, modo dual con capa de adaptación única (`runningInTauri`), regla de decisión Rust vs. sidecar por función, prevención del embebido recursivo de `frontendDist` y dos Phase Gates en `MASTER_PROMPT.md` (Bootstrap §7 y `/plan` Paso 3).
- **2026-08-28 - Cosecha de Lecciones de Producción y DoD de Escritorio (v2.7.0):** Consolidación de conocimiento práctico derivado de múltiples aplicaciones nativas publicadas en tiendas oficiales: Definición de Hecho (DoD) de escritorio nativo con 6 criterios de aceptación obligatorios (diálogos de archivo nativos, iconografía desde `app-icon.svg`, atajos universales en inputs, menú nativo macOS, layout fluido y tooltips), 4 trampas adicionales de Tauri v2 (`zoomHotkeysEnabled`, caché de assets embebidos y de Cargo en `.ico`, sondas inline válidas frente a `document.title`), obligatoriedad de trackear la carpeta de empaquetado `gen/windows/`, generación segura de claves de firma por el usuario, orquestación `spawnSync` en CI local y verificación de inputs en Actions de terceros.

## ⚠️ Lecciones Aprendidas / Errores Evitados
*Notas sobre bugs específicos, configuraciones que fallaron o refactors intentados para no repetirlos.*

- **[Feedback de Usabilidad]**: Es mejor integrar los conceptos nuevos (como Evals) en las fases existentes (`/test`) y delegar los modos de ejecución (Conductor/Orquestador) de forma implícita, en lugar de sobrecargar al desarrollador final con configuraciones complejas o preguntas confusas.
- **[Estructura de Onboarding]**: En proyectos existentes con archivos raíz consolidados (como `README.md` y `CHANGELOG.md`), es preferible descargar el framework completo en una subcarpeta dedicada (`dbv-specs-ops/`) e indicar al agente su ubicación a través de un archivo de activación ligero (`CLAUDE.md`, `GEMINI.md`). Esto evita colisiones de archivos y mantiene limpio el código de producción.
- **[Preferencia de Commit]**: Para los mensajes de git commit en este proyecto, usar siempre el formato conciso "Version v.X.Y.Z" (ejemplo: `Version v.2.7.0`) en lugar de mensajes largos de Conventional Commits.


## 🗺️ Mapa de Relaciones
*Breve descripción de cómo interactúan los módulos actuales para ayudar a la IA a navegar el código.*

- **[Módulo/Componente]:** [Responsabilidad y Dependencias]
- *Ejemplo: `auth_service.js` gestiona el JWT y depende de `api_client.js`. (Borra esta línea de ejemplo al crear la primera entrada real).*

---

## 🧹 Política de Mantenimiento

*Aplicar en cada `/ship` de tipo Major, o cuando este fichero supere las 200 líneas activas:*

- **Consolida** decisiones relacionadas en una sola entrada.
- **Archiva** lecciones ya internalizadas en el código: muévelas a `memory.archive.md` (créalo si no existe).
- **Elimina** entradas que describan decisiones revertidas o ya obsoletas.
- **Objetivo:** mantener `memory.md` por debajo de ~200 líneas activas para que la IA pueda leerlo íntegramente en cada sesión sin pérdida de atención.
