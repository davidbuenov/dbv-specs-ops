# 🎨 Sistema de Diseño: [Nombre del Proyecto]

> **Fase:** `/spec` (Especificación Visual)
> **Estado:** Borrador / Validado
> **Última Revisión:** [Fecha]
> **Aplica a:** Proyectos con interfaz de usuario (web, móvil, desktop). Opcional para proyectos sin UI.

---

> 📐 Inspirado en el estándar **[design.md](https://github.com/google-labs-code/design.md)** de Google Labs — un formato abierto para describir identidades visuales a agentes de codificación.

---

```yaml
# ────────────────────────────────────────────────
# DESIGN TOKENS — Legibles por la IA y por máquina
# ────────────────────────────────────────────────
version: alpha
name: "[Nombre del Proyecto]"
description: "[Breve descripción del estilo visual. Ej: Minimalismo editorial con acento en la claridad y el contraste.]"

# COLORES
# Usa códigos HEX. El campo "on-X" es el color de texto que va sobre el color "X".
colors:
  primary:      "#[hex]"   # Color principal de marca
  secondary:    "#[hex]"   # Color secundario / complementario
  accent:       "#[hex]"   # Color de llamada a la acción (CTAs, highlights)
  neutral:      "#[hex]"   # Fondo base neutro
  surface:      "#[hex]"   # Fondo de tarjetas y contenedores
  on-primary:   "#[hex]"   # Texto sobre "primary" (suele ser blanco o muy claro)
  on-surface:   "#[hex]"   # Texto principal sobre "surface"
  on-neutral:   "#[hex]"   # Texto secundario / mutado
  error:        "#[hex]"   # Rojo para errores y alertas destructivas
  success:      "#[hex]"   # Verde para confirmaciones y éxito
  warning:      "#[hex]"   # Amarillo/naranja para advertencias

# MODO OSCURO (Dark Mode) — Opcional pero recomendado
# Define los overrides de color. Los tokens no listados aquí heredan los valores de luz.
dark:
  primary:      "#[hex]"
  secondary:    "#[hex]"
  accent:       "#[hex]"
  neutral:      "#[hex]"   # Ej: "#111111" — fondo oscuro principal
  surface:      "#[hex]"   # Ej: "#1E1E1E" — tarjetas sobre fondo oscuro
  on-primary:   "#[hex]"
  on-surface:   "#[hex]"
  on-neutral:   "#[hex]"

# TIPOGRAFÍA
# fontFamily: nombre exacto de Google Fonts o fuente del sistema.
typography:
  heading:
    fontFamily: "[Ej: Inter]"
    fontSize:   2rem
    fontWeight: 700
    lineHeight: 1.2
    letterSpacing: "-0.02em"
  subheading:
    fontFamily: "[Ej: Inter]"
    fontSize:   1.25rem
    fontWeight: 600
    lineHeight: 1.3
  body:
    fontFamily: "[Ej: Inter]"
    fontSize:   1rem
    fontWeight: 400
    lineHeight: 1.6
  label:
    fontFamily: "[Ej: Inter]"
    fontSize:   0.875rem
    fontWeight: 500
    letterSpacing: "0.01em"
  caption:
    fontFamily: "[Ej: Inter]"
    fontSize:   0.75rem
    fontWeight: 400

# BORDES Y RADIOS
rounded:
  none: 0px
  sm:   4px
  md:   8px
  lg:   16px
  xl:   24px
  full: 9999px

# ESPACIADO (escala base 4px)
spacing:
  xs:  4px
  sm:  8px
  md:  16px
  lg:  24px
  xl:  48px
  xxl: 96px

# COMPONENTES — Mapeo de tokens a elementos de UI concretos
# Usa referencias {ruta.al.token} para garantizar coherencia.
components:
  button-primary:
    backgroundColor: "{colors.accent}"
    textColor:       "{colors.on-primary}"
    typography:      "{typography.label}"
    rounded:         "{rounded.md}"
    padding:         "12px 24px"
  button-primary-hover:
    backgroundColor: "{colors.primary}"
  button-secondary:
    backgroundColor: "transparent"
    textColor:       "{colors.accent}"
    rounded:         "{rounded.md}"
    padding:         "12px 24px"
    border:          "1.5px solid {colors.accent}"
  card:
    backgroundColor: "{colors.surface}"
    rounded:         "{rounded.lg}"
    padding:         "{spacing.lg}"
  input:
    backgroundColor: "{colors.surface}"
    textColor:       "{colors.on-surface}"
    rounded:         "{rounded.md}"
    padding:         "10px 14px"
    border:          "1px solid {colors.on-neutral}"
  input-focus:
    border:          "2px solid {colors.accent}"
```

---

## Visión General

*[2-3 frases que describan la filosofía visual del proyecto. ¿Qué emoción o sensación debe evocar la interfaz? ¿Qué referentes de diseño lo inspiran?]*

> Ejemplo: *"La interfaz combina minimalismo editorial con toques de calidez. Cada elemento comunica claridad y confianza, evocando las mejores herramientas SaaS profesionales. El usuario debe sentir que está usando algo construido para durar."*

---

## 🎨 Colores

*Explica el rol de cada color y cómo debe (y no debe) usarse.*

- **Primary (`[hex]`):** [Ej: Azul profundo. Color de marca principal. Úsalo en headers y elementos clave de navegación. Nunca en fondos de página completa.]
- **Secondary (`[hex]`):** [Ej: Gris azulado. Para elementos de soporte, bordes y estados secundarios.]
- **Accent (`[hex]`):** [Ej: Coral. El único motor de interacción — todos los CTAs, enlaces y elementos activos usan este color. Resérvalo para acciones, no decoración.]
- **Neutral (`[hex]`):** [Ej: Blanco cálido. Base de todas las páginas. Más suave que el blanco puro para reducir la fatiga visual.]
- **Surface (`[hex]`):** [Ej: Gris muy claro. Para tarjetas, modales y paneles que elevan el contenido del fondo.]
- **Error / Success / Warning:** [Semánticos. Solo para feedback del sistema, nunca como decoración.]

### Modo Oscuro
*[Describe la estrategia del modo oscuro. ¿Es una inversión completa, una paleta propia, o solo algunos colores cambian?]*

- **Estrategia:** [Ej: "Los fondos usan grises profundos (no negro puro) para evitar halos. Los colores de marca se mantienen, pero con mayor luminosidad para cumplir contraste WCAG AA sobre fondos oscuros."]

---

## ✍️ Tipografía

*Justifica la elección de fuentes y explica la escala.*

- **Fuente principal:** [Ej: `Inter` — Elegida por su excelente legibilidad en pantalla a todos los tamaños y su amplia familia de pesos.]
- **Fuente alternativa:** [Ej: `System UI` como fallback para carga instantánea sin layout shift.]
- **Escala:** La escala tipográfica sigue una progresión modular de 1.25. Los headings usan tracking negativo (-0.02em) para aspecto más premium. El body usa lineHeight 1.6 para máxima legibilidad.
- **No usar:** [Ej: Nunca mezclar más de 2 familias tipográficas. Nunca usar `font-weight: 300` en textos menores de 16px sobre fondo oscuro.]

---

## 🧩 Componentes Clave

*Describe las decisiones de diseño de los componentes más importantes.*

### Botones
- **Primary:** [Ej: "Fondo accent, texto blanco, radio md. Único por pantalla. Es el grito — solo uno debe gritar."]
- **Secondary:** [Ej: "Contorno transparente con borde accent. Para acciones importantes pero no primarias."]
- **Ghost / Text:** [Ej: "Sin borde ni fondo. Para acciones terciarias o en contextos densos de información."]

### Tarjetas (Cards)
- [Ej: "Surface background, radio lg, sombra suave (`box-shadow: 0 2px 8px rgba(0,0,0,0.08)`). Sin borde. El espacio interno mínimo es spacing.lg (24px)."]

### Formularios
- [Ej: "Inputs con borde de 1px neutral. En focus, el borde aumenta a 2px y cambia a accent. Nunca uses fondos coloreados en campos de formulario."]

---

## ✨ Movimiento e Interacción

*Define la "física" de la interfaz: velocidades, curvas de animación y principios.*

- **Duración base:** `200ms` para micro-interacciones (hover, focus). `350ms` para transiciones de página o apertura de modales.
- **Easing:** `cubic-bezier(0.4, 0, 0.2, 1)` (Material Design Standard) para la mayoría de animaciones. `ease-out` para elementos que "caen" en pantalla. `ease-in` para elementos que "salen".
- **Principio:** [Ej: "Las animaciones son funcionales, no decorativas. Si eliminar una animación no reduce la comprensión, elimínala."]
- **Reducción de movimiento:** Siempre respeta `prefers-reduced-motion`. Sustituye transiciones por cambios instantáneos o fundidos muy cortos (50ms).

---

**Instrucción para la IA:** Lee y respeta los tokens y decisiones definidos en este fichero. Si necesitas crear un componente no definido aquí, extrapola coherentemente desde los tokens existentes y registra la nueva decisión como "Decisión de Diseño" en este mismo archivo con fecha y justificación. Para proyectos sin UI, este fichero puede omitirse.
