<!--
  Author: David Bueno Vallejo (@davidbuenov)
  Repository: https://github.com/davidbuenov/dbv-specs-ops
  License: MIT
  Purpose: Comparativa técnica entre dbv-specs-ops y otros frameworks de Spec-Driven Development (SDD).
-->

<p align="right">🇪🇸 Español · <a href="./COMPARISON.en.md">🇬🇧 Read in English</a></p>

# 📊 Comparativa: `dbv-specs-ops` vs. otros frameworks SDD

El ecosistema de **Spec-Driven Development (SDD)** busca resolver el problema principal del *"vibe coding"*: cuando un asistente de IA programa sin rumbo, pierde el contexto entre sesiones, alucina dependencias o rompe funcionalidades existentes.

[**GitHub Spec Kit**](https://github.com/github/spec-kit), [**BMAD Method**](https://github.com/bmad-code-org/BMAD-METHOD) y [**Kiro**](https://kiro.dev/) cubren con solvencia el flujo especificar → planificar → implementar y, según el caso, también revisar y verificar. **`dbv-specs-ops` (v3.1.0)** comparte ese flujo y añade a su alrededor el resto del **ciclo de vida de la ingeniería de software (6+1 fases)**: la inicialización sin CLI, la trazabilidad en el código fuente, la preparación para agentes/GEO, las aplicaciones de escritorio multiplataforma, la publicación en tiendas oficiales y el mantenimiento post-despliegue.

> **Leyenda:** ✅ Sí · ⚠️ Parcial o con otro enfoque · ❌ No consta en su documentación oficial · ➖ Fuera de su alcance (no pretende cubrirlo)

---

## 🔗 Fuentes oficiales

Las celdas de los otros tres frameworks se han contrastado con su documentación oficial el **15/10/2026**; las filas *Puertas entre fases* y *Memoria de enfoques descartados*, el **16/10/2026**. Cuando una página no detalla algo, la celda lo dice. Son herramientas que evolucionan rápido: si detectas algo desfasado, abre un issue.

| Framework | Web oficial | Páginas consultadas |
| :--- | :--- | :--- |
| **GitHub Spec Kit** | [github.com/github/spec-kit](https://github.com/github/spec-kit) · [documentación](https://github.github.io/spec-kit/) | [README](https://github.com/github/spec-kit) · [referencia de comandos](https://github.github.io/spec-kit/reference/overview.html) · [Agentic SDD](https://github.github.io/spec-kit/reference/agentic-sdd.html) · [CLI `core`](https://github.github.io/spec-kit/reference/core.html) · [actualización](https://github.github.io/spec-kit/upgrade.html) · [proyectos existentes](https://github.github.io/spec-kit/guides/existing-projects.html) · [concepto SDD](https://github.github.io/spec-kit/concepts/sdd.html) |
| **BMAD Method** | [github.com/bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) · [documentación](https://docs.bmad-method.org/) | [README](https://github.com/bmad-code-org/BMAD-METHOD) · [instalación](https://docs.bmad-method.org/start/install-bmad/) · [revisión de cambios](https://docs.bmad-method.org/build/review-a-change/) · [bucles autónomos](https://docs.bmad-method.org/build/autonomous-development-loops/) · [contexto del proyecto](https://docs.bmad-method.org/existing-codebases/set-and-maintain-project-context/) · [skills y agentes](https://docs.bmad-method.org/reference/skills-and-agents/) · [módulo Test Architect](https://bmad-code-org.github.io/bmad-method-test-architecture-enterprise/) |
| **Kiro (AWS)** | [kiro.dev](https://kiro.dev/) | [Portada](https://kiro.dev/) · [Specs](https://kiro.dev/docs/specs/) · [Correctness](https://kiro.dev/docs/specs/correctness/) · [Steering](https://kiro.dev/docs/steering/) · [Hooks](https://kiro.dev/docs/hooks/) · [Licencia](https://kiro.dev/license/) |

> **Versión de BMAD:** su documentación actual describe la **v7** (en alfa, con salida prevista el 16/10/2026), mientras que la versión estable es la **v6.12**. Donde difieren, la comparativa sigue la documentación oficial vigente y lo indica.

---

## 🤝 Lo que comparten todos

Antes de las diferencias, lo que los cuatro hacen bien y de forma parecida:

| Característica común | **dbv-specs-ops** | [**GitHub Spec Kit**](https://github.com/github/spec-kit) | [**BMAD Method**](https://github.com/bmad-code-org/BMAD-METHOD) | [**Kiro (AWS)**](https://kiro.dev/) |
| :--- | :---: | :---: | :---: | :---: |
| **Especificar y planificar antes de codificar** | ✅ Spec → Plan → Build | ✅ `specify` → `plan` → `tasks` → `implement` | ✅ `bmad-spec` / `bmad-prd` → `bmad-architecture` → `bmad-build` | ✅ Requisitos → diseño → tareas |
| **Artefactos de especificación como ficheros del proyecto** | ✅ `docs/SPECIFICATIONS.md` | ✅ `spec.md`, `plan.md`, `tasks.md` | ✅ PRD, specs y `DESIGN.md` / `EXPERIENCE.md` | ✅ `requirements.md`, `design.md`, `tasks.md` |
| **Desglose del trabajo en tareas o historias** | ✅ `task.md` | ✅ `tasks.md` ordenado por dependencias | ✅ Iniciativas, épicas y tickets (`bmad-ticket`) | ✅ Lista de tareas con ejecución individual o en bloque |
| **Reglas de proyecto persistentes para la IA** | ✅ `MASTER_PROMPT.md` + `memory.md` | ✅ Constitución (`/speckit.constitution`) | ✅ `bmad-project-context` (bloque verificado en `AGENTS.md`) | ✅ Ficheros *steering* (`.kiro/steering/`) |

---

## 🏆 Lo que los diferencia

| Categoría / Característica | **dbv-specs-ops (v3.1.0)** | [**GitHub Spec Kit**](https://github.com/github/spec-kit) | [**BMAD Method**](https://github.com/bmad-code-org/BMAD-METHOD) (v6.12 / v7 alfa) | [**Kiro (AWS)**](https://kiro.dev/) |
| :--- | :---: | :---: | :---: | :---: |
| **Filosofía y Alcance** | Ciclo de vida completo (6+1 fases): de la idea al código, tiendas y mantenimiento | Flujo SDD guiado por una CLI y por comandos que ejecuta tu agente, ampliable con extensiones, presets y workflows | Agentes (Analyst, PM, Architect, Developer, UX Designer) y *skills* para planificar, construir y revisar; módulos opcionales (Test Architect, Game Dev…) | Agente de IA con IDE (Code OSS), CLI, web y móvil; flujo de specs con *hooks* y *steering* |
| **Instalación (Bootstrap)** | **1 frase en lenguaje natural**, sin instalar nada (requiere un asistente con shell y red; en chat web se pega `MASTER_PROMPT.md` a mano) | Requiere Python 3.11+, `uv` y la CLI `specify` (`uv tool install specify-cli`) | Un comando `npx` (Node 20.12+, Python 3.10+ y `uv`); en la v7 alfa, también como plugin de Claude Code o Codex | Instalador del IDE o de la CLI; también hay versión web y móvil |
| **Personalización y Cabeceras de Código** | ✅ **Sí (`project.config.md`)**: inyección automática de autor, licencia, fecha y propósito en cada archivo de cualquier lenguaje | ❌ No consta (los *presets* adaptan el comportamiento, no las cabeceras de código) | ❌ No consta | ❌ No consta |
| **Trazabilidad en Código (`@spec`)** | ✅ **Sí**: requisitos (`REQ-01`) citados en código y tests con `@spec` | ⚠️ `/speckit.analyze` (solo lectura) cruza spec, plan y tareas, p. ej. "a task with no matching requirement"; no consta enlace requisito → código o tests | ⚠️ Módulo opcional Test Architect: su skill *Trace* ("Map requirements to tests and decide a release gate") | ⚠️ Enlace trazable requisito ↔ propiedad ↔ tarea ("traceable link between your requirements and the tests that validate them"), solo en el IDE y opcional |
| **Linter Determinista propio** | ✅ **Sí**: Python estándar (0 deps). Trazabilidad `@spec` en varios lenguajes (py, ts/js, rs, go, java, kt, cs…); regla de un solo `return` **solo en Python**; flag `--changed` para código legado | ❌ No consta (`analyze` y `converge` los ejecuta tu agente) | ❌ No consta | ❌ No consta (los *hooks* pueden ejecutar tu linter o tus tests al guardar un fichero) |
| **Agent Readiness y Web GEO / AEO** | ✅ **Sí**: prepara webs para buscadores IA (GEO) y agentes con `llms.txt`, `Content-Signals`, `.well-known` y Agent Plugins 1.0.0 / MCP | ➖ Fuera de su alcance | ➖ Fuera de su alcance | ➖ Fuera de su alcance |
| **Apps de Escritorio y Tiendas Oficiales** | ✅ **Sí**: Arquitectura Rust/Tauri v2 multiplataforma (Win/Mac/Linux) + guías para **Microsoft Store, Homebrew y Uptodown** | ➖ Fuera de su alcance | ➖ Fuera de su alcance | ➖ Fuera de su alcance |
| **Arquitectura "AI-Native" de fábrica** | ✅ **Sí (`v2.9.0+`)**: Plantillas y arquitectura lista para conectar IA Local, APIs Cloud o Agentes (ACP/MCP) con keyring seguro del SO | ➖ Fuera de su alcance | ➖ Fuera de su alcance | ➖ Fuera de su alcance |
| **Scripts de arranque y parada (`start` / `stop`)** | ✅ **Sí**: genera `start.cmd` / `stop.cmd` (Windows) y `start.sh` / `stop.sh` (macOS/Linux) para servidores, APIs y servicios residentes; en Python activan solos el entorno virtual | ➖ Fuera de su alcance | ➖ Fuera de su alcance | ➖ Fuera de su alcance |
| **Gestión de Git y Releases** | ✅ **Sí**: desde `git init` hasta `/ship` (actualiza `CHANGELOG.md` y `walkthrough.md`, propone el commit y el tag `vx.y.z`; sugiere el push y lo ejecuta cuando se lo pides) | ⚠️ Parcial: la inicialización de git y las ramas son una extensión opcional (`specify extension add git`), no instalada por defecto; `/speckit.taskstoissues` exporta tareas como issues; no consta gestión de releases ni changelog | ⚠️ Parcial: `bmad-build-auto` hace commits pero no push; no consta ninguna skill de versionado, ramas o releases | ⚠️ Parcial: Kiro Web coordina cambios en repos de GitHub y GitLab y la CLI sin interfaz puede revisar PRs en CI/CD; no consta versionado ni changelog |
| **Revisión Adversarial y Anti-Slopsquatting** | ✅ **Sí**: autoevaluación de confianza (casos límite, seguridad, concurrencia) + 3 pases en `REVIEW.md` (incluye detección de paquetes alucinados) | ⚠️ Puertas de calidad `clarify`, `checklist` y `analyze` ("anything with meaningful ambiguity"); no consta revisión adversarial | ⚠️ `bmad-code-review` (varios revisores independientes y triaje por severidad) y `bmad-review`, que admite lentes como *adversarial*, *edge case* o *verification gap* | ⚠️ *Analyze Requirements* (IDE y CLI) detecta "inconsistencies, ambiguities, and gaps" antes del diseño |
| **Validación: Tests + AI Evals** | ✅ **Sí**: tests unitarios/integración + **Evals probabilísticos** para funciones de IA (la IA los diseña y ejecuta; el framework no trae un runner propio para los Evals de tu proyecto) | ⚠️ `/speckit.tasks` genera tests dentro de cada historia "cuando se piden" (when requested); la extensión de bugs exige verificación ("Missing verification is not a successful fix") | ⚠️ `bmad-qa-generate-e2e-tests` y el módulo opcional Test Architect (8 skills: Automate, Test Review, Test Design, Framework, NFR, Trace…) | ⚠️ Tests basados en propiedades (*Correctness*, solo en el IDE y opcionales por defecto) y estrategia de tests en el diseño |
| **Diseño UI y Accesibilidad** | ✅ **Sí (opcional)**: `design.md` (Google Labs), heurísticas *Impeccable* e ingeniería inversa *SkillUI* | ❌ No consta | ⚠️ `bmad-ux` y el agente UX Designer (`DESIGN.md`, `EXPERIENCE.md`); la accesibilidad no se menciona | ❌ No consta |
| **Memoria de decisiones y Sesiones Paralelas** | ✅ **Sí**: persistencia dual (`memory.md` + `task.md`), compresión Hot/Warm/Cold y *Scope Contracts* para agentes en paralelo | ⚠️ Constitución del proyecto; `/speckit.implement` respeta marcadores de paralelismo en `tasks.md` | ⚠️ `bmad-project-context` (*refresh*, *record*, *audit*); los flujos paralelos de épicas requieren "a higher layer" | ⚠️ *Steering* persistente (global y de workspace, 4 modos de inclusión); tareas en paralelo por *waves* y agentes en paralelo (local o nube) |
| **Puertas entre fases (grafo de fases)** | ✅ **Sí (v3.1.0)**: `PROCEDURAL_GRAPH.json` describe cada transición con `condition`, `guidance` y `pitfalls`, incluidas las vueltas atrás (Plan → Spec, Test → Build, Simplify → Build); cada comando recibe solo el contexto de su fase, generado desde el grafo y vigilado por el linter | ⚠️ "Run them in order, adding the clarify/checklist/analyze quality gates for anything with meaningful ambiguity": puertas de calidad opcionales, sin condiciones de entrada/salida formales | ⚠️ Algunas skills tienen pasos con puertas (p. ej. `bmad-project-context` enseña el bloque antes de escribir); no consta un formato general de condiciones entre fases | ⚠️ Las *Feature Specs* tienen puertas de aprobación entre requisitos, diseño y tareas (la *Quick Spec* las omite) |
| **Memoria de enfoques descartados** | ✅ **Sí (v3.1.0)**: *Rejection Memory* en `memory.md` (`REJECTED-NN`), consultada antes de proponer en `/plan`, `/test`, `/code-simplify` y `/maintain`, como evidencia y no como orden | ❌ No consta | ⚠️ `bmad-project-context` registra *observed pitfalls* ("A pitfall goes in only when someone has seen the mistake"): errores vistos, no enfoques descartados | ❌ No consta (no se describe que los comentarios de *Markdown Review* persistan) |
| **Mantenimiento Post-Entrega (Loop)** | ✅ **Sí (Fase 7 `Maintain`, opcional y desactivada por defecto)**: monitores en CI/cron que diagnostican en solo lectura y reabren specs | ⚠️ Extensión opcional de corrección de bugs (*assess*, *fix*, *validate*), lanzada por el usuario | ⚠️ `bmad-build-auto`: bucle desatendido de construcción y revisión; sin monitorización post-entrega | ⚠️ Specs de *bugfix* (`bugfix.md`) y CLI sin interfaz para CI/CD |
| **Actualización no destructiva** | ✅ **Sí**: Actualiza versiones del framework sin tocar tus especificaciones ni tu código | ✅ `specify self upgrade` + `specify integration upgrade`: no toca `specs/` ni la constitución; si editaste ficheros gestionados, se detiene y te pregunta (o usa `--force`) | ⚠️ `bmad setup` actualiza módulos y migra `_bmad/custom/`; la v7 es alfa y "can break an existing setup" | ➖ No consta un procedimiento de migración de ficheros; se actualiza como producto |
| **Soporte Multi-Asistente y `AGENTS.md`** | ✅ **Sí**: Estándar abierto `AGENTS.md`, Claude Code, Gemini CLI, Copilot, Cursor y Windsurf | ✅ 38 integraciones (Copilot, Gemini, Codex, Claude, Zed, Kiro…) más una `generic` | ⚠️ Cualquier herramienta con soporte de *skills* (la documentación cita Claude Code, Codex y Cursor); usa `AGENTS.md` para el contexto del proyecto | ⚠️ Es su propio agente (IDE, CLI, web, móvil) con modelos de Anthropic, OpenAI y de pesos abiertos; lee `AGENTS.md`, Skills y MCP |
| **Licencia** | Open Source (MIT) | Open Source (MIT) | Open Source (MIT) | IDE y CLI licenciados como "AWS Content" bajo el AWS Customer Agreement; precio por créditos; Kiro Crew es de código abierto |

---

## ⚖️ Donde `dbv-specs-ops` va por detrás

Una comparativa honesta también enseña lo que no se hace mejor.

| Categoría | **dbv-specs-ops** | [**GitHub Spec Kit**](https://github.com/github/spec-kit) | [**BMAD Method**](https://github.com/bmad-code-org/BMAD-METHOD) | [**Kiro (AWS)**](https://kiro.dev/) |
| :--- | :---: | :---: | :---: | :---: |
| **Comunidad y madurez** (estrellas en GitHub, 15/10/2026) | ❌ **29 ⭐** · 1 autor · repo creado en abril de 2026 | ✅ **141.379 ⭐** · repo de agosto de 2025 | ✅ **54.174 ⭐** · repo de abril de 2025 | ➖ Producto comercial de AWS |
| **Ecosistema y extensibilidad** | ⚠️ Guía de Agent Plugins y comandos nativos de Claude Code; sin catálogo de extensiones ni módulos | ✅ Extensiones, presets, workflows y bundles, con catálogo comunitario | ✅ Módulos oficiales (Builder, Creative Intelligence, Test Architect, Game Dev…) y `bmad-customize` | ✅ *Hooks*, *steering*, Skills, MCP y ACP |
| **Automatización determinista integrada** | ⚠️ Los guardarraíles de git/CI son opcionales y se montan a mano (`docs/GUARDRAILS.md`) | ⚠️ Extensiones y workflows | ⚠️ Skills y bucles desatendidos | ✅ *Hooks* que ejecutan comandos de shell ante eventos (`PostFileSave`, `PreToolUse`, `PostToolUse`, `Stop`) |
| **IDE / interfaz visual integrada** | ❌ Sin interfaz propia: depende del asistente que uses (en Claude Code, comandos `/spec`, `/plan`…) | ❌ | ❌ | ✅ IDE con paneles de specs, hooks y steering, además de web y móvil |

En estrellas, `dbv-specs-ops` es el último de los tres proyectos de código abierto, y con diferencia. Y estas son las limitaciones concretas del framework:

1. **Reglas consultivas por defecto.** El propio `MASTER_PROMPT.md` lo dice en `<boundaries>`: sus reglas son *advisory* y dependen de que la IA las siga. El linter, los hooks de pre-commit y los guardarraíles de CI son opcionales y hay que configurarlos (`docs/GUARDRAILS.md`); si no se activan, el cumplimiento depende de la IA.
2. **Coste de contexto.** `MASTER_PROMPT.md` ocupa unos 38 KB (222 líneas; del orden de 10 mil tokens, estimación) y se carga junto a specs, `task.md` y `memory.md` en cada sesión. En proyectos pequeños o cambios mínimos es sobrecarga: el Modo Conductor y la compresión Hot/Warm/Cold lo mitigan, pero no lo eliminan.
3. **El linter comprueba presencia, no corrección.** Verifica que cada requisito tenga su etiqueta `@spec` en código y tests, que no haya IDs duplicados ni etiquetas a requisitos inexistentes. No verifica que la implementación cumpla realmente el requisito: eso lo hacen los tests.
4. **Linter centrado en Python.** La regla de un solo `return` se verifica por AST solo en `.py`; en JS/TS hay una heurística en bash y en Rust se apoya en `clippy`. Solo la trazabilidad `@spec` cubre varios lenguajes.
5. **Estándares opinados y obligatorios.** Un solo `return`, patrón `Result`, validación con Pydantic/Zod y un estilo de cabeceras concreto están marcados como de obligado cumplimiento. Pueden chocar con el estilo idiomático de un equipo (por ejemplo, el uso de *early returns*).
6. **Los Evals de tu proyecto no tienen herramienta propia.** El framework exige que la IA diseñe y ejecute Evals para los componentes de IA, pero no trae un runner: la carpeta `evals/` solo valida la configuración del propio framework.
7. **El arranque en una frase necesita un asistente con shell y red.** En ChatGPT o Gemini web hay que pegar `MASTER_PROMPT.md` a mano.
8. **Idioma.** El prompt maestro y varias guías están solo en español. Los README, esta comparativa y algunas plantillas tienen versión en inglés, pero no el núcleo del framework.
9. **El grafo de fases es un resumen escrito a mano, no aprendido.** En el paper que lo inspira ([*Procedural Graphs*](https://arxiv.org/abs/2609.09153), Tabla 2), un grafo experto sin bucle de evolución llegó a empeorar los resultados en una de sus pruebas. Aquí se mitiga con un grafo pequeño, fiel a `<workflow>` y verificado por el linter, pero su efecto sobre la calidad no está medido: la suite `evals/` tiene hoy un solo eval.

---

## 🔍 Análisis detallado de las ventajas de `dbv-specs-ops`

### 1. Verificación determinista (`@spec` + Linter) frente a confianza ciega en el prompt
En los otros frameworks, la coherencia entre requisitos, tareas y código la comprueba el propio agente de IA (por ejemplo `/speckit.analyze` o `/speckit.converge` en Spec Kit, o la skill *Trace* de BMAD). `dbv-specs-ops` introduce un **linter propio en Python estándar (cero dependencias externas)** que verifica de forma determinista que cada requisito (`REQ-01`, `REQ-02`...) esté citado con `@spec` tanto en la implementación como en sus tests automatizados, y que audita la regla de un solo `return` por función en Python. Con el flag `--changed`, permite adoptarlo en proyectos existentes auditando únicamente los archivos modificados.

### 2. Personalización y propiedad intelectual en cada fichero (`project.config.md`)
Independientemente del lenguaje de programación (Python, Rust, TypeScript, C++, etc.), `dbv-specs-ops` inyecta automáticamente cabeceras estandarizadas en cada archivo nuevo con los metadatos del autor, sitio web, licencia y propósito técnico definidos en `project.config.md`.

### 3. Preparación nativa para GEO (Generative Engine Optimization) y Agentes
Cuando se construye una aplicación web con `dbv-specs-ops`, el proyecto queda preparado de serie para que los motores de respuesta generativos y agentes autónomos puedan descubrirlo, leerlo e interactuar con él mediante `llms.txt`, directivas `Content-Signals`, metadatos en `.well-known` y el estándar **Agent Plugins 1.0.0 / MCP**.

### 4. De la Web al Escritorio Multiplataforma y Tiendas Oficiales
Para proyectos de escritorio, proporciona una ruta completa basada en **Rust + Tauri v2** para compilar instaladores nativos y firmados en **Windows, macOS y Linux**, incluyendo la configuración y guías de despliegue en **Microsoft Store**, **Homebrew Tap** (`brew install`) y **Uptodown**.

### 5. Aplicaciones "AI-Native" listas de serie
Si el software que estás creando necesita ofrecer capacidades de IA a sus usuarios finales, el framework incorpora `docs/AI_DESKTOP_ARCHITECTURE.md` y las plantillas bilingües `AI_SPECIFICATIONS` e `IA.md`. Deja lista la arquitectura para conectar con **IAs locales** (Ollama, LM Studio, modelos locales), **agentes CLI vía protocolo ACP/MCP** o **APIs Cloud**, almacenando credenciales en el *keyring* nativo del sistema operativo y aplicando guardarraíles contra *prompt injection*.

### 6. Revisión Adversarial y protección contra *Slopsquatting*
Antes de escribir código, la IA debe superar una revisión adversarial de su propio plan: cita evidencias del repositorio, autoevalúa su nivel de confianza sobre casos límite, seguridad y concurrencia, y se detiene a preguntar al desarrollador si su certeza no es total. Posteriormente, en la fase `Simplify` (`docs/REVIEW.md`), ejecuta 3 pases de auditoría verificando expresamente que no existan paquetes alucinados (*slopsquatting*). BMAD ofrece también revisión de código por capas y con lentes adversariales; lo específico de `dbv-specs-ops` es la autoevaluación de confianza previa a construir y la comprobación de dependencias inexistentes.

### 7. Gestión integral de Git y cierre (`/ship`)
El framework gobierna Git desde la creación del repositorio hasta el comando de cierre `/ship`, que automatiza el paso de cambios a `CHANGELOG.md`, actualiza `walkthrough.md` y el `README.md`, propone el commit de versión, genera el tag semántico (`vx.y.z`) y sugiere el push, que ejecuta cuando el usuario se lo pide.

### 8. Scripts de arranque y parada multiplataforma (`start` / `stop`)
Si el proyecto es un servidor web, una API o un servicio residente, `/ship` genera en la raíz los pares `start.cmd` / `stop.cmd` (Windows) y `start.sh` / `stop.sh` (macOS/Linux, con `chmod +x`). En proyectos Python activan y desactivan el entorno virtual local automáticamente, y el `README.md` del proyecto documenta cómo usarlos. Las apps de escritorio nativas (Tauri), librerías y CLIs sin proceso residente no reciben scripts `stop` artificiales: documentan sus comandos nativos (`npm run tauri dev`, `cargo tauri build`...).


### 9. Grafo de fases y memoria de descartes (v3.1.0)
Adaptado de [*Procedural Graphs*](https://arxiv.org/abs/2609.09153) (Google, 2026). Cada comando de fase recibe solo su vecindario del grafo: cómo ejecutar la fase según de dónde vengas, sus salidas con sus condiciones y lo que revisará el paso siguiente. Así no carga con las reglas de todas las fases a la vez. Ese contexto se **genera** desde un único `PROCEDURAL_GRAPH.json`, y el linter falla si el grafo tiene callejones sin salida o si un comando se desincroniza de él. La *Rejection Memory* de `memory.md` registra los enfoques probados y descartados para que otra sesión no los repita. Lo que **no** se ha adoptado del paper (el LLM de guía y la auto-evolución automática) y por qué está en [`PROCEDURAL_GRAPHS.md`](./PROCEDURAL_GRAPHS.md).

---

## 🤔 ¿Cuándo elegir cada uno?

* **Elige [`GitHub Spec Kit`](https://github.com/github/spec-kit)** si buscas un flujo SDD guiado por CLI con constitución, 38 integraciones de agentes, un ecosistema de extensiones y presets, y la mayor comunidad.
* **Elige [`BMAD Method`](https://github.com/bmad-code-org/BMAD-METHOD)** si prefieres trabajar con agentes especializados (analista, PM, arquitecto, desarrollador, UX) y skills de planificación, construcción y revisión, con un módulo opcional de arquitectura de tests.
* **Elige [`Kiro`](https://kiro.dev/)** si prefieres un entorno integrado (IDE, CLI, web y móvil) con specs, hooks, steering y tests de propiedades, y aceptas depender de un producto comercial de AWS.
* **Elige `dbv-specs-ops`** si buscas un **marco de ingeniería de principio a fin** con trazabilidad en el código verificada por linter, cabeceras de autor, memoria entre sesiones paralelas, preparación para GEO/Agentes, apps AI-Native o de escritorio en tiendas oficiales, scripts `start`/`stop` y automatización de Git y releases, y aceptas que es un proyecto joven, de un solo autor y con reglas que, por defecto, dependen de que la IA las siga.
