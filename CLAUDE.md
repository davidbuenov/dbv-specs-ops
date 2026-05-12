# Instrucciones del Proyecto para Claude Code

Este proyecto sigue la metodología **Spec-Driven Development (SDD)**. Lee estos archivos al inicio de cada sesión antes de proponer cualquier código o plan:

| Archivo | Propósito |
| --- | --- |
| `project.config.md` | Identidad del proyecto: nombre, autor, licencia y plantilla de cabeceras |
| `docs/MASTER_PROMPT.md` | Workflow obligatorio, normas y límites |
| `docs/SPECIFICATIONS.md` | Requisitos del proyecto actual |
| `docs/ARCHITECTURE.md` | Stack y decisiones técnicas |
| `docs/DESIGN.md` | Sistema de diseño visual: tokens de color, tipografía, componentes y filosofía *(si existe)* |
| `memory.md` | **Contexto y Decisiones:** Conocimiento cualitativo (ADRs, lecciones, mapa) |
| `task.md` | Estado actual + Snapshot de Contexto |

## Detección automática del estado del proyecto

Al iniciar sesión, ejecuta **dos comprobaciones en orden**:

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

## Comportamiento esperado

- Sigue siempre el ciclo: **Spec → Plan → Build → Test → Simplify → Ship**
- Antes de codificar, confirma que el "qué" está definido en `docs/SPECIFICATIONS.md`
- Cuando elabores un plan complejo, créalo en `implementation_plan.md` y solicita aprobación antes de ejecutar
- Tras cada hito, actualiza el Snapshot de Contexto en `task.md`; al completar, rellena `walkthrough.md`
- Si falta información crítica, pregunta — no asumas
- Consulta el repo de estándares indicado en `docs/MASTER_PROMPT.md` antes de escribir código en un lenguaje nuevo

## Para adaptar esta plantilla a un nuevo proyecto

1. Rellena `docs/SPECIFICATIONS.md` con el contexto del nuevo proyecto
2. Rellena `docs/ARCHITECTURE.md` con el stack tecnológico elegido
3. Si el proyecto tiene interfaz de usuario, rellena `docs/DESIGN.md` con el sistema de diseño
4. Actualiza el Snapshot en `task.md`
5. Actualiza `README.md` con el nombre y estado del nuevo proyecto

---

> 🛠️ Framework SDD creado por **[David Bueno Vallejo](https://github.com/davidbuenov)** — libre y gratuito · [dbv-specs-ops](https://github.com/davidbuenov/dbv-specs-ops)
