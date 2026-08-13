# Backlog - dbv-specs-ops v2.5.0 (Native Desktop Apps, CI & Marketplace Integration)

## Contexto del Proyecto (Context Snapshot)
* **Objetivo**: Incorporar soporte nativo para aplicaciones de escritorio compiladas (Rust + Tauri v2), CI/CD multiplataforma en GitHub Actions y guías de publicación en marketplaces de apps (Microsoft Store, Uptodown, etc.).
* **Estado actual**: ENTREGA COMPLETADA (v2.5.0 lista para commit y tags).
* **Última decisión técnica**: Crear 3 nuevas guías operativas (`NATIVE_DESKTOP_APPS.md`, `NATIVE_APPS_RELEASE_CI.md`, `MARKETPLACE_PUBLISHING.md`) e integrar sus Phase Gates y opciones de stack en `MASTER_PROMPT.md`, `README.md`, `UPGRADE_PROMPT.md`, `project.config.md`, `CHANGELOG.md` y `memory.md`.
* **Próximo paso**: Proponer los comandos de Git commit y Git tag v2.5.0 al desarrollador.

## Checklist de Tareas

- [x] **Fase 1: Especificaciones (`/spec`)**
  - [x] Crear y obtener aprobación de `implementation_plan.md` (v2.5.0).
  - [x] Validar que el alcance incluye los 3 nuevos documentos y las actualizaciones en `MASTER_PROMPT.md`, `README.md`, `UPGRADE_PROMPT.md`, `project.config.md`, `CHANGELOG.md` y `memory.md`.

- [x] **Fase 3: Construcción (`/build`)**
  - [x] **1. Nuevas Guías Operativas**:
    - [x] Crear `docs/NATIVE_DESKTOP_APPS.md`.
    - [x] Crear `docs/NATIVE_APPS_RELEASE_CI.md`.
    - [x] Crear `docs/MARKETPLACE_PUBLISHING.md`.
  - [x] **2. Instrucción Maestra**: Modificar `docs/MASTER_PROMPT.md` (opción de stack en Bootstrap §7, Phase Gate en `/plan` y Phase Gate en `/ship`).
  - [x] **3. Índices de Documentación**: Modificar `docs/README.md` y `README.md`.
  - [x] **4. Asistente de Migración**: Modificar `docs/UPGRADE_PROMPT.md` para v2.5.0.
  - [x] **5. Configuración y Registro**:
    - [x] Modificar `project.config.md` para establecer la versión en `2.5.0`.
    - [x] Modificar `CHANGELOG.md` para registrar la versión `2.5.0`.
    - [x] Modificar `memory.md` para registrar el ADR de v2.5.0.

- [x] **Fase 4: Pruebas y Verificación (`/test`)**
  - [x] Validar sintaxis Markdown y consistencia de enlaces relativos en todos los documentos.

- [x] **Fase 5: Simplificar (`/code-simplify`)**
  - [x] Auditoría de estilo, enlaces y consistencia de textos.

- [x] **Fase 6: Entrega (`/ship`)**
  - [x] Crear `walkthrough.md` de los cambios y actualizar el Context Snapshot.

---

## 🔄 Context Snapshot / Snapshot de Contexto

> **Last update / Última actualización:** 2026-08-13
> **Exact point / Punto exacto:** Todo finalizado. v2.5.0 lista para commit en Git.
> **Pending / Pendiente:** Ninguna.
> **Next step / Próximo paso:** Ejecutar commit y tag en Git.