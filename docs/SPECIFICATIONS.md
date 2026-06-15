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

## 🚫 5. Fuera de Alcance (Out of Scope)
*Vital para evitar el "scope creep" (crecimiento descontrolado del proyecto).*

- [ ] [Funcionalidad o aspecto que NO se abordará en esta fase/versión].

## ⚠️ 6. Riesgos y Mitigación
*Anticipar problemas es de ingenieros senior.*

- **Riesgo:** [Ej: La API externa tiene límites de tasa (Rate limiting)].
  - **Mitigación:** [Ej: Implementar un sistema de caché local].
- **Riesgo de Seguridad y Privacidad (IA/Datos):** [Ej: Fuga de secretos, inyección de código vulnerable por parte del agente, o alucinación de paquetes dependientes].
  - **Mitigación:** [Ej: Implementar hooks deterministas de pre-commit con escaneo de secrets como gitleaks, o auditoría obligatoria de dependencias en /code-simplify].

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