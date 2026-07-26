# Backlog - dbv-specs-ops v2.2.0

## Contexto del Proyecto (Context Snapshot)
* **Objetivo**: Incorporar soporte de empaquetado para proyectos de Python y Node.js, simplificar el framework unificando la documentación bajo el método exclusivo de subcarpeta e implementar reglas estrictas de diferenciación de contexto.
* **Estado actual**: COMPLETADO (v2.2.0 lista para entrega y commit).
* **Última decisión técnica**: Publicar la versión 2.2.0 en CHANGELOG.md y actualizar walkthroughs.
* **Próximo paso**: Proponer comandos de Git commit y Git tag v2.2.0 al usuario para cerrar la tarea.

## Checklist de Tareas

- [x] **Fase 1: Especificaciones (`/spec`)**
  - [x] Validar que las características de Pip y Npm no tengan asunciones sin confirmar.
  - [x] Identificar la simplificación a subcarpeta isolation como el único método de adopción soportado.

- [x] **Fase 2: Planificación (`/plan`)**
  - [x] Crear `implementation_plan.md` y actualizar `task.md` con las tareas activas.
  - [x] Obtener aprobación final del usuario sobre los cambios propuestos.

- [x] **Fase 3: Construcción (`/build`)**
  - [x] Incrementar la versión del framework a `2.2.0` en `project.config.md`.
  - [x] Modificar `docs/MASTER_PROMPT.md` para:
    - [x] Exigir la creación de `pyproject.toml`/`setup.py` (PyPI-ready) en Python y `package.json` (npm-ready) en Node.
    - [x] Añadir directiva sobre archivos de activación (`CLAUDE.md`, `.github/copilot-instructions.md`, etc.) en la raíz cuando se usa aislamiento en subcarpeta.
    - [x] Añadir directiva sobre diferenciación de contexto para evitar que la IA documente el framework en lugar de la aplicación.
  - [x] Modificar `docs/ADOPTION_PROMPT.md` para:
    - [x] Añadir directiva de diferenciación estricta de contexto en la Fase C.
  - [x] Refactorizar `README.template.md` para incluir:
    - [x] Secciones de instalación de Python (`pip` local + `git+https` remoto).
    - [x] Secciones de instalación de Node (`npm` local + `git+https` remoto).
    - [x] Instrucciones de publicación oficiales (PyPI con `twine` y npm registry con `npm publish`).
  - [x] Actualizar `README.md` (English & Español):
    - [x] Describir nuevas características de instalación y publicación.
    - [x] Añadir instrucciones detalladas de copiar los archivos de activación a la raíz en la Opción B (Aislamiento en Subcarpeta) y dar ejemplos.
  - [x] Actualizar el manifiesto de actualización en `docs/UPGRADE_PROMPT.md` para reflejar la versión `2.2.0`.
  - [x] Registrar los cambios en `CHANGELOG.md` bajo `[Sin publicar]`.

- [x] **Fase 4: Pruebas y Verificación (`/test`)**
  - [x] Validar la sintaxis de todos los archivos y plantillas modificados.

- [x] **Fase 5: Simplificar (`/code-simplify`)**
  - [x] Realizar una auditoría de seguridad y simplificación del código/texto.

- [x] **Fase 6: Entrega (`/ship`)**
  - [x] Completar `walkthrough.md`.
  - [x] Publicar la versión en `CHANGELOG.md` con fecha de hoy.
  - [x] Proponer commit y tags de Git.

---

## 🔄 Context Snapshot / Snapshot de Contexto

> **Last update / Última actualización:** 2026-07-26
> **Exact point / Punto exacto:** Planificación terminada y registrada. Esperando aprobación del usuario.
> **Pending / Pendiente:** Obtener aprobación del plan de implementación por parte del usuario.
> **Next step / Próximo paso:** Una vez aprobado, iniciar la Fase 3 (`/build`).