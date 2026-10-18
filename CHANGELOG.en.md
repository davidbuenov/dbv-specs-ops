# Changelog — dbv-specs-ops (English)

English release notes for this framework. They are the first half of each GitHub Release
(`scripts/release-notes.py` puts them above the Spanish text). Versions 3.1.0 and 3.0.0 are
translated in full; earlier ones are condensed. The detailed reference, in Spanish, is
[CHANGELOG.md](./CHANGELOG.md).

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [3.1.0] — 2026-10-16

Phase graph and Rejection Memory, adapted from [*Procedural Graphs: Self-Evolving Execution Structures for LLM Agents*](https://arxiv.org/abs/2609.09153) (Lu et al., Google, arXiv:2609.09153). It adopts what works without the paper's infrastructure, verifies it deterministically and promises none of the paper's figures. **Backward compatible:** the upgrade does not touch `memory.md` or any project file.

### Added
- **`docs/PROCEDURAL_GRAPH.json` — the phase graph.** The `<workflow>` cycle as 16 `(source, relation, target)` transitions between 10 nodes, each with a `condition` (the gate), `guidance` (how to run the target step) and `pitfalls` (what to avoid). It includes the way back: Plan → Spec, Test → Build, Simplify → Build and Ship → Simplify. Maintain starts from the detector (`Start`), not from Ship.
- **`scripts/pg-guide.py`.** Standard library only (Python 3.8+, tested on 3.8.20, 3.11 and 3.13), UTF-8-safe output on Windows consoles, and compliant with its own linter.
  - `--validate`: checks node IDs and types, relations, the three attributes and duplicates; that the start node has no inputs and the end node no outputs; that every node is reachable from `start_node` and every node reaches `end_node`.
  - `--node <phase>`: prints a phase's context (inputs, outputs and a 2-hop horizon).
  - `--write-commands` / `--check-commands`: regenerate or verify each command's block.
- **Phase context in `.claude/commands/*.md`.** Each command carries, between `<!-- pg-guide:begin` and `<!-- pg-guide:end -->`, only its own phase's neighbourhood, generated from the graph. The command's own text is untouched.
- **Rejection Memory.** A `## 🚫 Rejection Memory` section in `memory.md` and `docs/templates/memory.template.md` with `REJECTED-NN` entries (scope, attempt, reason and alternative). It is consulted before proposing in `/plan`, `/test`, `/code-simplify` and Maintain, as evidence and not as an order (`<trust_boundary>`). In existing projects the AI creates it the first time it needs it.
- **`docs/PROCEDURAL_GRAPHS.md`.** What was taken from the paper, what was not and why, and how to change the graph without breaking anything. **`docs/AGENTIC_ENGINEERING.md` §6:** procedural memory (CoALA taxonomy).
- **`docs/COMPARISON.md` / `docs/COMPARISON.en.md`:** a bilingual technical comparison of `dbv-specs-ops` against GitHub Spec Kit, BMAD Method and Kiro, checked against their official sites (15/10/2026), including a section on its own limitations (documentation for GitHub only; not used by the AI). `README.md` and `README.en.md` gain a "How Does It Differ from GitHub Spec Kit, BMAD or Kiro?" section (summary table plus a link to the full comparison) and its index entry.
- **Comparison:** *Gates between phases* and *Memory of rejected approaches* rows, checked against the official Spec Kit, BMAD and Kiro documentation on 16/10/2026; §9 of the analysis and a new limitation (the graph is a hand-written summary and its effect on quality is not measured).
- **GitHub Releases from the changelog** (`scripts/release-notes.py`, `docs/templates/release.workflow.yml`, `CHANGELOG.en.md`): a git tag is not a Release, and `/ship` only created the tag. `/ship` now offers, once per project, to publish each version as a GitHub Release with English notes first and Spanish below, either automatically (a workflow triggered by the tag) or by hand with `gh`. Nothing is installed without the user's yes.

### Changed
- **`MASTER_PROMPT.md`:**
  - v3.1.0 header.
  - A `<procedural_graph>` block: a short pointer, not a copy of the transitions, because that would inject the whole graph into every session.
  - Rejection Memory triggers in `/plan`, `/test` and `/code-simplify`.
  - Maintain keeps the end of long logs.
  - `/ship` offers the GitHub Release step.
- **`scripts/dbv-lint.py`:**
  - Validates the graph and fails if a framework command does not match it.
  - Only warns if the project's root `.claude/commands/` copy is out of date, and does not claim commands the user did not copy.
  - Without a graph (framework < 3.1.0) it checks nothing.
- **`scripts/run-evals.sh`:** a `📊 Resultado: P/T pasan (F fallan, I inconclusos)` summary, comparable across runs. It keeps the CLI allow-list, the `INCONCLUSO` check, the `jq` warning and the temp file with `trap`.
- **`evals/README.md`:** a no-regression rule — a change to the prompt, the graph or the commands must not lower the number of passing evals.
- **`docs/MAINTAIN.md`:** prepare the diagnosis context by keeping the tail of the log and consulting Rejection Memory.
- **`README.md` / `README.en.md`:** version 3.1.0, two new features, two rows in the comparison table, pillar 8 in *Origin & Inspiration*, an entry in *References* (linking the paper) and the new files in the structure.
- **Indexes and upgrade:** `docs/README.md` and `docs/UPGRADE_PROMPT.md` (v3.1.0 manifest with the exception for commands carrying the user's own text on a separate row, new URLs and `chmod +x` for `pg-guide.py`). `project.config.md` moves to 3.1.0.

### Not adopted (and why)
- **Automatic self-evolution of the graph behind an `S_val` gate.**
  - There is no validation set: `evals/` holds a single eval.
  - Each run is a non-deterministic, paid call.
  - `UPGRADE_PROMPT` would overwrite the mutated graph.

  It remains a maintenance rule (do not lower the number of passing evals). See `REJECTED-05` in `memory.md`.
- **A guide LLM that rewrites the subgraph at each step**, and **the paper's figures as our own promise.** All of its gains use that LLM, and the "70.9 % fewer tokens" figure is ALFWorld only, against generative guidance over the whole graph.
- **Changes to `docs/ARCHITECTURE.md`.** It is the user project's template: documenting the framework there would go against `<context_management>`.

---

## [3.0.0] — 2026-10-14

Spec-to-Test traceability, deterministic governance and path coherence. **Backward compatible:** a pre-v3.0.0 project upgrades without touching its specifications, backlog or code; requirement IDs are only mandatory for what is written from now on.

### Added
- **`AGENTS.md` — the open-standard activator** (Agentic AI Foundation / Linux Foundation; read by Codex, Cursor, Copilot, Windsurf, Jules, Zed, Aider and others). A thin pointer with the same rules and path note as `CLAUDE.md`/`GEMINI.md`. Claude Code (≥ 2.1.277) reads it only when there is no `CLAUDE.md` (verified with the real CLI: only `AGENTS.md` → read; both → `CLAUDE.md` wins), so it is never loaded twice; Gemini CLI reads it only if added to `context.fileName`. Bootstrap and `UPGRADE_PROMPT` never overwrite a project's own `AGENTS.md`: they only add the redirect lines.
- **Spec-to-Test traceability (`REQ-NN` ↔ `@spec`).** Every *new or modified* requirement in `SPECIFICATIONS.md` carries a unique, immutable ID at the start of its line (`REQ-01`, `RNF-01`…; the exact format does not matter if the prefix is `REQ`/`RF`/`RNF`). Anti-conflict rules: next ID = maximum + 1, never renumbered or reused (retired = `~~REQ-07~~`), and each parallel branch reserves a range. Code and tests cite the ID with `@spec REQ-NN` (`/build`, `/test`). Requirements without an ID in earlier projects are outside the rule.
- **`scripts/dbv-lint.py` — a deterministic linter.** Standard library only (Python 3.8+). It checks (1) the single-`return` rule via the AST, with leading Guard Clauses (also after an assignment or with `elif`) and no false positives on nested functions or generators; (2) the YAML frontmatter of `implementation_plan.md`; (3) traceability: requirements with no tag, in code only (no test), duplicates and tags pointing to missing IDs. `--changed` (only what you touched) and `--staged` (pre-commit) modes avoid flooding a legacy codebase with warnings; `--strict` for CI and `--root`. In a git repo it analyses only versioned files plus new non-ignored ones (it respects `.gitignore`: virtualenvs, builds, bundled runtimes; without git it prunes known folders and any venv with `pyvenv.cfg`) — on a real repo with 9,299 `.py` files it went from 19 s and 14,580 false findings to 2 s and 55 real ones. If `SPECIFICATIONS.md` uses no IDs, traceability is skipped without warnings.
- **Architect Review with evidence and confidence.** `<architect_review>` gains `<evidence>` (a quote from the spec/architecture/path where the friction is) and `<confidence_score edge_cases security concurrency>`; a `low` axis forces a stop and a request for validation before `/build`.
- **Hot/Warm/Cold context compression policy** for `memory.md` in `<context_management>` (aligned with the existing ~200-line maintenance policy).
- **Subagent contracts (`scope_contract`)** in `docs/PARALLEL_WORK.md`: allowed/forbidden files, requirement-ID range and a per-session call budget for each parallel session.
- **Clean templates `docs/templates/task.template.md` and `memory.template.md`**, used by bootstrap to reset `task.md`/`memory.md` when they hold the framework's own history (with explicit user confirmation).
- **Bootstrap question #8 and the `Desktop AI Assistant` field** in `project.config.md`; hooks from `/spec` and `/ship` into `AI_DESKTOP_ARCHITECTURE.md`, `AI_SPECIFICATIONS.template.md` and `IA.template.md`; a "Client-Embedded AI Subsystem (Desktop 3-Tier Engine)" section in `ARCHITECTURE.md` and item 4.2 in `SPECIFICATIONS.md`.
- **Rust/Tauri and IIFE standards** in `<coding_standards>`: `Result` in `#[tauri::command]` handlers, no `.unwrap()`/`.expect()` in IPC or at runtime (allowed at startup and in tests; enforced with `clippy::unwrap_used`/`expect_used`), and a bundler-less frontend exception (IIFE, `withGlobalTauri`).
- **Optional governance blocks in `project.config.md`** (`Source of truth`, `Active guardrails`, `Maintain (Phase 7)`), named as their guides use them.
- **`docs/REVIEW.md`:** audit of `keyring` key custody, path confinement (`..`), Tauri v2 traps, isolation in `api.js` (`runningInTauri`), `@spec` tags and running `dbv-lint.py --changed`. **`docs/GUARDRAILS.md`:** a pre-commit hook with `dbv-lint.py --staged` (Python), `clippy` (Rust) and the bash heuristic only for JS/TS.
- **Tauri project detection and `memory.md` creation** in `docs/ADOPTION_PROMPT.md` (adoption now generates four files) and ID assignment for documented requirements.

### Changed
- **`MASTER_PROMPT.md`:** v3.0.0 header; the single-`return` rule defines precisely what a Guard Clause is (the same definition the linter checks); `uv` first for Python (`.venv/` and `venv/` in `.gitignore`); `start`/`stop` scripts only for servers, APIs and resident services (desktop apps, libraries and CLIs document their native commands); `/code-simplify` runs `dbv-lint.py --changed` when there is Python.
- **Bootstrap detects a nested `.git` in `dbv-specs-ops/`** (the framework installed with `git clone`). With a nested repo, the project repository would only record a reference to the folder and never version `task.md`, `memory.md` or the specs; Bootstrap explains this and asks for confirmation to remove it (or respects it if the user wants a submodule).
- **Explicit source-repo exception:** if `docs/MASTER_PROMPT.md` is at the workspace root, bootstrap, the `task.md`/`memory.md` reset and the phase cycle do not run on the framework itself (formalising the lesson recorded in `memory.md`).
- **Activators** (`CLAUDE.md`, `GEMINI.md`, `ANTIGRAVITY.md`, `.windsurfrules`, `.github/copilot-instructions.md`): a path note for projects with the framework in `dbv-specs-ops/`; `ANTIGRAVITY.md` adds `project.config.md`; Antigravity planning artifacts go to `dbv-specs-ops/`.
- **`.claude/commands/`:** `dbv-specs-ops/`-prefixed paths, `memory.md` in `/spec`, and references to IDs, `@spec` and `dbv-lint.py`.
- **`README.template.md`:** `uv` installation, Rust/Tauri v2 sections, a tree with the `dbv-specs-ops/` subfolder and a link to `dbv-specs-ops/CHANGELOG.md`.
- **`README.md` / `README.en.md`:** a new "Get started in a minute" section at the top with the one message you need to give the AI (*"I want to create a project using the dbv-specs-ops framework you can find at github.com/davidbuenov"*), a table for new / existing / upgrade, and collapsible instructions for AI agents (download the GitHub `.tar.gz` without git history — there is no `.git` to delete, something sandboxes protect —, minimal activators, Bootstrap, nothing destructive without confirmation). Step-by-step installation becomes "Manual installation", "Adding it to an existing project" gets its own section and the index links only real sections.
- **`docs/UPGRADE_PROMPT.md`:** v3.0.0 manifest, download URLs, path and activator note, `memory.md` as a protected file, `SPECIFICATIONS.md`/`ARCHITECTURE.md` marked as templates (not overwritten) and the current version defined in a single place (Phase 1).
- **Repo hygiene:** `docs/actualizacion_dbv_specs_ops.md`, `docs/dbv-improvedesign.md` and `linkedin_post.md` removed from git (already integrated in v2.5.0/v2.3.0, of no value to someone cloning the framework; history keeps them) and `old/` added to `.gitignore`.

### Fixed
- **`docs/UPGRADE_PROMPT.md` (found upgrading a v2.9.0 project with real GitHub downloads):** the `run-evals.sh`/`evals/` row mixed the mandatory action with an "if the user already added their own evals, don't touch them", and the AI skipped the whole row, leaving the old `run-evals.sh`. They are now two rows (framework scripts are always updated; only the user's `.json` files are respected). It also explains that, if the whole repository was copied, `dbv-specs-ops/` holds inert copies of the activators that are updated like any framework file.
- **404 links in `README.md` and `README.en.md`:** 25 links per file pointed to `./dbv-specs-ops/…`, a path that does not exist in this repo; two links to files that are not versioned (`implementation_plan.md`, `walkthrough.md`) are also removed. The upgrade instructions now say `dbv-specs-ops/docs/UPGRADE_PROMPT.md`.
- **`docs/UPGRADE_PROMPT.md`:** Phase 4 wrote `Framework Version: 1.4.0` into `project.config.md`; the v2.9.0 table listed `AI_DESKTOP_ARCHITECTURE.md` twice instead of `docs/README.md`.
- **`scripts/run-evals.sh`:** resolves its path with `SCRIPT_DIR`/`FRAMEWORK_DIR` (works in the framework repo and from `dbv-specs-ops/`) and looks for `check_target` inside the framework folder too; the AI CLI is only accepted as a full `claude|gemini|copilot|cursor… -p` (it used to accept suffixes with shell metacharacters that ended up in an `eval`); it warns if `jq` is missing, uses its own temp file and marks as `INCONCLUSO` an eval whose pattern was already in the file before running the CLI (it would pass even if the AI did nothing; `EVALS_ALLOW_STALE=1` ignores this). The example eval is no longer vacuous (its `[Oo]lvid` pattern is not in the template). `evals/README.md` documents the real eval format.
- **Scope of `docs/GUARDRAILS.md`:** the pre-commit example no longer counts Python `return`s with `grep` (false positives); it now uses the `dbv-lint.py` AST.

---

## [2.9.0] — 2026-10-03

A canonical architecture for generative AI in desktop apps (hybrid local / cloud with API keys / subscription agents via ACP), a standard catalogue of AI requirements for SDD and bilingual end-user documentation templates, together with the full reference implementation in `dbv-tauri-starter` v0.2.0. It also includes critical CI/CD fixes for AppImage packaging and the Homebrew Cask in native desktop apps.

### Added
- **`docs/AI_DESKTOP_ARCHITECTURE.md` — the canonical 3-tier desktop AI architecture:** local models (Ollama, LM Studio on `localhost`), cloud models with an API key kept only in the OS keychain through Rust (`keyring`), and subscription agents via ACP (JSON-RPC over `stdin`/`stdout`) that reuse CLIs already signed in on the machine. Includes the atomic interaction cycle (unified-diff proposal plus human review before applying), zero-footprint lazy loading and reactive cancellation through `AbortController`.
- **`docs/templates/AI_SPECIFICATIONS.template.md`:** a modular requirements template for AI projects (RNF-IA.1–4: secure credential custody, guaranteed offline/local mode, strict privacy, zero footprint at rest; RF-IA-01–04: a document-aware assistant, a guided connections panel, atomic application of proposals with diff review, ACP support).
- **`docs/templates/IA.template.md` and `IA.en.template.md`:** bilingual end-user templates for any desktop app's help, explaining the difference between subscriptions (ACP), API keys and free local models, with transparency about privacy and cost.
- **Reference implementation in `dbv-tauri-starter` (v0.2.0):** a modular Rust backend (`src-tauri/src/ai/`), a vanilla JS frontend (`src/ai/`) and a 40-test suite (including a mock ACP transport and SSE streaming).
- **`docs/NATIVE_APPS_RELEASE_CI.md` §6quater — incremental AppImage updates (`.zsync`).** Three verified traps and the updated `release-linux.yml` template. Origin: DBV Typst Editor 0.12.0, tested on WSL.
- **`docs/MARKETPLACE_PUBLISHING.md` §1 — AppImageHub** as a channel.

### Fixed
- **`docs/NATIVE_APPS_RELEASE_CI.md` §6quinquies — the Homebrew Cask and Homebrew 7.** `preflight`/`postflight` Ruby blocks are deprecated in third-party taps; the new section covers `command_wrapper`, `generated_script` and declarative `preflight_steps`, plus the Linux trap where `app_image` now **moves** the AppImage. Reported by a DBV Typst Editor user on 2026-09-29.
- **`docs/NATIVE_APPS_RELEASE_CI.md` §6ter — the Tauri `.AppImage` did not start under another user.** linuxdeploy leaves `AppRun.wrapped` at `0770` inside a root-owned squashfs; firejail and the AppImageHub test fail with `Permission denied`. The `release-linux.yml` template now extracts, normalises permissions, repacks, verifies and replaces the asset. Found in production in two derived apps on 2026-09-28.

---

## [2.8.0] — 2026-09-01

A curated integration of the [AI-Native SDLC Playbook (Anthropic)](https://claude.com/blog/the-ai-native-sdlc-playbook): autonomous loop closure, layered code review with severity and deterministic guardrails — all provider-agnostic and optional except the review gate that already existed. It also fixes a usability bug seen in real use: bare phase commands (`/build`, etc.) typed with no extra text were not reliably interpreted.

### Added
- **`docs/MAINTAIN.md` — Phase 7 (optional, off by default).** Closes the Spec→Ship cycle with no human trigger: a deterministic detector (CI/cron on a metric) invokes the AI read-only to diagnose a deviation and write it up as a `[Detected automatically]` entry in `SPECIFICATIONS.md`, which re-enters the cycle through `/plan`. It never deploys or merges on its own.
- **`docs/REVIEW.md`.** Three review passes with severity (Bugs / Security / Compliance; Critical/Important/Nit) hooked into `/code-simplify`; Critical blocks `/ship`. The Compliance pass explicitly audits, function by function, the `<coding_standards>` of `MASTER_PROMPT.md` (single `return` + guard clauses, Result pattern, strict typing).
- **`docs/GUARDRAILS.md`.** Separates *advisory* instructions (`MASTER_PROMPT.md`) from deterministic git/CI guardrails (pre-commit, branch protection) that hold even if the model forgets a rule.
- **`docs/PARALLEL_WORK.md`** (concrete `git worktree` mechanics for 2–3 independent AI sessions), **`docs/SOURCE_OF_TRUTH.md`** (coexisting with Jira/ServiceNow), **`docs/METRICS.md`** (optional leading/lagging indicators readable from git alone).
- **`evals/` + `scripts/run-evals.sh`.** An optional regression suite for the agent's own configuration — it checks that the process still happens the same way after a configuration change.
- **`.claude/commands/{spec,plan,build,test,code-simplify,ship,maintain}.md`.** Native Claude Code commands (with autocomplete) for each phase.

### Fixed
- **Bare phase commands misread.** Typing only `/build` could be answered as if the code did not exist. Fixed with an explicit rule in `docs/MASTER_PROMPT.md`: a bare phase command always means "run that phase now", cascading through any missing earlier phases instead of rejecting the request; reinforced in Claude Code with real native commands.

### Changed
- **`docs/MASTER_PROMPT.md`** hooks into the 6 new documents; **`README.md` split into two single-language files** (`README.md` in Spanish, `README.en.md` in English); `docs/README.md` and `docs/UPGRADE_PROMPT.md` updated (v2.8.0 manifest).

---

## [2.7.0] — 2026-08-28

Lessons, patterns and technical traps gathered from several native apps published in official stores (Microsoft Store, Apple App Store, Uptodown…), all from real production debugging.

### Added
- **`docs/NATIVE_DESKTOP_APPS.md` §7 — Desktop Experience Definition of Done.** A Tauri build is not yet a desktop app: the difference is always the same six details (native file dialogs, full iconography from one `app-icon.svg`, universal shortcuts that also work with focus inside an input, native macOS menu, themed scrollbars and fluid layout, tooltips that announce shortcuts), raised to acceptance criteria. Plus two verification rules: launch the real executable and keep the version in sync across `package.json`, `tauri.conf.json`, `Cargo.toml` and the UI "About".
- **`docs/WEB_TO_DESKTOP_MIGRATION.md` §9 — the bundler path (React/Vue/Svelte + Vite):** stale native-event handlers captured in a `useEffect`, the linter walking Cargo's generated JS in `src-tauri/target/`, and dead environment-detection code duplicating the §3.1 adaptation layer.
- **`docs/NATIVE_APPS_RELEASE_CI.md` §6bis — a third-party Action's inputs change between versions and an invalid input does not break the build.** The reliable source is the `Unexpected input(s)` warning of the first real run.

### Changed
- **`docs/NATIVE_DESKTOP_APPS.md` §6 — four new traps** (10 → 14), **§4 item 4** (the user, never the AI, runs `tauri signer generate`), **`docs/MARKETPLACE_PUBLISHING.md` §3** (track the whole generated packaging folder), **`docs/NATIVE_APPS_RELEASE_CI.md` §6** (the local variant of the signing failure) and the **`docs/WEB_TO_DESKTOP_MIGRATION.md` §8** checklist.

---

## [2.6.0] — 2026-08-22

### Added
- **Migrating existing web apps to native desktop:** the new guide `docs/WEB_TO_DESKTOP_MIGRATION.md` covers the decisions that come *before* `NATIVE_DESKTOP_APPS.md` — four archetypes, adoption direction, one repo per app vs monorepo, dual desktop+web mode with a single adaptation layer (`api.js`), Rust-vs-sidecar per function, sidecar assembly, the hidden installer-size cost of an ML sidecar, copyleft licence audit, migration order by pipeline risk and a 12-point checklist.
- **`docs/NATIVE_DESKTOP_APPS.md` §3 — the IIFE is mandatory in ALL your own JS files**, because classic scripts share one global scope and a duplicate top-level identifier kills the second file with a silent *parse-time* `SyntaxError`. Includes the debugging technique (an inline `window.onerror` in `<head>`).
- **`docs/WEB_TO_DESKTOP_MIGRATION.md` — two real gotchas:** `frontendDist` when `src-tauri/` lives inside that same folder (the `scripts/sync-frontend.mjs` pattern), and `isTauri` renamed to `runningInTauri` because Tauri v2 already declares a global `isTauri`.
- **Phase Gates in the Master Prompt:** Bootstrap §7 requires resolving the 4 migration decisions before proposing a stack; a new web→desktop migration gate in `/plan`.
- Indexes, `UPGRADE_PROMPT` manifest (v2.6.0) and `project.config.md` updated.

---

## [2.5.1] — 2026-08-21

### Added
- **`docs/NATIVE_DESKTOP_APPS.md`:** a new §6 "Concrete Tauri v2 traps — permissions, WebView and threading" (8 real gotchas), item 9 (`core:webview:allow-print` for WKWebView) and §5 on file-association behaviour in `.deb` vs `.AppImage`.
- **`docs/MARKETPLACE_PUBLISHING.md`:** §6 on NSIS installer customisation traps and §7 on MSIX / package identity for stores.
- **`docs/NATIVE_APPS_RELEASE_CI.md`:** complete, copy-ready GitHub Actions workflow templates (`release-windows.yml`, `release-linux.yml`, `release-macos.yml`) validated against real runs.
- `UPGRADE_PROMPT` manifest (v2.5.1) and `project.config.md` updated.

---

## [2.5.0] — 2026-08-13

### Added
- **Native compiled desktop apps (Rust + Tauri v2):** a new guide `docs/NATIVE_DESKTOP_APPS.md` (Rust core vs web frontend, the bundler-less pattern and 8 transferable design lessons), a cross-platform CI/CD guide `docs/NATIVE_APPS_RELEASE_CI.md` (per-OS runners, accumulating draft Releases, explicit `GITHUB_TOKEN` permissions, Apple Silicon, unsigned artifacts as conscious debt) and a marketplace publishing guide `docs/MARKETPLACE_PUBLISHING.md`.
- **Phase Gates in `MASTER_PROMPT.md`:** a 4th stack option in Bootstrap §7, a native-app CI-matrix gate in `/plan` and a marketplace checklist gate in `/ship`.
- **Agent Plugins 1.0.0 support** (Google, Amazon, Microsoft, OpenAI, Vercel): a guide `docs/AGENT_PLUGINS.md`, the Agent Readiness checklist in `/spec` pointing at `.well-known/agent-plugin/`, `plugin.json`/`mcp.json` validation directives in `/test`, and an optional active-migration phase in `UPGRADE_PROMPT`.
- Indexes, templates, `UPGRADE_PROMPT` manifest and `project.config.md` updated.

---

## [2.3.0] — 2026-07-29

### Added
- **Design enrichment and audit:** a new guide `docs/DESIGN_ENRICHMENT.md` for the **Impeccable** and **SkillUI** workflow; an interactive step in `/spec` to install Impeccable and extract tokens; automatic sync of the root `DESIGN.md` in `/ship`; `/impeccable critique` (WCAG AA contrast, heuristics) in `/test`; `/impeccable polish` and `harden` in `/code-simplify`; cross-references and automatic upgrade to v2.3.0.

---

## [2.2.0] — 2026-07-26

### Added
- **Install and packaging support for Python and Node.js projects:** `/build` generates a minimal `pyproject.toml` (PEP 621) or `package.json` with `start` and `test` scripts.
- **A renewed `README.template.md`:** Installation (local and from a remote git repository) and Publishing (PyPI with `twine`, npm registry) sections.
- **Good-practice guidelines embedded** in `<coding_standards>` so they apply even offline: a single `return` + Guard Clauses, Result Pattern, strict typing, validation at the boundaries and specific exceptions.
- **Default technology recommendations in bootstrap** (FastAPI, Express/TypeScript, React/Tailwind, PostgreSQL/SQLite, uv/pnpm) and **frontend modularity rules** (no single-file React apps).

### Changed
- **Installation unified: subfolder only.** The framework always lives in a dedicated `dbv-specs-ops/` subdirectory to avoid collisions; exact activator-file contents are documented.
- **Strict context separation** in `MASTER_PROMPT.md` and `ADOPTION_PROMPT.md`: generated specs, architecture, backlog and READMEs refer only to the user's application, never to the framework's internals.

---

## [2.1.0] — 2026-06-17

### Added
- **Agent Readiness** for web projects and public APIs: `robots.txt` with Content-Signals, `llms.txt` and `auth.md`, OIDC/OAuth discovery metadata under `.well-known/`, bot and MCP server cards, an `agent-skills/` repository, Markdown content negotiation (`Accept: text/markdown`) and HTTP `Link` headers.
- Template updates (`project.config.md`, `SPECIFICATIONS.md`, `ARCHITECTURE.md`) and lifecycle hooks: `/spec` assesses feasibility, `/build` guides file creation and `/ship` verifies the injected Link headers.

### Changed
- **`README.md`:** an opening section describing the framework's differentiating features in English and Spanish.

### Fixed
- **`README.md`:** the Quickstart contradiction suggesting `/plan` as the first message instead of `/spec`.

---

## [2.0.0] — 2026-06-15

### Added
- **Agentic Engineering:** the concepts of Google's white paper *"The New SDLC With Vibe Coding"*, with a foundation guide `docs/AGENTIC_ENGINEERING.md`.
- **Implicit working modes:** automatic, invisible classification into *Conductor* (interactive, short loops) and *Orchestrator* (asynchronous background) during `/plan`.
- **Unified Evals in `/test`:** deterministic tests plus probabilistic evals (output rubrics, trajectory and hallucination checks).
- **Security audit in `/code-simplify`:** a mandatory review of AI-generated code against fake dependencies (slopsquatting), secret injection and vulnerable inputs.
- **Agent Harness** in `docs/ARCHITECTURE.md` (static vs dynamic context, sandboxing, security hooks) and **Model Routing Guidelines** in `project.config.md` to cut operating cost.
- **Active MCP and skill suggestions** in `/spec` and `/plan`, and an updated `UPGRADE_PROMPT` so existing projects can upgrade past v1.5.0.

---

## [1.5.2] — 2026-05-12

### Added
- **Trust Boundary (`<trust_boundary>`):** separates valid directives (the prompt's XML tags) from project data (`SPECIFICATIONS.md`, `task.md`, `memory.md`), preventing silent execution of imperative text injected into data files.
- **Granular Memory Triggers:** record in `memory.md` immediately when the Adversarial Review accepts a risk, when `/build` contradicts `ARCHITECTURE.md` or when a `/test` invalidates a `SPECIFICATIONS.md` assumption — no waiting for `/ship`.
- **Maintenance policy in `memory.md`:** keep it under 200 active lines, with consolidation instructions and `memory.archive.md`.

### Fixed
- **Anti-boilerplate Adversarial Review:** the `<adversary>` block must cite at least one concrete noun from `docs/SPECIFICATIONS.md`.
- **Bootstrap no longer runs `git init` automatically:** it shows the command and asks for explicit confirmation.
- **Clearer memory/task boundary:** `task.md` is operational (the exact next step), `memory.md` is thematic (qualitative context).

---

## [1.5.1] — 2026-05-12

### Fixed
- Documentation inconsistencies in `README.md` and `project.config.md` still describing the old interview flow; internal evaluation files added to `.gitignore`.

---

## [1.5.0] — 2026-05-12

### Added
- **Enforcement layer (hard law):** from suggested methods to strict contracts. A mandatory YAML frontmatter in `implementation_plan.md` (dependencies, risks, rollback), a mandatory `<memory_update_proposal>` Memory Gate in `/ship`, and a forced `<builder>` vs `<adversary>` Adversarial Review in `/plan`.

### Fixed
- **Cognitive redundancy:** the activator files (`CLAUDE.md`, `GEMINI.md`, `.windsurfrules`, `.github/copilot-instructions.md`, `ANTIGRAVITY.md`) are now light pointers to `docs/MASTER_PROMPT.md`.
- **Interview friction:** bootstrap and adoption propose a complete draft with marked `[ASSUMPTION: ...]` items instead of asking one question at a time.
- Template cleanup (`task.md`), consistency (`memory.md` in every adapter's reading list), and the `MASTER_PROMPT.md` header version.

---

## [1.4.0] — 2026-05-12

### Added
- **`memory.md`** — a standard file separating qualitative context (technical decisions, lessons, relationship map) from quantitative progress (`task.md`), preventing architectural drift and context loss.
- **Architect Review in `/plan`** — the AI validates first in a Software Architect role to catch edge cases and logic flaws before breaking down tasks.

### Changed
- **XML prompts:** `MASTER_PROMPT.md`, `ADOPTION_PROMPT.md` and `UPGRADE_PROMPT.md` rewritten with semantic XML tags to improve compliance in Claude, Gemini and GPT-4o-class models. `README.md`, `GEMINI.md` and `.gitignore` updated.

---

## [1.3.0] — 2026-05-05

### Added
- **`docs/DESIGN.md`** — a visual design-system template (YAML design tokens + prose) following Google Labs' [design.md standard](https://github.com/google-labs-code/design.md), with native **Dark Mode** support.
- **`docs/UPGRADE_PROMPT.md`** — the framework update agent: it detects the project's current version, computes the delta, downloads framework files from GitHub (with a manual fallback) and never touches project files.
- **`framework_version`** in `project.config.md`.

### Changed
- All activators read `docs/DESIGN.md` at session start; `/spec` creates it for UI projects; the READMEs gain upgrade sections.

---

## [1.2.1] — 2026-04-30

### Added
- **`README.template.md`** for user projects; a **Bootstrap language question** (ES, EN or bilingual); automatic generation of the project's `README.md`; and a reference table for the 6 phases.

---

## [1.2.0] — 2026-04-30

### Added
- **`project.config.md`** for project identity (name, author, licence, file-header templates) and a **bootstrap interview** at session start.
- **Mandatory file headers** with the `dbv-specs-ops` credit line, **`LICENSE` auto-generation**, **Git integration** (`git init`, stack-aware `.gitignore`, first commit; on each `/ship` a Conventional Commits message, version tag and push suggestion), **`CHANGELOG.md` auto-management**, **semantic versioning** on `/ship`, **multiplatform start scripts** and **Python `venv`** by default.

### Changed
- Quick Start rewritten per platform; activators and `MASTER_PROMPT.md` updated with the two-step state detection.

---

## [1.1.0] — 2026-04-26

### Added
- **Bilingual README**, **`ANTIGRAVITY.md`**, **Windsurf support** (`.windsurfrules`), a **Mermaid workflow diagram**, **`docs/ADOPTION_PROMPT.md`** (adopting SDD in existing projects) and a **`docs/README.md`** index.

### Changed
- Core files renamed to English (`SPECIFICATIONS.md`, `ARCHITECTURE.md`) and artifact names standardised (`task.md`, `implementation_plan.md`, `walkthrough.md`).

---

## [1.0.0] — 2026-04-15

Initial public release of the **dbv-specs-ops** SDD framework.

### Added
- **`docs/MASTER_PROMPT.md`** (the system's brain: Senior Engineer rules and the Spec→Plan→Build→Test→Simplify→Ship cycle), **`docs/SPECIFICATIONS.md`** and **`docs/ARCHITECTURE.md`** templates, **`task.md`**, the activators **`CLAUDE.md`**, **`GEMINI.md`** and **`.github/copilot-instructions.md`**, **`project.config.md`**, **`README.md`** and an MIT **`LICENSE`**.
