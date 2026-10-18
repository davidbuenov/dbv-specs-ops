# Backlog - dbv-specs-ops v3.1.0 (Phase Graph & Rejection Memory)

## Contexto del Proyecto (Context Snapshot)
* **Objetivo**: Tomar de *Procedural Graphs* (arXiv:2609.09153) solo lo que mejora el SDD sin la infraestructura del paper, verificarlo de forma determinista y con la IA real, y documentarlo con honestidad (comparativas, referencias y enlace al paper).
* **Estado actual**: IMPLEMENTADA Y VERIFICADA en el árbol de trabajo (scripts en Windows/Linux/Python 3.8 y tres pruebas con `claude -p`: Rejection Memory en `/plan`, creación de la sección en un proyecto antiguo y actualización v3.0.0 → v3.1.0). Pendiente de commit y tag por el usuario.
* **Última decisión técnica**: El grafo es la fuente única; los bloques de `.claude/commands/` se generan y el linter vigila que no diverjan. `MASTER_PROMPT.md` solo lleva un puntero. No se adopta la auto-evolución automática (REJECTED-05 en `memory.md`).
* **Próximo paso**: Revisar `git diff --stat`, commit `Version v.3.1.0` y tag `v3.1.0`.

## Checklist de Tareas

- [x] **Fase 1: Diagnóstico del borrador** — contrastado con el PDF del paper (cifras correctas, pero aplicadas a configuraciones que el paper no midió) y su código ejecutado en el scratchpad (fallos en Windows, regla de un `return`, Python 3.10+, validador permisivo, regresiones en `run-evals.sh`).
- [x] **Fase 2: Grafo y motor** — `docs/PROCEDURAL_GRAPH.json` (10 nodos, 16 transiciones con vueltas atrás) y `scripts/pg-guide.py` (validación, contexto por fase, generación y comprobación de comandos).
- [x] **Fase 3: Comandos y linter** — bloques generados en los 7 comandos; `dbv-lint.py` valida el grafo y la sincronía (error en el framework, aviso en la raíz del consumidor).
- [x] **Fase 4: Rejection Memory** — `MASTER_PROMPT.md` (`<procedural_graph>` + disparadores), `memory.md` (5 entradas reales) y `memory.template.md`.
- [x] **Fase 5: Maintain y evals** — regla de la cola del log, resumen `PASA/TOTAL` en `run-evals.sh` (sin perder protecciones) y regla de no regresión.
- [x] **Fase 6: Documentación** — `PROCEDURAL_GRAPHS.md`, `AGENTIC_ENGINEERING.md` §6, `docs/README.md`, `GUARDRAILS.md`, `UPGRADE_PROMPT.md` (manifiesto v3.1.0, URLs, `chmod`).
- [x] **Fase 7: README y comparativas** — versión 3.1.0, características, pilar 8 y referencias con enlace al paper, dos filas contrastadas con Spec Kit/BMAD/Kiro (16/10/2026), §9 y limitación nueva en `COMPARISON*.md`.
- [x] **Fase 8: Verificación** — ver `walkthrough.md`.
- [x] **Fase 9: Cierre** — `CHANGELOG.md` `[3.1.0] — 2026-10-16` (incluye la comparativa que estaba en `[Sin publicar]`), `memory.md`, `task.md`, `walkthrough.md`, borrador archivado en `old/`.
- [ ] **Pendiente (usuario):** `git diff --stat`, commit `Version v.3.1.0`, tag `v3.1.0` y push.

## Deuda Técnica
- `evals/` tiene un solo eval: la regla de no regresión y cualquier medición del efecto del grafo necesitan una suite real (15-30 tareas, una por fase como mínimo). Es también la condición para reabrir REJECTED-05.
- `MASTER_PROMPT.md` ha crecido de 35,2 a 38,5 KB con el bloque `<procedural_graph>`; vigilar el tamaño en próximas versiones.
- La actualización v3.0.0 → v3.1.0 se probó con un espejo local de `master`: al publicar, repetirla contra las URL de GitHub.
- El tag `v2.8.0` no existe en git (hay `v2.7.0` y `v2.9.0`), aunque el `CHANGELOG.md` enlaza `compare/v2.7.0...v2.8.0`.
- El snippet de `.windsurfrules` del Paso 2 de los README está en formato JSON, mientras el `.windsurfrules` del repo es Markdown plano; no se ha verificado cuál es el formato correcto de Windsurf.

---

## 🔄 Context Snapshot / Snapshot de Contexto

> **Last update / Última actualización:** 2026-10-16
> **Exact point / Punto exacto:** Release v3.1.0 completa y verificada (deterministas + IA real). Sin commit ni tag.
> **Pending / Pendiente:** Commit `Version v.3.1.0`, tag `v3.1.0` y push (acciones del usuario); tras publicar, repetir la actualización contra GitHub.
> **Next step / Próximo paso:** `git status` y `git diff --stat` para revisar el conjunto antes del commit.
