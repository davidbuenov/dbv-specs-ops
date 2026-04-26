# Project Documentation / Documentación del Proyecto

This directory contains the operational documents that guide AI-assisted development.
Este directorio contiene los documentos operativos que guían el desarrollo asistido por IA.

| File | Role | When to use |
|---|---|---|
| [`MASTER_PROMPT.md`](./MASTER_PROMPT.md) | Rules, workflow and AI constraints | Attach in the first message if not using Claude Code, Copilot, Gemini CLI or Windsurf |
| [`SPECIFICATIONS.md`](./SPECIFICATIONS.md) | Requirements, users and acceptance criteria | Fill in during **Phase 0 (Spec)** before planning |
| [`ARCHITECTURE.md`](./ARCHITECTURE.md) | Stack, directory structure and technical decisions | Fill in after Phase 0, before **Phase 1 (Plan)** |
| [`ADOPTION_PROMPT.md`](./ADOPTION_PROMPT.md) | Onboarding SDD onto an existing project | Use instead of Phase 0 if you already have code |

## Document Flow / Flujo entre documentos

```
MASTER_PROMPT   →  defines HOW the AI works
SPECIFICATIONS  →  defines WHAT we build and WHY
ARCHITECTURE    →  defines WITH WHAT and HOW we build it
task.md (root)  →  records state and ensures continuity between sessions
```

The AI must read these three files at the start of each session before writing a single line of code.
La IA debe leer estos tres archivos al inicio de cada sesión antes de escribir una sola línea de código.
