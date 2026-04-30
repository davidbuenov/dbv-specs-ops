# README.template.md — Project README Template

<!--
  INSTRUCCIONES PARA LA IA / AI INSTRUCTIONS:
  Este fichero es la plantilla base para generar el README.md del proyecto del usuario.
  Durante el bootstrap:
  1. Lee project.config.md para obtener nombre, descripción, autor y licencia.
  2. Genera README.md en el idioma elegido por el usuario (EN / ES / Bilingüe).
  3. Sustituye todos los placeholders con los valores reales.
  4. Genera la Tabla de Contenidos automáticamente según las secciones presentes.
  5. Borra este fichero README.template.md una vez generado el README.md.
  6. En cada /ship, actualiza README.md con los cambios de la versión.
-->

---

# [Project Name]

> [One-line description of the project]

<!-- TOC: Generate automatically based on the sections below, in the chosen language -->
## 📑 Table of Contents / Índice

- [About / Sobre el proyecto](#about)
- [Requirements / Requisitos](#requirements)
- [Installation / Instalación](#installation)
- [How to run / Cómo ejecutar](#usage)
- [How to stop / Cómo parar](#stop)
- [Project structure / Estructura del proyecto](#structure)
- [Changelog](#changelog)
- [License / Licencia](#license)

---

<a name="about"></a>
## 📌 About / Sobre el proyecto

<!-- Describe what the project does, who it's for, and what problem it solves. 2-4 sentences. -->

[Project Name] is a [type of application] that [main purpose and key benefit].

**Built with:** [Main technologies from ARCHITECTURE.md]

---

<a name="requirements"></a>
## 🧰 Requirements / Requisitos

<!-- List what the user needs installed before running the project. -->

- [e.g. Python 3.11+ / Node.js 20+ / Java 17+]
- [e.g. pip / npm / maven]
- [Any other dependency]

---

<a name="installation"></a>
## ⚙️ Installation / Instalación

<!-- Step-by-step instructions to get the project running for the first time. -->

```bash
# 1. Get the project files (if not already done)
# Option A — GitHub Template: click "Use this template" on GitHub
# Option B — Download ZIP: Code → Download ZIP

# 2. Install dependencies
# [AI: fill in the correct commands for the project's stack]
# e.g. for Python:
python -m venv venv
venv\Scripts\activate     # Windows
source venv/bin/activate  # macOS / Linux
pip install -r requirements.txt
```

---

<a name="usage"></a>
## ▶️ How to run / Cómo ejecutar

Use the startup scripts included in the project root:

**Windows:**
```cmd
start.cmd
```

**macOS / Linux:**
```bash
./start.sh
```

Then open your browser at: `http://localhost:[PORT]`

<!-- AI: fill in the correct port and any additional startup notes -->

---

<a name="stop"></a>
## ⏹ How to stop / Cómo parar

**Windows:**
```cmd
stop.cmd
```

**macOS / Linux:**
```bash
./stop.sh
```

---

<a name="structure"></a>
## 📂 Project structure / Estructura del proyecto

<!-- AI: generate based on the actual project structure created during /build -->

```
/
├── src/              # Application source code
├── tests/            # Unit and integration tests
├── docs/             # Project documentation (SDD files)
├── start.cmd         # Windows startup script
├── start.sh          # macOS/Linux startup script
├── stop.cmd          # Windows stop script
├── stop.sh           # macOS/Linux stop script
├── project.config.md # Project identity and file header config
├── CHANGELOG.md      # Version history
└── README.md         # This file
```

---

<a name="changelog"></a>
## 📋 Changelog

See [CHANGELOG.md](./CHANGELOG.md) for the full version history.

---

<a name="license"></a>
## 📄 License / Licencia

[License] — see [LICENSE](./LICENSE) for details.

Copyright (c) [Year] [Author / Company]

---

> 🛠️ Built with **[dbv-specs-ops](https://github.com/davidbuenov/dbv-specs-ops)** — the SDD framework for AI-assisted development.
> Created by [David Bueno Vallejo](https://github.com/davidbuenov) — free and open source.
