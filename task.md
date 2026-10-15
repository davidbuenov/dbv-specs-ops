# Backlog - dbv-specs-ops v3.0.0 (Spec-to-Test Traceability, Deterministic Governance & Path Coherence)

## Contexto del Proyecto (Context Snapshot)
* **Objetivo**: Que la v3.0.0 sea claramente mejor que la v2.9.0 sin inconsistencias y sin romper proyectos existentes: trazabilidad de requisitos (`REQ-NN` ↔ `@spec`), linter determinista, Architect Review con evidencia y confianza, compresión de contexto, contratos de subagente, activador `AGENTS.md` y coherencia de rutas entre el repo del framework y los proyectos consumidores.
* **Estado actual**: IMPLEMENTADA Y VERIFICADA DE EXTREMO A EXTREMO (scripts + IA real + Linux) en el árbol de trabajo. Pendiente de commit y tag por el usuario.
* **Última decisión técnica**: Los IDs de requisito son obligatorios solo para requisitos nuevos o modificados; un proyecto anterior a v3.0.0 se actualiza sin tocar su spec, su backlog ni su código (el linter omite la trazabilidad si no hay IDs y ofrece `--changed`/`--staged`). `AGENTS.md` es un puntero fino más y nunca se sobrescribe uno propio del proyecto.
* **Próximo paso**: Revisar `git diff --stat`, commit `Version v.3.0.0` y tag `v3.0.0`; revisar a mano la carpeta `old/` (ignorada por git) y borrarla.

## Checklist de Tareas

- [x] **Fase 1: Linter determinista (`scripts/dbv-lint.py`)** — reescrito desde el borrador, cumple su propia regla, Python 3.8+, UTF-8 seguro en Windows, raíz = `.git` más cercano, respeta `.gitignore` (git) o poda venvs por `pyvenv.cfg` (sin git), `--changed`/`--staged`/`--strict`/`--root`, trazabilidad omitida si la spec no usa IDs.
- [x] **Fase 2: `docs/MASTER_PROMPT.md`** — IDs/`@spec`, `<evidence>` + `<confidence_score>`, Hot/Warm/Cold, bootstrap (#8, excepción del repo fuente, restablecimiento confirmado con plantillas, no sobrescribir activadores propios), `uv`, Rust/Tauri/IIFE, start/stop condicionales, `dbv-lint.py` en `/code-simplify`.
- [x] **Fase 3: Resto del framework** — `SPECIFICATIONS.md`, `ARCHITECTURE.md`, `REVIEW.md`, `GUARDRAILS.md`, `PARALLEL_WORK.md`, `ADOPTION_PROMPT.md`, `docs/README.md`, plantillas `task.template.md` y `memory.template.md`.
- [x] **Fase 4: Activadores y comandos** — nota de rutas en los 5 activadores existentes; **nuevo `AGENTS.md`** (estándar abierto, comprobado con la documentación oficial y con Claude Code 2.1.283); `.claude/commands/*.md` con prefijo y referencias nuevas.
- [x] **Fase 5: `project.config.md` y `README.template.md`**.
- [x] **Fase 6: `README.md` / `README.en.md`** — badge/estado 3.0.0, 25 enlaces 404 corregidos por fichero, `AGENTS.md` en la tabla de plataformas y en el Paso 2, características nuevas, actualización con ruta de subcarpeta, créditos.
- [x] **Fase 7: `scripts/run-evals.sh` y `evals/`** — rutas dinámicas, lista blanca de CLI, `INCONCLUSO` si el patrón ya existía, eval de ejemplo no vacuo, permisos de escritura documentados.
- [x] **Fase 8: `docs/UPGRADE_PROMPT.md`** — manifiesto v3.0.0 (verificado contra `git diff v2.9.0`), 42 URLs con fichero local, versión definida solo en la Fase 1, `memory.md` protegido, activadores en la raíz conservando contenido del usuario.
- [x] **Fase 9: Higiene y cierre** — `old/` (fuera de git), `CHANGELOG.md` `[3.0.0] — 2026-10-14`, `memory.md`, `task.md`, `walkthrough.md`.
- [x] **Fase 10: Verificación de extremo a extremo** — ver `walkthrough.md`.
- [x] **Fase 11: Ajustes tras publicar** — `UPGRADE_PROMPT` (fila de evals y copias inertes de activadores, verificado con descargas reales) y README con «Empezar en un minuto». Publicado como 3.0.0 (el tag `v3.0.0` se movió al commit final; nadie había descargado la versión).
- [ ] **Pendiente (usuario):** `git diff --stat`, commit `Version v.3.0.0`, tag `v3.0.0` y push.

## Deuda Técnica
- El tag `v2.8.0` no existe en git (hay `v2.7.0` y `v2.9.0`), aunque el `CHANGELOG.md` enlaza `compare/v2.7.0...v2.8.0`.
- El snippet de `.windsurfrules` del Paso 2 de los README está en formato JSON, mientras el `.windsurfrules` del repo es Markdown plano; no se ha verificado cuál es el formato correcto de Windsurf.
- La prueba de actualización se hizo con un espejo local de `master` (la v3.0.0 aún no está publicada): al publicar conviene repetir la actualización real contra las URL de GitHub en un proyecto de prueba.

---

## 🔄 Context Snapshot / Snapshot de Contexto

> **Last update / Última actualización:** 2026-10-14
> **Exact point / Punto exacto:** Release v3.0.0 completa y verificada de extremo a extremo (bootstrap, ciclo plan→build→test, evals con `jq` real y CLI real, actualización de un proyecto v2.9.0, Linux/WSL). Sin commit ni tag.
> **Pending / Pendiente:** Commit `Version v.3.0.0`, tag `v3.0.0` y push (acciones del usuario); revisar y borrar `old/`; tras publicar, repetir la actualización contra GitHub.
> **Next step / Próximo paso:** `git status` y `git diff --stat` para revisar el conjunto antes del commit.
