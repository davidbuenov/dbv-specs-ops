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

## 🤖 MCP (Model Context Protocol)

> Rellena esta sección si el proyecto usa servidores MCP para conectar la IA con servicios externos.

- **Servidor MCP:** [Ej: filesystem, github, sqlite]
- **Propósito:** [Ej: Acceso directo a la BD para queries de contexto]
- **Configuración:** Ver `.claude/settings.json` o `CLAUDE.md`

---

**Instrucción para la IA:** Respeta las decisiones documentadas aquí. Si necesitas desviarte por un motivo técnico, regístralo como "Decisión Técnica" en este archivo con fecha y justificación.
