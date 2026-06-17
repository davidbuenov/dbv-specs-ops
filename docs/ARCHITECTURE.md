# 🏗 Arquitectura Técnica: [Nombre del Proyecto]

> **Fase:** `/plan` (Planificación Técnica)
> **Estado:** Borrador / Validado
> **Última Revisión:** [Fecha]

---

## 🛠 Stack Tecnológico

| Capa | Tecnología | Justificación |
| --- | --- | --- |
| **Lenguaje** | [Ej: TypeScript 5.x] | [Ej: Tipado estático, ecosistema maduro] |
| **Framework principal** | [Ej: Fastify / React] | [Ej: Alto rendimiento / SPA sin complejidad SSR] |
| **Persistencia** | [Ej: SQLite / PostgreSQL] | [Ej: Sin infra para MVP / producción] |
| **Autenticación** | [Ej: JWT + bcrypt] | [Ej: Stateless, fácil de escalar] |
| **Testing** | [Ej: Vitest / Pytest] | [Ej: Rápido, compatible con ESM] |
| **CI/CD** | [Ej: GitHub Actions] | [Ej: Integrado con el repo] |

---

## 📂 Estructura de Directorios

```text
/
├── src/
│   ├── domain/          # Lógica de negocio pura (sin dependencias externas)
│   ├── application/     # Casos de uso, orquestación
│   ├── infrastructure/  # BD, APIs externas, servicios externos
│   └── interfaces/      # Controladores HTTP, CLI, WebSocket
├── tests/
│   ├── unit/
│   └── integration/
├── docs/                # Documentación del proyecto (este directorio)
└── [config files]       # tsconfig, .env.example, etc.
```

> Adapta esta estructura al stack elegido. Si es un proyecto pequeño, una sola carpeta `src/` plana es suficiente.

---

## 🔑 Decisiones Técnicas Clave

### Seguridad

- **Autenticación:** [Ej: JWT con expiración de 1h + refresh token en httpOnly cookie]
- **Autorización:** [Ej: RBAC — roles definidos en BD]
- **Datos sensibles:** [Ej: Variables de entorno via `.env`, nunca en código]

### Estilo de Código

- **Paradigma:** [Ej: Funcional preferente / Orientado a objetos]
- **Convenciones:** Ver repo de referencia en `MASTER_PROMPT.md`
- **Complejidad máxima por función:** [Ej: 20 líneas / complejidad ciclomática < 5]

### Gestión de Estado

- [Ej: Estado del servidor en BD, estado UI en React Context (sin Redux hasta que escale)]

---

## 🔗 Integraciones Externas

| Servicio | Propósito | Notas / Límites |
| --- | --- | --- |
| [Ej: Stripe API] | [Pagos] | [Rate limit: 100 req/s] |
| [Ej: SendGrid] | [Email transaccional] | [Free tier: 100 emails/día] |

---

## ⚠️ Restricciones y Riesgos Técnicos

- **Restricción:** [Ej: El despliegue debe ser en un VPS de 1GB RAM — optimizar footprint]
- **Riesgo:** [Ej: Dependencia de API de terceros sin SLA garantizado]
  - **Mitigación:** [Ej: Circuit breaker + caché local de 5 min]

---

## 🤖 Agent Harness (Arnés del Agente)

> Rellena esta sección para configurar la infraestructura, el contexto y las herramientas que rodean al agente de IA para que trabaje de forma segura y autónoma.

### 1. Gestión de Contexto (Context Engineering)
- **Contexto Estático:** [Ficheros de reglas globales y memory cargados siempre en el arranque (ej: CLAUDE.md, GEMINI.md, memory.md)].
- **Contexto Dinámico / Skills:** [Lista de módulos de habilidades en skills/ o pipelines RAG cargados bajo demanda por el agente].

### 2. Herramientas y MCP (Model Context Protocol)
- **Servidores MCP Requeridos:** [Ej: filesystem, sqlite (para acceso estructurado a datos), github (para gestión de PRs)].
- **Propósito:** [Ej: Conexión directa a base de datos de staging para consultas de contexto].
- **Configuración de Herramientas:** Ver `.claude/settings.json`, `.windsurfrules` o equivalentes.

### 3. Entorno de Ejecución (Sandboxing)
- **Aislamiento:** [Define el sandbox donde corre el agente. Ej: Docker local, máquina virtual, o entorno virtual local (venv)].
- **Límites de Ejecución:** [Límites de coste de tokens, tiempos de timeout o número máximo de iteraciones en comandos asíncronos].

### 4. Guardrails Deterministas de Seguridad
- **Filtros de Código:** [Definición de scripts automáticos (linters, pre-commit hooks con gitleaks, herramientas SAST) para evitar la filtración de secretos o dependencias ficticias generadas por la IA].
- **Políticas de Commit/Push:** [Ej: Bloquear commits que contengan strings que parezcan API keys o passwords].

### 5. Interfaz Externa para Agentes (Agent Readiness)
*Define la arquitectura y métodos que permiten a agentes externos descubrir y consumir los servicios del sitio:*
- **Autodescubrimiento**: [Describe cómo se exponen los recursos de IA (ej: Link Headers en el servidor web inyectando las tarjetas de agente, api-catalog, etc.)].
- **Protocolos y Tarjetas**: [Ubicación de tarjetas de agente (agent.json) y mcp.json. Detalla el soporte para el protocolo de contexto Model Context Protocol (MCP) y WebMCP en cliente].
- **Formato del Contenido**: [Define las políticas de optimización de contexto, tales como la negociación dinámica de Markdown para cabeceras Accept: text/markdown y la estructura de agent-skills/].

---

**Instrucción para la IA:** Respeta las decisiones y configuraciones del arnés documentadas aquí. Si necesitas desviarte por un motivo técnico o sugerir una nueva herramienta MCP/Skill para el proyecto, regístralo como "Decisión Técnica" en `memory.md` y obtén la aprobación del desarrollador.
