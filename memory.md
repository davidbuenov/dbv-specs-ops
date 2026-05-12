# 🧠 Memory & Context

> **Frontera de uso (Memory vs. Tasks):** 
> - `task.md` es para el progreso cuantitativo (el checklist de qué hacer, tareas terminadas, y el Snapshot de Contexto de la sesión).
> - `memory.md` es para el contexto cualitativo (conocimiento persistente, decisiones técnicas profundas, y el por qué de las cosas).
> 
> *Instrucción para la IA: Consulta este archivo al inicio de cada sesión para recuperar el hilo técnico. Sugiere actualizaciones aquí cada vez que tomes una decisión que desvíe el proyecto de la especificación original, o cuando resuelvas un bloqueo complejo.*

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
