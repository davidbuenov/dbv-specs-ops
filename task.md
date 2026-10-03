# Backlog - dbv-specs-ops v2.9.0 (Desktop Generative AI Architecture, Templates & Reference Implementation)

## Contexto del Proyecto (Context Snapshot)
* **Objetivo**: Integrar y formalizar la arquitectura canónica de IA generativa de escritorio (híbrida local / nube / agentes de suscripción vía ACP) con zero-footprint, custodia en llavero nativo del SO y ciclo atómico de diffs/propuestas, crear plantillas de especificaciones y ayuda de usuario bilingüe en `dbv-specs-ops`, y proporcionar la implementación de referencia completa y probada en `dbv-tauri-starter` (v0.2.0).
* **Estado actual**: ENTREGA COMPLETADA (v2.9.0 documentada y sincronizada exhaustivamente en `dbv-specs-ops`; starter actualizado y testeado en `dbv-tauri-starter`).
* **Última decisión técnica**: Estandarizar el modelo en 3 niveles de conexión con desacoplamiento total en el frontend mediante carga perezosa (`entry.js`), custodia en llavero nativo vía crate `keyring` de Rust, y soporte ACP (Agent Client Protocol) para permitir al usuario final utilizar sus suscripciones activas de Claude Code, ChatGPT CLI o Gemini CLI sin costes por token.
* **Próximo paso**: Commit y tag de la release v2.9.0 en `dbv-specs-ops` y template-v0.2.0 en `dbv-tauri-starter`.

## Checklist de Tareas

- [x] **Fase 1: Especificaciones & Arquitectura Canónica**
  - [x] Diseñar y redactar `docs/AI_DESKTOP_ARCHITECTURE.md` (3 niveles de conectividad, zero-footprint lazy loading, custodia en keyring de Rust, protocolo ACP, ciclo de propuestas atómicas y diffs).
  - [x] Diseñar plantilla de especificaciones `docs/templates/AI_SPECIFICATIONS.template.md` con RNF-IA.1-4 y RF-IA-01-04.
  - [x] Crear plantillas bilingües de documentación de usuario `docs/templates/IA.template.md` y `docs/templates/IA.en.template.md`.

- [x] **Fase 2: Implementación de Referencia en `dbv-tauri-starter` (v0.2.0)**
  - [x] Backend Rust en `src-tauri/src/ai/` (`secrets.rs`, `detect.rs`, `connections.rs`, `providers.rs`, `acp.rs`, `store.rs`, `check.rs`, `commands.rs`).
  - [x] Registrar comandos e inicialización en `src-tauri/src/lib.rs` y dependencias en `Cargo.toml`.
  - [x] Frontend modular en `src/ai/` con carga perezosa, i18n reactivo, wizard de conexión y visor de diffs.
  - [x] Documentación de usuario en `docs/IA.md` y `docs/IA.en.md`.
  - [x] Suite de pruebas automatizadas: 40 tests unitarios y de integración pasando al 100%.
  - [x] Sincronización de versiones en `Cargo.toml`, `package.json`, `tauri.conf.json` y `CHANGELOG.md` a `0.2.0` (`template-v0.2.0`).

- [x] **Fase 3: Sincronización de Metadatos del Framework (`dbv-specs-ops` v2.9.0)**
  - [x] `project.config.md`: `Framework Version: 2.9.0`.
  - [x] `docs/MASTER_PROMPT.md`: Cabecera v2.9.0.
  - [x] `docs/README.md`: Tabla de documentos y diagrama de flujo Mermaid actualizados.
  - [x] `README.md` & `README.en.md`: Badge de versión `2.9.0`, mención de arquitectura de IA y catálogo de plantillas.
  - [x] `docs/UPGRADE_PROMPT.md`: Manifiesto v2.9.0, URLs de descarga en Fase 3 y cierre en Fase 6.
  - [x] `CHANGELOG.md`: Entrada formal `[2.9.0] — 2026-10-03` con notas detalladas y actualización de enlaces de comparación.
  - [x] `memory.md`: Contexto activo y ADR v2.9.0 documentado.
  - [x] `task.md`: Context snapshot y tareas sincronizadas.

---

## 🔄 Context Snapshot / Snapshot de Contexto

> **Last update / Última actualización:** 2026-10-03
> **Exact point / Punto exacto:** Framework `dbv-specs-ops` actualizado íntegramente a v2.9.0. Starter `dbv-tauri-starter` actualizado a v0.2.0 con todos los tests validados.
> **Pending / Pendiente:** Ejecutar commits y tags de versión correspondientes.
> **Next step / Próximo paso:** Verificación final con git status en ambos repositorios.
