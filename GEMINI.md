# Project Instructions for Gemini CLI & Antigravity

This project follows **Spec-Driven Development (SDD)**. Read these files at the start of each session before proposing any code or plan:

| File | Purpose |
| --- | --- |
| `docs/MASTER_PROMPT.md` | Mandatory workflow, rules and boundaries |
| `docs/SPECIFICATIONS.md` | Current project requirements |
| `docs/ARCHITECTURE.md` | Stack and technical decisions |
| `task.md` | Current state + Context Snapshot |

> **Note:** This file is auto-loaded by both **Gemini CLI** (`gemini` in terminal) and **Antigravity** (VS Code · Google DeepMind). For Antigravity-specific setup instructions (Planning Mode, Knowledge Items), see `ANTIGRAVITY.md`.

## Automatic Project State Detection

At session start, check if `docs/SPECIFICATIONS.md` has real content (not just placeholders):

- **If empty or placeholders only** → The project has no specs yet. Inform the user and follow the flow defined in `docs/ADOPTION_PROMPT.md` to reconstruct context.
- **If filled** → The project already uses SDD. Check `task.md` to resume from the Context Snapshot.

## Expected Behavior

- Always follow the cycle: **Spec → Plan → Build → Test → Simplify → Ship**
- Before coding, confirm that the "what" is defined in `docs/SPECIFICATIONS.md`
- When creating a complex plan, write it in `implementation_plan.md` and request approval before executing
- After each milestone, update the Context Snapshot in `task.md`; when done, complete `walkthrough.md`
- If critical information is missing, ask — do not assume
- Check the standards repo indicated in `docs/MASTER_PROMPT.md` before writing code in a new language

## Antigravity-Specific Behavior

*(Only applies when running in Antigravity / VS Code · Google DeepMind)*

- **Planning Mode**: When creating a plan, activate Antigravity's native Planning Mode. Create the artifacts (`implementation_plan.md`, `task.md`, `walkthrough.md`) **inside the project workspace root** — not only in the conversation brain directory — so they are versioned with the project and accessible to the team and other AI platforms.
- **Knowledge Items (KIs)**: After completing a significant milestone, offer to create a Knowledge Item summarizing the project context. This enables seamless context recovery in future sessions without rereading all docs.
- **Context Snapshot**: At the end of each session or before a conversation limit, write a Context Snapshot to `task.md` with the exact next step so work can be resumed instantly.

## To adapt this template to a new project

1. Fill `docs/SPECIFICATIONS.md` with the new project context
2. Fill `docs/ARCHITECTURE.md` with the chosen tech stack
3. Update the Snapshot in `task.md`
4. Update `README.md` with the new project name and status

---

> 🛠️ Framework SDD creado por **[David Bueno Vallejo](https://github.com/davidbuenov)** — libre y gratuito · [dbv-specs-ops](https://github.com/davidbuenov/dbv-specs-ops)
