# 🚀 CI/CD Multiplataforma para Apps Nativas Compiladas (GitHub Actions)

> Patrón de referencia para compilar y publicar una app de escritorio nativa (Tauri, o similar) en
> Windows, Linux y macOS, validado contra ejecuciones reales de GitHub Actions.

## 1. Principio: no existe compilación cruzada fiable

Para apps nativas compiladas (Tauri/Electron con módulos nativos/etc.), compilar el binario de una
plataforma requiere un runner de **esa misma plataforma**. No asumir que "funciona en Windows" implica que
compilará en Linux/macOS — la única verificación real es un job de CI corriendo en `ubuntu-*` / `macos-*`
respectivamente. Un workflow por plataforma (`release-windows.yml`, `release-linux.yml`,
`release-macos.yml`) es más simple de razonar y depurar que una única matriz condicional.

## 2. Patrón "Release como borrador acumulativo"

Cuando la publicación de alguna plataforma sigue siendo manual (p. ej. Windows con firma local de
actualizador) y otras se automatizan por CI, el patrón que funciona bien es:

- Cada workflow de plataforma automatizada construye su(s) artefacto(s) y los **adjunta a un borrador**
  (`releaseDraft: true`) de GitHub Release para el tag correspondiente — si el borrador no existe, la
  acción de release lo crea; si ya existe, añade los artefactos nuevos sin tocar los existentes.
- El mantenedor completa ese mismo borrador subiendo a mano los artefactos de la plataforma no automatizada,
  y pulsa "Publish" cuando están todos.
- Soportar también `workflow_dispatch` con un input `draft` (`true`/`false`) para poder re-lanzar el
  workflow **después** de que la Release ya esté publicada (p. ej. añadir macOS más tarde a una versión que
  ya salió solo con Windows+Linux) — la acción de release típica **falla** si le pides `draft: true` y solo
  existe ya una Release publicada con ese tag (no la encuentra, no la toca), así que hay que poder pedir
  explícitamente `draft: false` para ese caso.

```yaml
on:
  push:
    tags: ["v*.*.*"]
  workflow_dispatch:
    inputs:
      draft:
        description: >
          "true" (normal): crea/usa un borrador para esa versión. "false": la Release de esa
          versión ya está PUBLICADA y solo quieres añadirle artefactos de esta plataforma.
        required: false
        default: "true"
        type: choice
        options: ["true", "false"]
```

## 3. Leer la versión del fichero de configuración, no de `github.ref_name`

Leer la versión desde el propio fichero de configuración del proyecto (p. ej. `tauri.conf.json`) en vez de
derivarla del tag que disparó el workflow permite relanzar el workflow manualmente sobre la rama principal
(vía `workflow_dispatch`) para adjuntar artefactos de una plataforma a una Release cuyo tag ya existe, sin
depender de empujar un tag nuevo:

```yaml
- name: Leer versión del fichero de configuración
  id: version
  run: echo "tag=v$(node -p "require('./ruta/al/config.json').version")" >> "$GITHUB_OUTPUT"
```

## 4. Permisos de `GITHUB_TOKEN`: conceder por workflow, no globalmente

El `GITHUB_TOKEN` por defecto de un repo suele ser de **solo lectura** (Settings → Actions → Workflow
permissions). Sin escritura explícita, cualquier acción que suba artefactos a una Release falla con
`Resource not accessible by integration`. Conceder el permiso **solo al workflow que lo necesita**, no como
valor por defecto de todo el repositorio (menor privilegio):

```yaml
jobs:
  build:
    permissions:
      contents: write
```

## 5. Gotcha real — runners macOS son Apple Silicon por defecto

Desde que GitHub cambió `macos-latest` a runners Apple Silicon, compilar **sin especificar target** produce
un binario de una sola arquitectura (`aarch64`), no un binario universal. Un Mac Intel no puede ejecutarlo
(Rosetta traduce x86_64→Apple Silicon, no al revés). Si se quiere dar soporte a Mac Intel, hay que pedir
explícitamente el target universal e instalar ambos targets de Rust antes del build:

```yaml
- uses: dtolnay/rust-toolchain@stable
  with:
    targets: "aarch64-apple-darwin,x86_64-apple-darwin"
# ...
- uses: tauri-apps/tauri-action@v0
  with:
    args: --target universal-apple-darwin
```

**Lección general:** al añadir un job de CI nuevo para una plataforma, no asumir que "sin especificar
arquitectura/target" produce el build más compatible por defecto — verificarlo explícitamente contra la
documentación actual del runner, que cambia con el tiempo.

## 6. Gotcha real — artefactos de auto-actualización rompen el build si faltan las claves

Si el framework de empaquetado soporta generar artefactos firmados para auto-actualización (`.sig`,
manifiesto de versión) pero esa plataforma **no** tiene todavía las variables de entorno de firma
configuradas en CI, el build completo puede fallar (exit 1) en vez de simplemente omitir ese paso — aunque
el propio instalador/paquete se genere bien. Hay que desactivar explícitamente la generación de artefactos
de actualización para esa plataforma hasta que se resuelva la firma cross-máquina (ver §7):

```json
// tauri.<platform>.conf.json de la plataforma sin firma todavía
{ "bundle": { "createUpdaterArtifacts": false } }
```

```yaml
- uses: tauri-apps/tauri-action@v0
  with:
    includeUpdaterJson: false
```

## 7. Deuda técnica aceptable: firma cross-máquina no resuelta

Si el par de claves de firma del actualizador se usa hoy solo en la máquina local donde se firma el build
de una plataforma (p. ej. Windows), fusionar en un único manifiesto de actualización (`latest.json`) una
firma generada en CI (otra plataforma) con otra generada en local introduce una coordinación cross-máquina
real. Es preferible **documentarlo explícitamente como deuda técnica consciente** (esa plataforma queda sin
auto-actualización hasta resolverlo) que improvisar una coordinación frágil bajo presión de tiempo.

## 8. Builds sin firmar como estrategia intermedia legítima

Publicar un binario sin firma de código ni notarización (coste real: cuenta de desarrollador de pago +
verificación de identidad recurrente) es una decisión de producto válida cuando ese coste no está
justificado por el volumen de usuarios — no es un atajo vergonzoso, es una decisión consciente. Lo que sí es
obligatorio: documentar para el usuario final cómo abrir un binario sin firmar pese al aviso del sistema
operativo (SmartScreen en Windows: "Más información" → "Ejecutar de todas formas"; Gatekeeper en macOS: clic
derecho → Abrir, o `xattr -cr` sobre el `.app`).

Para el checklist de qué exige cada tienda de apps en materia de firma/certificación, ver
[`MARKETPLACE_PUBLISHING.md`](./MARKETPLACE_PUBLISHING.md).
