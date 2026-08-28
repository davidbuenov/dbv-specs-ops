# Backlog - dbv-specs-ops v2.7.0 (Desktop DoD, Tauri v2 Hardening, Web-to-Desktop & Production Lessons)

## Contexto del Proyecto (Context Snapshot)
* **Objetivo**: Consolidar y cerrar la versión 2.7.0 del framework con la cosecha de lecciones de producción (DoD de escritorio nativo, 4 nuevas trampas de Tauri v2, gestión segura de firmas, orquestación CI `spawnSync`, mitigación de inputs en Actions), estrategia de migración web → escritorio (v2.6.0) y sincronización documental completa sin nombres de proyectos específicos.
* **Estado actual**: ENTREGA COMPLETADA (v2.7.0 revisada, sincronizada y lista para commit y tags).
* **Última decisión técnica**: Generalizar todo el contenido para que reporte hallazgos de pruebas y lecciones reales de producción de forma neutral y transferible, sincronizando el número de versión 2.7.0 en `README.md`, `project.config.md`, `UPGRADE_PROMPT.md`, `MASTER_PROMPT.md`, `CHANGELOG.md` y `memory.md`.
* **Próximo paso**: Realizar commit de v2.7.0 y solicitar confirmación para push/tag.

## Checklist de Tareas

- [x] **Fase 1: Especificaciones (`/spec`)**
  - [x] Revisar diffs y contenido técnico de las guías de escritorio y migración.
  - [x] Validar que todo el contenido es 100% genérico y sin rutas o identificadores acoplados a proyectos concretos.

- [x] **Fase 3: Construcción (`/build`)**
  - [x] **1. Actualización de Guías Operativas**:
    - [x] `docs/NATIVE_DESKTOP_APPS.md`: DoD de Experiencia de Escritorio (§7), 4 nuevas trampas de Tauri v2 (§6), updater key por el usuario (§4.4) y patrón de menú nativo en macOS.
    - [x] `docs/WEB_TO_DESKTOP_MIGRATION.md`: Ruta con bundler (§9), gotchas de `frontendDist` recursivo y `runningInTauri`.
    - [x] `docs/MARKETPLACE_PUBLISHING.md`: Trackeo completo de `gen/windows/` (§3).
    - [x] `docs/NATIVE_APPS_RELEASE_CI.md`: `spawnSync` en local (§6) y verificación de inputs de Actions (§6bis).
  - [x] **2. Asistente de Migración y Metadatos**:
    - [x] Modificar `docs/UPGRADE_PROMPT.md` para v2.7.0 (manifest, fases y cierre).
    - [x] Modificar `docs/MASTER_PROMPT.md` para reflejar v2.7.0 en la cabecera.
    - [x] Modificar `project.config.md` para establecer la versión en `2.7.0` y limpiar subtítulo de Model Routing Guidelines.
    - [x] Modificar `README.md` (badges y versión de estado a `2.7.0`).
    - [x] Modificar `CHANGELOG.md` para registrar la versión `2.7.0` y enlaces de comparación.
    - [x] Modificar `memory.md` para registrar los ADRs de v2.6.0 y v2.7.0 con lenguaje genérico.

- [x] **Fase 4: Pruebas y Verificación (`/test`)**
  - [x] Validar consistencia de versiones en todos los documentos del repositorio.
  - [x] Verificar por grep que no queden referencias acopladas o nombres específicos.

- [x] **Fase 5: Simplificar (`/code-simplify`)**
  - [x] Auditoría de estilo y coherencia con la convención SemVer (minor 2.7.0).

- [x] **Fase 6: Entrega (`/ship`)**
  - [x] Actualizar `memory.md`, `task.md` y `walkthrough.md`.
  - [x] Preparar Git commit y tags de la versión v2.7.0.

---

## 🔄 Context Snapshot / Snapshot de Contexto

> **Last update / Última actualización:** 2026-08-28
> **Exact point / Punto exacto:** Cambios revisados, generalizados y consistentes. v2.7.0 lista para commit.
> **Pending / Pendiente:** Confirmación del usuario para `git commit` y `git push --tags`.
> **Next step / Próximo paso:** Realizar commit local y consultar al usuario.