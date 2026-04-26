# Instrucciones del Proyecto para Claude Code

Este proyecto sigue la metodología **Spec-Driven Development (SDD)**. Lee estos archivos al inicio de cada sesión antes de proponer cualquier código o plan:

| Archivo | Propósito |
| --- | --- |
| `docs/MASTER_PROMPT.md` | Workflow obligatorio, normas y límites |
| `docs/SPECIFICATIONS.md` | Requisitos del proyecto actual |
| `docs/ARCHITECTURE.md` | Stack y decisiones técnicas |
| `task.md` | Estado actual + Snapshot de Contexto |

## Detección automática del estado del proyecto

Al iniciar sesión, comprueba si `docs/SPECIFICATIONS.md` tiene contenido real (no solo placeholders):

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
3. Actualiza el Snapshot en `task.md`
4. Actualiza `README.md` con el nombre y estado del nuevo proyecto

---

> 🛠️ Framework SDD creado por **[David Bueno Vallejo](https://github.com/davidbuenov)** — libre y gratuito · [dbv-specs-ops](https://github.com/davidbuenov/dbv-specs-ops)
