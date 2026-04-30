# Changelog — dbv-specs-ops

All notable changes to this framework are documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [Sin publicar] / [Unreleased]

---

## [1.2.1] — 2026-04-30

### Added
- **`README.template.md`** — base skeleton for user projects.
- **Bootstrap Language question** — a 5th question added to the initial interview to choose the project's documentation language (ES, EN, or Bilingual).
- **Project README auto-generation** — the AI now generates a customized `README.md` for the user project using the template and then deletes the `.template` file.
- **Reference Section for Phases** — detailed table in the main README explaining the 6 development phases (/spec, /plan, /build, /test, /code-simplify, /ship) and their trigger commands.

---

## [1.2.0] — 2026-04-30

### Added
- **`project.config.md`** — new root file for project identity: name, author/company, license and file header templates for all languages (JS, Python, HTML, CSS, Java, C#, Go).
- **Bootstrap interview** — at session start, the AI detects placeholders in `project.config.md` and asks 4 questions one by one before the Engineering Interview:
  1. Project name
  2. Author / company (+ optional URL)
  3. License (MIT by default)
  4. Git version control (⭐ HIGHLY RECOMMENDED, enabled by default)
- **File headers** — every new source file must include a copyright header adapted to the language. The `dbv-specs-ops` credit line is mandatory in all headers.
- **`LICENSE` auto-generation** — the AI generates the `LICENSE` file after the bootstrap interview based on the chosen license.
- **Git integration** — if the user accepts Git in bootstrap: `git init`, `.gitignore` generation (stack-aware), and first commit (`chore: project initialized with dbv-specs-ops`). On each `/ship`: Conventional Commits message, version tag, and push suggestion (not auto-executed).
- **`CHANGELOG.md` auto-management** — the AI accumulates entries in `[Unreleased]` during `/build` and `/test`, and publishes them under a new versioned section on each `/ship`.
- **Semantic versioning on `/ship`** — the AI presents a clear 4-option menu: Patch / Minor ✅ / Major / No change.
- **Multiplatform startup scripts on `/ship`** — always generates `start.cmd` + `stop.cmd` (Windows) and `start.sh` + `stop.sh` (macOS/Linux), with automatic `venv` activation for Python projects.
- **Python `venv`** — Python projects always get a local virtual environment (`venv/`) created before installing dependencies in `/build`.

### Changed
- **Quick Start (EN + ES)** completely rewritten: per-platform table with exact first message to type (`/plan`), explicit Antigravity guidance, clear new vs. existing project paths.
- **Step 1** clarified: two safe options (GitHub Template button and Download ZIP), with explicit warning not to clone directly.
- **`GEMINI.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, `.windsurfrules`** — all updated with `project.config.md` in the startup reading list and two-step state detection (bootstrap check → specs check).
- **`MASTER_PROMPT.md`** — bootstrap section, `/build` CHANGELOG rule, `/test` CHANGELOG rule, and full `/ship` protocol added.
- **README file structure tables (EN + ES)** — `project.config.md` and `CHANGELOG.md` added to the root table.

---

## [1.1.0] — 2026-04-26

### Added
- **Bilingual README** (English / Spanish) with full Table of Contents and HTML anchors for navigation.
- **`ANTIGRAVITY.md`** — dedicated setup file for Antigravity (VS Code · Google DeepMind): Planning Mode artifacts, Knowledge Items, and Context Snapshot instructions.
- **Windsurf support** — `.windsurfrules` file for automatic context loading in Windsurf.
- **Visual Workflow diagram** (Mermaid) added to README.
- **`docs/ADOPTION_PROMPT.md`** — flow for adopting SDD in existing projects without specs.
- **`docs/README.md`** — index of all files in the `/docs` folder.

### Changed
- Core documentation files renamed to English for universal AI compatibility: `ESPECIFICACIONES.md` → `SPECIFICATIONS.md`, `ARQUITECTURA.md` → `ARCHITECTURE.md`.
- Artifact names standardized: `task.md`, `implementation_plan.md`, `walkthrough.md` — compatible with Antigravity's native Planning Mode.
- `GEMINI.md` updated with Antigravity-specific behavior section (Planning Mode + Knowledge Items).
- Credit line for David Bueno Vallejo added consistently across all platform files.

---

## [1.0.0] — 2026-04-15

Initial public release of the **dbv-specs-ops** SDD framework.

### Added
- **`docs/MASTER_PROMPT.md`** — the brain of the system: Senior Engineer rules, Spec→Plan→Build→Test→Simplify→Ship cycle, development standards and boundaries.
- **`docs/SPECIFICATIONS.md`** — template for project requirements: context, objectives, users, features, out-of-scope, risks and open questions.
- **`docs/ARCHITECTURE.md`** — template for technical decisions: stack, directory structure, key decisions, integrations and MCP section.
- **`task.md`** — logbook template with backlog, in-progress tracking and Context Snapshot for session continuity.
- **`CLAUDE.md`** — automatic context loading for Claude Code, Claude Desktop and Cursor.
- **`GEMINI.md`** — automatic context loading for Gemini CLI and Antigravity.
- **`.github/copilot-instructions.md`** — automatic context loading for GitHub Copilot.
- **`project.config.md`** placeholder for project identity.
- **`README.md`** — project documentation with origin, workflow and usage instructions.
- **`LICENSE`** — MIT License.

---

## How to read this file

- **Added** — new features or files.
- **Changed** — changes to existing functionality or documentation.
- **Deprecated** — features that will be removed in a future release.
- **Removed** — features removed in this release.
- **Fixed** — bug fixes.
- **Security** — security vulnerability fixes.

---

[Sin publicar]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.2.1...HEAD
[1.2.1]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/davidbuenov/dbv-specs-ops/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/davidbuenov/dbv-specs-ops/releases/tag/v1.0.0
