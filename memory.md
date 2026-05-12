# 🧠 Memory & Context

> **Frontera de uso (Memory vs. Tasks):**
> - `task.md` → progreso **operativo**: checklist de tareas, Snapshot de Contexto (el paso exacto siguiente), y estado de la sesión.
> - `memory.md` → contexto **cualitativo y temático**: conocimiento persistente, decisiones técnicas profundas, lecciones, y el área del producto en foco (no el paso específico).
> Si hay info que sirva para los dos, prioriza: datos con fecha/paso exacto → `task.md`; razonamiento/por-qué/lecciones → `memory.md`.
>
> *Instrucción para la IA: Consulta este archivo al inicio de cada sesión para recuperar el hilo técnico. Actualiza las secciones correspondientes cuando el workflow lo indique (triggers en `/plan`, `/build`, `/test` y gate en `/ship`).*

## 🎯 Contexto Activo
- **Estado actual del desarrollo:** [Describe brevemente en qué fase está el proyecto, ej: "Implementando el sistema de base de datos local y refactorizando el UI de configuración"]
- **Foco inmediato:** [El problema principal que se está intentando resolver hoy]

## 🏗️ Log de Decisiones Técnicas (ADR Ligero)
*Registro de por qué se tomaron ciertas rutas (ej. cambios en librerías, arquitectura o patrones).*

- **[Fecha] - [Decisión]:** [Razón/Contexto]
- *Ejemplo: 2026-05-12 - Se cambió SQLite por IndexedDB: Necesitábamos soporte offline. (Borra esta línea de ejemplo al crear la primera entrada real).*

## ⚠️ Lecciones Aprendidas / Errores Evitados
*Notas sobre bugs específicos, configuraciones que fallaron o refactors intentados para no repetirlos.*

- **[Fallo/Bug]:** [Solución/Evitación]
- *Ejemplo: El watcher de Vite entra en bucle si `/dist` no está en exclude. (Borra esta línea de ejemplo al crear la primera entrada real).*

## 🗺️ Mapa de Relaciones
*Breve descripción de cómo interactúan los módulos actuales para ayudar a la IA a navegar el código.*

- **[Módulo/Componente]:** [Responsabilidad y Dependencias]
- *Ejemplo: `auth_service.js` gestiona el JWT y depende de `api_client.js`. (Borra esta línea de ejemplo al crear la primera entrada real).*

---

## 🧹 Política de Mantenimiento

*Aplicar en cada `/ship` de tipo Major, o cuando este fichero supere las 200 líneas activas:*

- **Consolida** decisiones relacionadas en una sola entrada.
- **Archiva** lecciones ya internalizadas en el código: muévelas a `memory.archive.md` (créalo si no existe).
- **Elimina** entradas que describan decisiones revertidas o ya obsoletas.
- **Objetivo:** mantener `memory.md` por debajo de ~200 líneas activas para que la IA pueda leerlo íntegramente en cada sesión sin pérdida de atención.
