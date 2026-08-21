# Backlog - dbv-specs-ops v2.5.1 (Tauri v2 Gotchas, NSIS Customization & CI Release Templates)

## Contexto del Proyecto (Context Snapshot)
* **Objetivo**: Incorporar gotchas reales de Tauri v2 (permisos, WebView2, WKWebView, threading), personalización de NSIS e identidad MSIX, y plantillas completas de GitHub Actions para release de desktop apps.
* **Estado actual**: ENTREGA COMPLETADA (v2.5.1 lista para commit y tags).
* **Última decisión técnica**: Endurecer `docs/NATIVE_DESKTOP_APPS.md`, `docs/MARKETPLACE_PUBLISHING.md` y `docs/NATIVE_APPS_RELEASE_CI.md` con contenido 100% genérico extraído de lecciones reales, e incrementar versión a v2.5.1 en `project.config.md`, `UPGRADE_PROMPT.md`, `CHANGELOG.md` y `memory.md`.
* **Próximo paso**: Realizar commit de v2.5.1 y solicitar confirmación para push/tag.

## Checklist de Tareas

- [x] **Fase 1: Especificaciones (`/spec`)**
  - [x] Revisar diffs generados en la sesión previa.
  - [x] Validar que todo el contenido es 100% genérico y sin rutas o identificadores acoplados.

- [x] **Fase 3: Construcción (`/build`)**
  - [x] **1. Actualización de Guías Operativas**:
    - [x] `docs/NATIVE_DESKTOP_APPS.md`: 9 gotchas de Tauri v2 y matiz `.deb` vs `.AppImage`.
    - [x] `docs/MARKETPLACE_PUBLISHING.md`: personalización NSIS e identidad MSIX.
    - [x] `docs/NATIVE_APPS_RELEASE_CI.md`: plantillas YAML completas (`release-windows.yml`, `release-linux.yml`, `release-macos.yml`).
  - [x] **2. Asistente de Migración y Versiones**:
    - [x] Modificar `docs/UPGRADE_PROMPT.md` para v2.5.1.
    - [x] Modificar `project.config.md` para establecer la versión en `2.5.1`.
    - [x] Modificar `CHANGELOG.md` para registrar la versión `2.5.1`.
    - [x] Modificar `memory.md` para registrar el ADR de v2.5.1.

- [x] **Fase 4: Pruebas y Verificación (`/test`)**
  - [x] Validar consistencia de versiones en todos los documentos.
  - [x] Verificar que no existan nombres o rutas específicas filtradas.

- [x] **Fase 5: Simplificar (`/code-simplify`)**
  - [x] Auditoría de estilo y coherencia con la convención SemVer (patch 2.5.1).

- [x] **Fase 6: Entrega (`/ship`)**
  - [x] Actualizar `memory.md` y `task.md`.
  - [x] Ejecutar Git commit de la versión v2.5.1.

---

## 🔄 Context Snapshot / Snapshot de Contexto

> **Last update / Última actualización:** 2026-08-21
> **Exact point / Punto exacto:** Cambios revisados y consistentes. v2.5.1 lista para commit.
> **Pending / Pendiente:** Confirmación del usuario antes de `git push`.
> **Next step / Próximo paso:** Realizar commit local y consultar al usuario.