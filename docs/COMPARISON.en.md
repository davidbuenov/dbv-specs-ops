<!--
  Author: David Bueno Vallejo (@davidbuenov)
  Repository: https://github.com/davidbuenov/dbv-specs-ops
  License: MIT
  Purpose: Technical comparison between dbv-specs-ops and other Spec-Driven Development (SDD) frameworks.
-->

<p align="right">🇬🇧 English · <a href="./COMPARISON.md">🇪🇸 Leer en español</a></p>

# 📊 Comparison: `dbv-specs-ops` vs. other SDD frameworks

The **Spec-Driven Development (SDD)** ecosystem aims to solve the main problem of *"vibe coding"*: an AI assistant that codes aimlessly, loses context between sessions, hallucinates dependencies or breaks existing functionality.

[**GitHub Spec Kit**](https://github.com/github/spec-kit), [**BMAD Method**](https://github.com/bmad-code-org/BMAD-METHOD) and [**Kiro**](https://kiro.dev/) cover the specify → plan → implement flow well and, depending on the case, also review and verification. **`dbv-specs-ops` (v3.1.0)** shares that flow and adds the rest of the **software engineering lifecycle (6+1 phases)** around it: CLI-free initialization, traceability in the source code, agent/GEO readiness, cross-platform desktop apps, publication in official stores and post-deployment maintenance.

> **Legend:** ✅ Yes · ⚠️ Partial or a different approach · ❌ Not stated in its official documentation · ➖ Out of scope (it does not aim to cover it)

---

## 🔗 Official sources

The cells for the other three frameworks were checked against their official documentation on **2026-10-15**; the *Phase gates* and *Memory of discarded approaches* rows on **2026-10-16**. When a page does not detail something, the cell says so. These tools evolve quickly: if you spot something outdated, please open an issue.

| Framework | Official site | Pages consulted |
| :--- | :--- | :--- |
| **GitHub Spec Kit** | [github.com/github/spec-kit](https://github.com/github/spec-kit) · [documentation](https://github.github.io/spec-kit/) | [README](https://github.com/github/spec-kit) · [command reference](https://github.github.io/spec-kit/reference/overview.html) · [Agentic SDD](https://github.github.io/spec-kit/reference/agentic-sdd.html) · [`core` CLI](https://github.github.io/spec-kit/reference/core.html) · [upgrade](https://github.github.io/spec-kit/upgrade.html) · [existing projects](https://github.github.io/spec-kit/guides/existing-projects.html) · [SDD concept](https://github.github.io/spec-kit/concepts/sdd.html) |
| **BMAD Method** | [github.com/bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) · [documentation](https://docs.bmad-method.org/) | [README](https://github.com/bmad-code-org/BMAD-METHOD) · [install](https://docs.bmad-method.org/start/install-bmad/) · [review a change](https://docs.bmad-method.org/build/review-a-change/) · [autonomous loops](https://docs.bmad-method.org/build/autonomous-development-loops/) · [project context](https://docs.bmad-method.org/existing-codebases/set-and-maintain-project-context/) · [skills and agents](https://docs.bmad-method.org/reference/skills-and-agents/) · [Test Architect module](https://bmad-code-org.github.io/bmad-method-test-architecture-enterprise/) |
| **Kiro (AWS)** | [kiro.dev](https://kiro.dev/) | [Home](https://kiro.dev/) · [Specs](https://kiro.dev/docs/specs/) · [Correctness](https://kiro.dev/docs/specs/correctness/) · [Steering](https://kiro.dev/docs/steering/) · [Hooks](https://kiro.dev/docs/hooks/) · [License](https://kiro.dev/license/) |

> **BMAD version:** its current documentation describes **v7** (alpha, with a release planned for 2026-10-16), while the stable version is **v6.12**. Where they differ, this comparison follows the current official documentation and says so.

---

## 🤝 What they all share

Before the differences, what all four do well and in a similar way:

| Common feature | **dbv-specs-ops** | [**GitHub Spec Kit**](https://github.com/github/spec-kit) | [**BMAD Method**](https://github.com/bmad-code-org/BMAD-METHOD) | [**Kiro (AWS)**](https://kiro.dev/) |
| :--- | :---: | :---: | :---: | :---: |
| **Specify and plan before coding** | ✅ Spec → Plan → Build | ✅ `specify` → `plan` → `tasks` → `implement` | ✅ `bmad-spec` / `bmad-prd` → `bmad-architecture` → `bmad-build` | ✅ Requirements → design → tasks |
| **Specification artifacts as project files** | ✅ `docs/SPECIFICATIONS.md` | ✅ `spec.md`, `plan.md`, `tasks.md` | ✅ PRD, specs and `DESIGN.md` / `EXPERIENCE.md` | ✅ `requirements.md`, `design.md`, `tasks.md` |
| **Work broken down into tasks or stories** | ✅ `task.md` | ✅ Dependency-ordered `tasks.md` | ✅ Initiatives, epics and tickets (`bmad-ticket`) | ✅ Task list, run one by one or all at once |
| **Persistent project rules for the AI** | ✅ `MASTER_PROMPT.md` + `memory.md` | ✅ Constitution (`/speckit.constitution`) | ✅ `bmad-project-context` (verified block in `AGENTS.md`) | ✅ *Steering* files (`.kiro/steering/`) |

---

## 🏆 What sets them apart

| Category / Feature | **dbv-specs-ops (v3.1.0)** | [**GitHub Spec Kit**](https://github.com/github/spec-kit) | [**BMAD Method**](https://github.com/bmad-code-org/BMAD-METHOD) (v6.12 / v7 alpha) | [**Kiro (AWS)**](https://kiro.dev/) |
| :--- | :---: | :---: | :---: | :---: |
| **Philosophy and Scope** | Complete lifecycle (6+1 phases): from idea to code, stores and maintenance | SDD flow driven by a CLI and by commands your agent runs, extensible with extensions, presets and workflows | Agents (Analyst, PM, Architect, Developer, UX Designer) and *skills* to plan, build and review; optional modules (Test Architect, Game Dev…) | AI agent with an IDE (Code OSS), CLI, web and mobile; specs flow with *hooks* and *steering* |
| **Installation (Bootstrap)** | **1 natural-language sentence**, nothing to install (requires an assistant with shell and network access; in a web chat you paste `MASTER_PROMPT.md` by hand) | Requires Python 3.11+, `uv` and the `specify` CLI (`uv tool install specify-cli`) | An `npx` command (Node 20.12+, Python 3.10+ and `uv`); in v7 alpha, also as a Claude Code or Codex plugin | Installer for the IDE or the CLI; a web and a mobile version also exist |
| **Customization and Code Headers** | ✅ **Yes (`project.config.md`)**: automatic injection of author, license, date and purpose into every file, in any language | ❌ Not stated (*presets* adapt behavior, not code headers) | ❌ Not stated | ❌ Not stated |
| **Traceability in Code (`@spec`)** | ✅ **Yes**: requirements (`REQ-01`) cited in code and tests with `@spec` | ⚠️ `/speckit.analyze` (read-only) cross-checks spec, plan and tasks, e.g. "a task with no matching requirement"; no requirement → code or tests link is stated | ⚠️ Optional Test Architect module: its *Trace* skill will "Map requirements to tests and decide a release gate" | ⚠️ Traceable requirement ↔ property ↔ task link ("traceable link between your requirements and the tests that validate them"), IDE only and optional |
| **Own Deterministic Linter** | ✅ **Yes**: standard Python (0 deps). `@spec` traceability across several languages (py, ts/js, rs, go, java, kt, cs…); single-`return` rule **Python only**; `--changed` flag for legacy code | ❌ Not stated (`analyze` and `converge` are run by your agent) | ❌ Not stated | ❌ Not stated (*hooks* can run your linter or your tests when a file is saved) |
| **Agent Readiness and Web GEO / AEO** | ✅ **Yes**: prepares sites for AI search engines (GEO) and agents with `llms.txt`, `Content-Signals`, `.well-known` and Agent Plugins 1.0.0 / MCP | ➖ Out of scope | ➖ Out of scope | ➖ Out of scope |
| **Desktop Apps and Official Stores** | ✅ **Yes**: cross-platform Rust/Tauri v2 architecture (Win/Mac/Linux) + guides for **Microsoft Store, Homebrew and Uptodown** | ➖ Out of scope | ➖ Out of scope | ➖ Out of scope |
| **"AI-Native" Architecture Out of the Box** | ✅ **Yes (`v2.9.0+`)**: templates and architecture ready to connect Local AI, Cloud APIs or Agents (ACP/MCP) with the OS's secure keyring | ➖ Out of scope | ➖ Out of scope | ➖ Out of scope |
| **Start and Stop Scripts (`start` / `stop`)** | ✅ **Yes**: generates `start.cmd` / `stop.cmd` (Windows) and `start.sh` / `stop.sh` (macOS/Linux) for servers, APIs and resident services; in Python projects they activate the virtual environment automatically | ➖ Out of scope | ➖ Out of scope | ➖ Out of scope |
| **Git and Release Management** | ✅ **Yes**: from `git init` to `/ship` (updates `CHANGELOG.md` and `walkthrough.md`, proposes the commit and the `vx.y.z` tag; suggests the push and runs it when you ask) | ⚠️ Partial: git initialization and branching are an optional extension (`specify extension add git`), not installed by default; `/speckit.taskstoissues` exports tasks as issues; no release or changelog management is stated | ⚠️ Partial: `bmad-build-auto` commits but does not push; no versioning, branching or release skill is stated | ⚠️ Partial: Kiro Web coordinates changes across GitHub and GitLab repos and the headless CLI can review PRs in CI/CD; no versioning or changelog is stated |
| **Adversarial Review and Anti-Slopsquatting** | ✅ **Yes**: confidence self-assessment (edge cases, security, concurrency) + 3 passes in `REVIEW.md` (includes hallucinated-package detection) | ⚠️ `clarify`, `checklist` and `analyze` quality gates for "anything with meaningful ambiguity"; no adversarial review is stated | ⚠️ `bmad-code-review` (several independent reviewers and severity triage) and `bmad-review`, which supports lenses such as *adversarial*, *edge case* or *verification gap* | ⚠️ *Analyze Requirements* (IDE and CLI) finds "inconsistencies, ambiguities, and gaps" before design |
| **Validation: Tests + AI Evals** | ✅ **Yes**: unit/integration tests + **probabilistic Evals** for AI functions (the AI designs and runs them; the framework ships no runner of its own for your project's Evals) | ⚠️ `/speckit.tasks` generates tests inside each story phase "when requested"; the bug extension requires verification ("Missing verification is not a successful fix") | ⚠️ `bmad-qa-generate-e2e-tests` and the optional Test Architect module (8 skills: Automate, Test Review, Test Design, Framework, NFR, Trace…) | ⚠️ Property-based tests (*Correctness*, IDE only and optional by default) and a testing strategy in the design |
| **UI Design and Accessibility** | ✅ **Yes (optional)**: `design.md` (Google Labs), *Impeccable* heuristics and *SkillUI* reverse engineering | ❌ Not stated | ⚠️ `bmad-ux` and the UX Designer agent (`DESIGN.md`, `EXPERIENCE.md`); accessibility is not mentioned | ❌ Not stated |
| **Decision Memory and Parallel Sessions** | ✅ **Yes**: dual persistence (`memory.md` + `task.md`), Hot/Warm/Cold compression and *Scope Contracts* for parallel agents | ⚠️ Project constitution; `/speckit.implement` respects parallel markers in `tasks.md` | ⚠️ `bmad-project-context` (*refresh*, *record*, *audit*); parallel epic streams need "a higher layer" | ⚠️ Persistent *steering* (global and workspace, 4 inclusion modes); tasks run in parallel by *waves* and parallel agents (local or cloud) |
| **Phase gates (phase graph)** | ✅ **Yes (v3.1.0)**: `PROCEDURAL_GRAPH.json` describes every transition with `condition`, `guidance` and `pitfalls`, including the way back (Plan → Spec, Test → Build, Simplify → Build); each command receives only its phase context, generated from the graph and checked by the linter | ⚠️ "Run them in order, adding the clarify/checklist/analyze quality gates for anything with meaningful ambiguity": optional quality gates, no formal entry/exit conditions | ⚠️ Some skills have gated steps (e.g. `bmad-project-context` shows the block before writing); no general format for conditions between phases is documented | ⚠️ *Feature Specs* have approval gates between requirements, design and tasks (*Quick Spec* skips them) |
| **Memory of discarded approaches** | ✅ **Yes (v3.1.0)**: *Rejection Memory* in `memory.md` (`REJECTED-NN`), checked before proposing in `/plan`, `/test`, `/code-simplify` and `/maintain`, as evidence rather than an order | ❌ Not stated | ⚠️ `bmad-project-context` records *observed pitfalls* ("A pitfall goes in only when someone has seen the mistake"): mistakes seen, not discarded approaches | ❌ Not stated (*Markdown Review* comments are not described as persistent) |
| **Post-Delivery Maintenance (Loop)** | ✅ **Yes (Phase 7 `Maintain`, optional and off by default)**: CI/cron monitors that diagnose read-only and reopen specs | ⚠️ Optional bug-fixing extension (*assess*, *fix*, *validate*), triggered by the user | ⚠️ `bmad-build-auto`: unattended build and review loop; no post-delivery monitoring | ⚠️ *Bugfix* specs (`bugfix.md`) and a headless CLI for CI/CD |
| **Non-Destructive Upgrades** | ✅ **Yes**: updates framework versions without touching your specifications or your code | ✅ `specify self upgrade` + `specify integration upgrade`: leaves `specs/` and the constitution untouched; if you edited managed files, it stops and asks you (or use `--force`) | ⚠️ `bmad setup` updates modules and migrates `_bmad/custom/`; v7 is alpha and "can break an existing setup" | ➖ No file-migration procedure is stated; it is updated as a product |
| **Multi-Assistant Support and `AGENTS.md`** | ✅ **Yes**: open `AGENTS.md` standard, Claude Code, Gemini CLI, Copilot, Cursor and Windsurf | ✅ 38 integrations (Copilot, Gemini, Codex, Claude, Zed, Kiro…) plus a `generic` one | ⚠️ Any tool that supports *skills* (the documentation cites Claude Code, Codex and Cursor); uses `AGENTS.md` for project context | ⚠️ It is its own agent (IDE, CLI, web, mobile) with Anthropic, OpenAI and open-weight models; reads `AGENTS.md`, Skills and MCP |
| **License** | Open Source (MIT) | Open Source (MIT) | Open Source (MIT) | IDE and CLI are licensed as "AWS Content" under the AWS Customer Agreement; credit-based pricing; Kiro Crew is open source |

---

## ⚖️ Where `dbv-specs-ops` falls behind

An honest comparison also shows what is not done better.

| Category | **dbv-specs-ops** | [**GitHub Spec Kit**](https://github.com/github/spec-kit) | [**BMAD Method**](https://github.com/bmad-code-org/BMAD-METHOD) | [**Kiro (AWS)**](https://kiro.dev/) |
| :--- | :---: | :---: | :---: | :---: |
| **Community and maturity** (GitHub stars, 2026-10-15) | ❌ **29 ⭐** · 1 author · repo created April 2026 | ✅ **141,379 ⭐** · repo from August 2025 | ✅ **54,174 ⭐** · repo from April 2025 | ➖ Commercial AWS product |
| **Ecosystem and extensibility** | ⚠️ Agent Plugins guide and native Claude Code commands; no catalog of extensions or modules | ✅ Extensions, presets, workflows and bundles, with a community catalog | ✅ Official modules (Builder, Creative Intelligence, Test Architect, Game Dev…) and `bmad-customize` | ✅ *Hooks*, *steering*, Skills, MCP and ACP |
| **Built-in deterministic automation** | ⚠️ Git/CI guardrails are optional and set up by hand (`docs/GUARDRAILS.md`) | ⚠️ Extensions and workflows | ⚠️ Skills and unattended loops | ✅ *Hooks* that run shell commands on events (`PostFileSave`, `PreToolUse`, `PostToolUse`, `Stop`) |
| **Integrated IDE / visual interface** | ❌ No interface of its own: it depends on the assistant you use (in Claude Code, `/spec`, `/plan`… commands) | ❌ | ❌ | ✅ IDE with panels for specs, hooks and steering, plus web and mobile |

By stars, `dbv-specs-ops` is the last of the three open-source projects, and by a wide margin. And these are the framework's concrete limitations:

1. **Advisory rules by default.** `MASTER_PROMPT.md` itself says so in `<boundaries>`: its rules are *advisory* and depend on the AI following them. The linter, pre-commit hooks and CI guardrails are optional and must be set up (`docs/GUARDRAILS.md`); if they are not enabled, compliance depends on the AI.
2. **Context cost.** `MASTER_PROMPT.md` weighs about 38 KB (222 lines; on the order of 10 thousand tokens, an estimate) and is loaded together with the specs, `task.md` and `memory.md` in every session. On small projects or tiny changes this is overhead: Conductor Mode and Hot/Warm/Cold compression mitigate it, but do not remove it.
3. **The linter checks presence, not correctness.** It verifies that every requirement has its `@spec` tag in code and tests, and that there are no duplicate IDs or tags pointing to nonexistent requirements. It does not verify that the implementation actually meets the requirement: the tests do that.
4. **Python-centric linter.** The single-`return` rule is verified through the AST only in `.py` files; JS/TS get a bash heuristic and Rust relies on `clippy`. Only `@spec` traceability covers several languages.
5. **Opinionated, mandatory standards.** A single `return`, the `Result` pattern, Pydantic/Zod validation and a specific header style are marked as mandatory. They may clash with a team's idiomatic style (for example, the use of *early returns*).
6. **No tooling for your project's Evals.** The framework requires the AI to design and run Evals for AI components, but it ships no runner: the `evals/` folder only validates the framework's own configuration.
7. **The one-sentence start needs an assistant with shell and network access.** In ChatGPT or Gemini web you have to paste `MASTER_PROMPT.md` by hand.
8. **Language.** The master prompt and several guides are in Spanish only. The READMEs, this comparison and some templates have an English version, but the framework core does not.
9. **The phase graph is a hand-written summary, not a learned one.** In the paper that inspired it ([*Procedural Graphs*](https://arxiv.org/abs/2609.09153), Table 2), a hand-crafted expert graph without the evolution loop made results worse in one of its benchmarks. Here this is mitigated by keeping the graph small, faithful to `<workflow>` and checked by the linter, but its effect on quality has not been measured: the `evals/` suite has a single eval today.

---

## 🔍 Detailed Analysis of the Advantages of `dbv-specs-ops`

### 1. Deterministic verification (`@spec` + Linter) instead of blind trust in the prompt
In the other frameworks, consistency between requirements, tasks and code is checked by the AI agent itself (for example `/speckit.analyze` or `/speckit.converge` in Spec Kit, or BMAD's *Trace* skill). `dbv-specs-ops` introduces its **own linter in standard Python (zero external dependencies)** that deterministically verifies that every requirement (`REQ-01`, `REQ-02`...) is cited with `@spec` both in the implementation and in its automated tests, and that audits the single-`return`-per-function rule in Python. With the `--changed` flag it can be adopted in existing projects by auditing only the modified files.

### 2. Customization and intellectual property in every file (`project.config.md`)
Regardless of the programming language (Python, Rust, TypeScript, C++, etc.), `dbv-specs-ops` automatically injects standardized headers into every new file with the author, website, license and technical purpose metadata defined in `project.config.md`.

### 3. Native readiness for GEO (Generative Engine Optimization) and Agents
When a web application is built with `dbv-specs-ops`, the project is prepared out of the box so that generative answer engines and autonomous agents can discover it, read it and interact with it through `llms.txt`, `Content-Signals` directives, `.well-known` metadata and the **Agent Plugins 1.0.0 / MCP** standard.

### 4. From the Web to the Cross-Platform Desktop and Official Stores
For desktop projects, it provides a complete path based on **Rust + Tauri v2** to build native, signed installers for **Windows, macOS and Linux**, including the configuration and deployment guides for the **Microsoft Store**, a **Homebrew Tap** (`brew install`) and **Uptodown**.

### 5. "AI-Native" applications ready out of the box
If the software you are building needs to offer AI capabilities to its end users, the framework includes `docs/AI_DESKTOP_ARCHITECTURE.md` and the bilingual `AI_SPECIFICATIONS` and `IA.md` templates. It leaves the architecture ready to connect with **local AIs** (Ollama, LM Studio, local models), **CLI agents through the ACP/MCP protocol** or **Cloud APIs**, storing credentials in the operating system's native *keyring* and applying guardrails against *prompt injection*.

### 6. Adversarial Review and protection against *Slopsquatting*
Before writing code, the AI must pass an adversarial review of its own plan: it cites evidence from the repository, self-assesses its confidence on edge cases, security and concurrency, and stops to ask the developer whenever its certainty is not total. Later, in the `Simplify` phase (`docs/REVIEW.md`), it runs 3 audit passes that expressly verify that no hallucinated packages (*slopsquatting*) exist. BMAD also offers layered code review with adversarial lenses; what is specific to `dbv-specs-ops` is the confidence self-assessment before building and the check for nonexistent dependencies.

### 7. Integral Git management and closing (`/ship`)
The framework governs Git from repository creation to the `/ship` closing command, which automates moving changes into `CHANGELOG.md`, updates `walkthrough.md` and the `README.md`, proposes the version commit, generates the semantic tag (`vx.y.z`) and suggests the push, which it runs when the user asks for it.

### 8. Cross-platform start and stop scripts (`start` / `stop`)
If the project is a web server, an API or a resident service, `/ship` generates in the project root the pairs `start.cmd` / `stop.cmd` (Windows) and `start.sh` / `stop.sh` (macOS/Linux, with `chmod +x`). In Python projects they activate and deactivate the local virtual environment automatically, and the project's `README.md` documents how to use them. Native desktop apps (Tauri), libraries and CLIs with no resident process do not get artificial `stop` scripts: they document their native commands (`npm run tauri dev`, `cargo tauri build`...).


### 9. Phase graph and memory of discarded approaches (v3.1.0)
Adapted from [*Procedural Graphs*](https://arxiv.org/abs/2609.09153) (Google, 2026). Each phase command receives only its neighborhood of the graph: how to run the phase depending on where you came from, its exits with their conditions, and what the next step will review. That way it does not carry the rules of every phase at once. That context is **generated** from a single `PROCEDURAL_GRAPH.json`, and the linter fails if the graph has dead ends or if a command drifts from it. The *Rejection Memory* in `memory.md` records approaches that were tried and discarded, so another session does not repeat them. What was **not** adopted from the paper (the guidance LLM and automatic self-evolution), and why, is in [`PROCEDURAL_GRAPHS.md`](./PROCEDURAL_GRAPHS.md) *(in Spanish)*.

---

## 🤔 When to choose each one?

* **Choose [`GitHub Spec Kit`](https://github.com/github/spec-kit)** if you want a CLI-driven SDD flow with a constitution, 38 agent integrations, an ecosystem of extensions and presets, and the largest community.
* **Choose [`BMAD Method`](https://github.com/bmad-code-org/BMAD-METHOD)** if you prefer working with specialized agents (analyst, PM, architect, developer, UX) and skills for planning, building and review, with an optional test-architecture module.
* **Choose [`Kiro`](https://kiro.dev/)** if you prefer an integrated environment (IDE, CLI, web and mobile) with specs, hooks, steering and property-based tests, and you accept depending on a commercial AWS product.
* **Choose `dbv-specs-ops`** if you are looking for an **end-to-end engineering framework** with code-level traceability verified by a linter, author headers, memory across parallel sessions, GEO/Agent readiness, AI-Native or desktop apps in official stores, `start`/`stop` scripts and automation of Git and releases, and you accept that it is a young, single-author project whose rules, by default, depend on the AI following them.
