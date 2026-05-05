# Antigravity Reference — SDD Extended Behavior

> **How Antigravity loads context:** `GEMINI.md` is **automatically read** from the workspace root at the start of every session (same mechanism as Gemini CLI). You do NOT need to paste anything manually.
>
> This file documents the **Antigravity-specific features** built on top of the base SDD workflow defined in `GEMINI.md`. Read it if you want to take full advantage of Planning Mode, Knowledge Items, and cross-session persistence.

---

This project follows **Spec-Driven Development (SDD)**. Read these files at the start of each session before proposing any code or plan:

| File | Purpose |
|---|---|
| `docs/MASTER_PROMPT.md` | Mandatory workflow, rules and boundaries |
| `docs/SPECIFICATIONS.md` | Current project requirements |
| `docs/ARCHITECTURE.md` | Stack and technical decisions |
| `docs/DESIGN.md` | Visual design system: color tokens, typography, components and philosophy *(if it exists)* |
| `task.md` | Current state + Context Snapshot |

## Automatic Project State Detection

At session start, check if `docs/SPECIFICATIONS.md` has real content (not just placeholders):

- **If empty or placeholders only** → The project has no specs yet. Inform the user and follow the flow defined in `docs/ADOPTION_PROMPT.md` to reconstruct context.
- **If filled** → The project already uses SDD. Check `task.md` to resume from the Context Snapshot.

## Antigravity-Specific Behavior

- **Planning Mode**: When creating a plan, activate Antigravity's native Planning Mode. Create the artifacts (`implementation_plan.md`, `task.md`, `walkthrough.md`) **inside the project workspace root** — not only in the conversation brain directory — so they are versioned with the project and accessible to other AI platforms and team members.
- **Knowledge Items (KIs)**: After completing a significant milestone, offer to create a Knowledge Item summarizing the project context. This enables seamless context recovery in future sessions without rereading all docs.
- **Context Snapshot**: At the end of each session or before a conversation limit, write a Context Snapshot to `task.md` with the exact next step so the work can be resumed instantly.

## Expected Behavior

- Always follow the cycle: **Spec → Plan → Build → Test → Simplify → Ship**
- Before coding, confirm that the "what" is defined in `docs/SPECIFICATIONS.md`
- After each milestone, update the Context Snapshot in `task.md`
- If critical information is missing, ask — do not assume
- Check the standards repo indicated in `docs/MASTER_PROMPT.md` before writing code in a new language

## To adapt this template to a new project

1. Fill `docs/SPECIFICATIONS.md` with the new project context
2. Fill `docs/ARCHITECTURE.md` with the chosen tech stack
3. If the project has a user interface, fill `docs/DESIGN.md` with the visual design system
4. Update the Snapshot in `task.md`
5. Update `README.md` with the new project name and status

---

> 🛠️ SDD Framework created by **[David Bueno Vallejo](https://github.com/davidbuenov)** — free and open source · [dbv-specs-ops](https://github.com/davidbuenov/dbv-specs-ops)
