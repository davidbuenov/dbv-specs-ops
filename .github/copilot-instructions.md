# Copilot Instructions — dbv-specs-ops

Este proyecto usa **Spec-Driven Development (SDD)**. Aplica estas instrucciones en todas las interacciones.

## Antes de proponer código

1. Consulta `project.config.md` para obtener nombre, autor, licencia y plantilla de cabeceras del proyecto
2. Consulta `docs/SPECIFICATIONS.md` para entender los requisitos actuales
3. Consulta `docs/ARCHITECTURE.md` para respetar el stack y las decisiones técnicas
4. Consulta `docs/DESIGN.md` para respetar el sistema de diseño visual: colores, tipografía y componentes *(si existe)*
5. Consulta `memory.md` para cargar el contexto cualitativo y decisiones técnicas
6. Consulta `task.md` para conocer el estado actual del proyecto

El workflow completo y las normas de desarrollo están en `docs/MASTER_PROMPT.md`.

## Detección automática del estado del proyecto

Al abrir el workspace, ejecuta **dos comprobaciones en orden**:

**1. Bootstrap** — Lee `project.config.md`. Si contiene placeholders (p.ej. `[Project Name]`), no hagas preguntas una a una. Propón un borrador inicial completo con asunciones marcadas y pide confirmación:
  1. *Nombre del proyecto:* [ASSUMPTION: Inferido del directorio, confirma]
  2. *Autor / Empresa:* [ASSUMPTION: Tu nombre, confirma]
  3. *Licencia:* [ASSUMPTION: MIT por defecto, confirma]
  4. *Control de versiones con Git:* [ASSUMPTION: Sí, altamente recomendado, confirma]
  5. *Idioma de la documentación:* [ASSUMPTION: ES por defecto, confirma]
  Tras la confirmación del usuario en un solo paso, rellena `project.config.md`, genera `LICENSE` y crea el `README.md` del proyecto desde `README.template.md`.

**2. Specs** — Comprueba si `docs/SPECIFICATIONS.md` tiene contenido real (no solo placeholders):
- **Si está vacío o solo tiene placeholders** → el proyecto aún no tiene specs. Informa al usuario y sigue el flujo definido en `docs/ADOPTION_PROMPT.md` para reconstruir el contexto.
- **Si está relleno** → el proyecto ya usa SDD. Consulta `task.md` para retomar desde el Snapshot de Contexto.

## Ciclo de desarrollo obligatorio

Sigue este orden. No saltes fases.

1. `/spec` — Valida que el requisito está definido en `SPECIFICATIONS.md`. Si no, pregunta.
2. `/plan` — Desglosa el trabajo en pasos atómicos en `task.md` (máx. 50 líneas por paso). Para planes complejos, usa `implementation_plan.md`.
3. `/build` — Implementa de forma incremental. "One slice at a time."
4. `/test` — Crea y ejecuta tests. Sin test, la tarea no está hecha.
5. `/code-simplify` — Refactoriza para claridad. "Clarity over cleverness."
6. `/ship` — Actualiza `README.md`, completa `walkthrough.md` y marca la tarea como completada en `task.md`.

## Normas

- No inventes decisiones técnicas: si no está en `ARCHITECTURE.md`, pregunta antes de asumir
- Registra la deuda técnica en `task.md`, no en comentarios de código
- Actualiza `task.md` tras cada hito con un nuevo Snapshot de Contexto
- Nunca dejes secretos, claves API o datos sensibles en el código
- Antes de escribir código en un lenguaje, consulta el repo de estándares indicado en `MASTER_PROMPT.md`

---

> 🛠️ Framework SDD creado por **[David Bueno Vallejo](https://github.com/davidbuenov)** — libre y gratuito · [dbv-specs-ops](https://github.com/davidbuenov/dbv-specs-ops)
