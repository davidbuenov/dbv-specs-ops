# 🧠 Memory & Context

> **Frontera de uso (Memory vs. Tasks):**
> - `task.md` → progreso **operativo**: checklist de tareas, Snapshot de Contexto (el paso exacto siguiente), y estado de la sesión.
> - `memory.md` → contexto **cualitativo y temático**: conocimiento persistente, decisiones técnicas profundas, lecciones, y el área del producto en foco (no el paso específico).
> Si hay info que sirva para los dos, prioriza: datos con fecha/paso exacto → `task.md`; razonamiento/por-qué/lecciones → `memory.md`.
>
> *Instrucción para la IA: Consulta este archivo al inicio de cada sesión para recuperar el hilo técnico. Actualiza las secciones correspondientes cuando el workflow lo indique (triggers en `/plan`, `/build`, `/test` y gate en `/ship`).*

## 🎯 Contexto Activo
- **Estado actual del desarrollo:** Release de la versión v2.0.0 finalizada con éxito. El framework ha sido actualizado por completo para soportar la metodología de ingeniería agéntica y optimización del arnés.
- **Foco inmediato:** Distribución y documentación de la v2.0.0 para desarrolladores de equipo.

## 🏗️ Log de Decisiones Técnicas (ADR Ligero)
*Registro de por qué se tomaron ciertas rutas (ej. cambios en librerías, arquitectura o patrones).*

- **2026-06-15 - Transición a dbv-specs-ops v2.0.0 (Agentic Engineering):** Implementación de los principios del libro blanco de Google. Se unificaron los Evals no deterministas de IA en la fase `/test` para simplificar el flujo, y se añadió la auditoría de seguridad en `/code-simplify` para evitar la fuga de credenciales o de paquetes alucinados (*slopsquatting*). Se transicionó la sección MCP en la arquitectura a una definición explícita de Arnés (Harness) del Agente.

## ⚠️ Lecciones Aprendidas / Errores Evitados
*Notas sobre bugs específicos, configuraciones que fallaron o refactors intentados para no repetirlos.*

- **[Feedback de Usabilidad]**: Es mejor integrar los conceptos nuevos (como Evals) en las fases existentes (`/test`) y delegar los modos de ejecución (Conductor/Orquestador) de forma implícita, en lugar de sobrecargar al desarrollador final con configuraciones complejas o preguntas confusas.
- **[Estructura de Onboarding]**: En proyectos existentes con archivos raíz consolidados (como `README.md` y `CHANGELOG.md`), es preferible descargar el framework completo en una subcarpeta dedicada (`dbv-specs-ops/`) e indicar al agente su ubicación a través de un archivo de activación ligero (`CLAUDE.md`, `GEMINI.md`). Esto evita colisiones de archivos y mantiene limpio el código de producción.


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
