---
description: Fase Test del ciclo SDD (dbv-specs-ops) — tests y evals obligatorios
---
Ejecuta ahora la fase **Test** (`/test`) del ciclo Spec→Plan→Build→Test→Simplify→Ship definido en
`dbv-specs-ops/docs/MASTER_PROMPT.md` (`<workflow>`, paso 4). Crea y ejecuta los tests que falten para el
código construido; ninguna tarea se marca como hecha sin un test que pase. Cada test que valide un
requisito con ID lo cita con `@spec REQ-NN`. Si el proyecto tiene
componentes de IA no deterministas, incluye también Evals. Registra cualquier bug corregido en
`dbv-specs-ops/CHANGELOG.md`.

$ARGUMENTS
