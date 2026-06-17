# 📋 Especificaciones: [Nombre del Proyecto]

> **Fase:** `/spec` (Especificación)
> **Estado:** En Definición / Validado
> **Última Revisión:** [Fecha]

---

## 🎯 1. Contexto y Objetivos
*Basado en la filosofía de "entender el problema antes de proponer la solución".*

- **Problema:** [Describe el dolor o necesidad que motiva este proyecto. ¿Qué está roto o qué falta?]
- **Objetivo (Éxito):** [¿Cómo sabremos que este proyecto ha tenido éxito? Define un resultado tangible.]

## 👥 2. Usuarios y Escenarios
*Identifica para quién construimos y en qué situaciones usarán el sistema.*

- **Perfil de Usuario:** [Ej: Desarrollador, Administrador de Hospital, Usuario final].
- **Escenarios Clave:**
  - *Escenario A:* [Ej: "El usuario necesita consultar el historial médico en menos de 2 segundos"].
  - *Escenario B:* [Ej: "El sistema debe alertar si hay una colisión de horarios"].

## ✨ 3. Funcionalidades Principales (Requisitos)
*El "Qué" del sistema. Estas tareas se trasladarán luego a `task.md`.*

- [ ] **Funcionalidad A:** [Descripción breve y criterio de aceptación].
- [ ] **Funcionalidad B:** [Descripción breve y criterio de aceptación].

## 🏗️ 4. Propuesta de Solución Técnica (Resumen)
*Enlace directo con `ARCHITECTURE.md`.*

- **Enfoque:** [Breve descripción de la solución técnica elegida].
- **Dependencias Críticas:** [Ej: API externa, Servidor MCP específico].
- **Oportunidades de Skills y MCPs**: [Analizar si el proyecto se beneficia de la creación de un servidor MCP local para conectar con la lógica interna, o de paquetes de habilidades dinámicas (skills/) para facilitar la orquestación del agente].
- **Sistema de Diseño:** Si el proyecto tiene interfaz de usuario, ver `docs/DESIGN.md` para tokens de color, tipografía y componentes.

### 4.1. Agent Readiness Checklist (Proyectos Web)
*Si la configuración de Agent Readiness (Web) está activa, documentar las tareas de descubrimiento para agentes inteligentes:*
- [ ] **robots.txt**: Configurar con directiva `Content-Signal: ai-train=no, search=yes, ai-input=yes` y ruta al sitemap.
- [ ] **llms.txt**: Crear mapa de contenidos en Markdown para agilizar la lectura semántica de la IA.
- [ ] **auth.md**: Describir los procesos de registro y acceso para los bots.
- [ ] **Metadatos en `.well-known/`**: Crear `api-catalog`, `oauth-protected-resource`, `oauth-authorization-server` y `http-message-signatures-directory`.
- [ ] **Agent & MCP Cards**: Declarar la identidad del bot (`agent.json`) y la conexión al servidor MCP (`mcp.json`).
- [ ] **agent-skills/**: Definir el índice `index.json` y los manifiestos `SKILL.md` de habilidades del proyecto.
- [ ] **Negociación de Markdown**: Configurar el enrutamiento para retornar texto plano Markdown con la cabecera `Accept: text/markdown` y definir las cabeceras `Link` HTTP en el hosting.

## 🚫 5. Fuera de Alcance (Out of Scope)
*Vital para evitar el "scope creep" (crecimiento descontrolado del proyecto).*

- [ ] [Funcionalidad o aspecto que NO se abordará en esta fase/versión].

## ⚠️ 6. Riesgos y Mitigación
*Anticipar problemas es de ingenieros senior.*

- **Riesgo:** [Ej: La API externa tiene límites de tasa (Rate limiting)].
  - **Mitigación:** [Ej: Implementar un sistema de caché local].
- **Riesgo de Seguridad y Privacidad (IA/Datos):** [Ej: Fuga de secretos, inyección de código vulnerable por parte del agente, o alucinación de paquetes dependientes].
  - **Mitigación:** [Ej: Implementar hooks deterministas de pre-commit con escaneo de secrets como gitleaks, o auditoría obligatoria de dependencias en /code-simplify].
- **Riesgo de Consumo de Contexto de IA / Mal Rastreo de Bots:** [Ej: Los agentes inteligentes consumen demasiados tokens interpretando código HTML complejo o se pierden en los formularios de registro].
  - **Mitigación:** [Ej: Implementar un archivo llms.txt con el mapa web en Markdown y configurar la negociación dinámica de contenido en formato text/markdown].

## ❓ 7. Preguntas Abiertas
*Cosas que aún no sabemos o decisiones que dependen del usuario.*

- [ ] ¿Necesitamos soporte offline desde el primer día?
- [ ] ¿Qué volumen de datos esperamos manejar en el primer mes?

## 🧪 8. Criterios de Evaluación y Evals (No Deterministas)
*Define las rúbricas y métricas de calidad para evaluar la salida de componentes no deterministas (IA, prompts, etc.) integrados en la fase /test.*

- [ ] **Métricas de Output:** [Ej: Precisión de respuesta, conformidad de formato JSON, ausencia de alucinaciones].
- [ ] **Métricas de Trayectoria:** [Ej: Eficiencia en el uso de herramientas MCP, límite de llamadas a la API].

---
**Instrucción para la IA:** No pases a la fase `/plan` hasta que las "Preguntas Abiertas" críticas hayan sido resueltas o tengan un camino de solución definido.