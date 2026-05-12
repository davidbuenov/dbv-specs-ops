# Changelog — dbv-specs-ops

All notable changes to this framework are documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [Sin publicar] / [Unreleased]

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

[Sin publicar]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.5.0...HEAD
[1.5.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.4.0...v1.5.0
[1.4.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.3.0...v1.4.0
[1.3.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.2.1...v1.3.0
[1.2.1]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/davidbuenov/dbv-specs-ops/releases/tag/v1.0.0
