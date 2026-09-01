# Backlog - dbv-specs-ops v2.8.0 (AI-Native SDLC Loop Closure & Guardrails)

## Contexto del Proyecto (Context Snapshot)
* **Objetivo**: Integrar de forma curada el [AI-Native SDLC Playbook (Anthropic)](https://claude.com/blog/the-ai-native-sdlc-playbook) — cierre autónomo del loop (Fase 7 Maintain, opcional), revisión de código por pases con severidad, guardarraíles deterministas y trabajo paralelo formalizado — más un fix de usabilidad real (comandos de fase escuetos mal interpretados) y la división del README en dos ficheros de un solo idioma.
* **Estado actual**: ENTREGA COMPLETADA (v2.8.0 commiteada y pusheada a `origin/master`, commit `7c33e42`).
* **Última decisión técnica**: El pase "Cumplimiento" de `docs/REVIEW.md` audita ahora explícitamente los `<coding_standards>` de `MASTER_PROMPT.md` (no solo specs/arquitectura), porque se detectó código real con 5-6 `return` por función pese a que la regla ya estaba declarada "obligatoria" — la sola declaración advisory no bastaba.
* **Próximo paso**: Ninguno pendiente en el ciclo de esta versión. `task.md`, `memory.md` y `walkthrough.md` sincronizados en un commit de seguimiento tras el cierre.

## Checklist de Tareas

- [x] **Fase 1: Especificaciones (`/spec`, implícita)**
  - [x] Analizar el playbook de Anthropic y los 6 documentos generados en una sesión previa (`docs/novedades2.8/`, ya integrados y eliminados).
  - [x] Decidir alcance de integración con el usuario (curada completa, vs. núcleo, vs. todo-menos-Maintain) → elegido: curada completa.

- [x] **Fase 3: Construcción (`/build`)**
  - [x] **1. Documentos nuevos en `docs/`**: `MAINTAIN.md` (Fase 7 opcional), `REVIEW.md`, `GUARDRAILS.md`, `PARALLEL_WORK.md`, `SOURCE_OF_TRUTH.md`, `METRICS.md`.
  - [x] **2. Infraestructura opcional**: `evals/README.md`, `evals/example-spec-eval.json`, `scripts/run-evals.sh` (regresión de la configuración del agente, no del código de proyecto).
  - [x] **3. Fix de comandos de fase escuetos**: regla de cascada en `docs/MASTER_PROMPT.md` (`<workflow>`) + `.claude/commands/{spec,plan,build,test,code-simplify,ship,maintain}.md` (comandos nativos de Claude Code con autocompletado).
  - [x] **4. Enganches en `MASTER_PROMPT.md`**: `/plan` → `PARALLEL_WORK.md`; `/code-simplify` → los 3 pases de `REVIEW.md`; `/ship` → gate de hallazgos Crítico; `<boundaries>` → `GUARDRAILS.md`; `<context_management>` → `SOURCE_OF_TRUTH.md`; Fase 7 documentada al final de `<workflow>`.
  - [x] **5. Metadatos**: `project.config.md` (versión → `2.8.0`), `docs/UPGRADE_PROMPT.md` (manifest v2.8.0 completo, nuevas URLs de descarga, mensaje de cierre), `README.md`/`README.en.md`, `docs/README.md`.

- [x] **Fase 4: Pruebas y Verificación (`/test`)**
  - [x] `bash -n scripts/run-evals.sh` (sintaxis) y validación del JSON de ejemplo.
  - [x] Etiquetas XML de `MASTER_PROMPT.md` balanceadas tras las inserciones.
  - [x] Paridad manifest ↔ disco entre `docs/UPGRADE_PROMPT.md` y los ficheros nuevos.

- [x] **Fase 5: Simplificar (`/code-simplify`)**
  - [x] Extender el pase "Cumplimiento" de `docs/REVIEW.md` para auditar `<coding_standards>` (un solo `return` + guard clauses, patrón Result, tipado estricto) — gap detectado por el usuario tras ver código real con múltiples `return` por función.
  - [x] Añadir a `docs/GUARDRAILS.md` un ejemplo concreto de heurística pre-commit para esa regla.

- [x] **Fase 6: Entrega (`/ship`, manual — sin usar los comandos de fase sobre este propio repo)**
  - [x] Dividir `README.md` bilingüe en `README.md` (español, principal) y `README.en.md` (inglés), con enlace cruzado de idioma y diagramas `mermaid` actualizados (nodo opcional Maintain, pase de revisión en Simplify).
  - [x] Corregir el ancla rota `#adoption` en el índice del README (bug preexistente, no introducido en esta versión).
  - [x] Borrar `docs/novedades2.8/` (contenido ya integrado) y `docs/thenewsdlcwithvibecoding.pdf` (limpieza de espacio, sin referencias cruzadas).
  - [x] Actualizar el pie de comparación de `CHANGELOG.md` (`[Sin publicar]` → `v2.8.0...HEAD`, nuevo `[2.8.0]: v2.7.0...v2.8.0`).
  - [x] Commit `Version 2.8.0` (`7c33e42`) y push a `origin/master`.

---

## 🔄 Context Snapshot / Snapshot de Contexto

> **Last update / Última actualización:** 2026-09-01
> **Exact point / Punto exacto:** v2.8.0 publicada en `origin/master` (commit `7c33e42`). `task.md`, `memory.md` y `walkthrough.md` sincronizados en el commit de seguimiento inmediato.
> **Pending / Pendiente:** Ninguno para esta versión.
> **Next step / Próximo paso:** Retomar el backlog normal del framework en la próxima sesión.
