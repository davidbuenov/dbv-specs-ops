# AGENTS.md — Project Instructions for AI Coding Agents

This project follows **Spec-Driven Development (SDD)**. Read these files at the start of each session before proposing any code or plan:

| File | Purpose |
| --- | --- |
| `project.config.md` | Project identity: name, author, license and file header template |
| `docs/MASTER_PROMPT.md` | Mandatory workflow, rules and boundaries |
| `docs/SPECIFICATIONS.md` | Current project requirements |
| `docs/ARCHITECTURE.md` | Stack and technical decisions |
| `docs/DESIGN.md` | Visual design system: color tokens, typography, components and philosophy *(if it exists)* |
| `memory.md` | **Context and Decisions:** Qualitative knowledge (ADRs, lessons learned, active context) |
| `task.md` | Current state + Context Snapshot |

> **Path note (subfolder isolation):** In consumer projects where this framework lives inside the `dbv-specs-ops/` subfolder, prefix every path in the table above with `dbv-specs-ops/` (e.g. `dbv-specs-ops/docs/MASTER_PROMPT.md`, `dbv-specs-ops/task.md`). If `docs/MASTER_PROMPT.md` exists at the workspace root, you are inside the framework repository itself and the paths are those in the table.

## ⚠️ Core Rules (Strong Pointer)

**Read `docs/MASTER_PROMPT.md` and follow its workflow strictly. If you detect contradictions between the prompt and project specs, halt and report before proceeding.**
All initialization logic (Bootstrap), state checking (Specs Check), lifecycle (Workflow) and coding standards are centrally defined there to avoid cognitive redundancy.

---

> 🛠️ SDD Framework created by **[David Bueno Vallejo](https://github.com/davidbuenov)** — free and open source · [dbv-specs-ops](https://github.com/davidbuenov/dbv-specs-ops)
