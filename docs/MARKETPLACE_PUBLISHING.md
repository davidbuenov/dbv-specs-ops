# 🏬 Publicación en Marketplaces de Apps — Guía General

> Checklist y comparativa de canales de distribución para apps nativas, validado contra una publicación
> real en Microsoft Store (aprobada, con un rechazo real de por medio) y un envío real a Uptodown.

## 1. Comparativa de canales de distribución

| Canal | Ejemplo | Firma de código | Coste | Revisión | Actualizaciones |
|---|---|---|---|---|---|
| **Self-hosted** | GitHub Releases | Opcional (recomendable con clave propia tipo minisign) | Gratis | Ninguna, publicas cuando quieras | Gestionadas por ti (p. ej. `tauri-plugin-updater`) |
| **Tienda curada con auto-firma** | Microsoft Store (vía MSIX) | **La tienda firma el paquete automáticamente** tras certificación — no hace falta comprar certificado | Gratis (cuenta de desarrollador individual) | Automatizada + manual, días | Gestionadas por la tienda/SO |
| **Tienda curada con firma propia** | Mac App Store, listado "EXE/MSI" de Microsoft Store | Certificado propio obligatorio (Apple Developer 99$/año, o Authenticode de una CA del Trusted Root Program) | Recurrente | Manual, revisión estricta | Gestionadas por la tienda |
| **Catálogo de terceros** | Uptodown y similares | Normalmente **no exige firma de plataforma** | Gratis | Editorial, manual, sin plazo garantizado | No integradas — el usuario reinstala la nueva versión |

**Decisión clave a no dar por hecha:** antes de asumir que hace falta comprar un certificado de firma de
código, comprobar si la tienda ofrece una vía de **auto-firma tras certificación** (como el MSIX de
Microsoft Store) — puede eliminar el bloqueo más caro y lento del proceso a cambio de un empaquetado
adicional, que sí es automatizable con código.

## 2. Antes de adoptar una herramienta de empaquetado de terceros

Si el framework de la app no genera nativamente el formato que exige la tienda (p. ej. Tauri no genera MSIX
de fábrica), y hace falta una herramienta de terceros no oficial, auditarla antes de instalarla:

- [ ] Licencia compatible (MIT/Apache/etc.), sin cláusulas restrictivas.
- [ ] Señal de adopción real (miles de descargas/mes, no un paquete recién publicado con 3 estrellas).
- [ ] Cadena de suministro: publicada vía CI con *trusted publishing*/OIDC (sin token npm manual filtrable)
      es una señal fuerte de buena higiene.
- [ ] Sin issues de seguridad abiertos relevantes en su repositorio.
- [ ] Verificar el flag/parámetro correcto para invocar el CLI subyacente del framework si el proyecto no
      usa la extensión "estándar" (p. ej. si Tauri se invoca vía `@tauri-apps/cli`/npm en vez de la
      extensión `cargo-tauri`, la herramienta puede necesitar un flag `--runner npm` explícito o fallar
      buscando un comando que no existe en el proyecto).

## 3. Gotcha real — assets generados automáticamente pueden ser placeholders silenciosos

Si una herramienta de empaquetado genera automáticamente los iconos/tiles/assets de marketing requeridos
por una tienda (componiendo el icono real sobre un lienzo de un tamaño específico), esa composición puede
fallar silenciosamente para *algunos* tamaños y caer a una imagen de repuesto (p. ej. un rectángulo de color
sólido) sin lanzar ningún error visible durante el build. Ese placeholder puede pasar desapercibido durante
meses hasta que la propia tienda lo rechaza en certificación citando una política concreta.

**Checklist obligatorio antes de cada envío/reenvío a certificación:**

- [ ] Abrir visualmente **cada** asset generado automáticamente (iconos en todos los tamaños exigidos), no
      solo el icono principal.
- [ ] Si hay muchos assets, verificar por script que ninguno sea de un único color sólido (heurística barata
      de "esto es un placeholder", no una prueba exhaustiva pero sí una red de seguridad rápida).
- [ ] Repetir esta verificación en **cada** reenvío tras un rechazo, no solo la primera vez — el bug puede
      haberse introducido en cualquier sesión de pulido visual anterior, no necesariamente en la más reciente.

## 4. Los formularios de cada tienda no son intercambiables

No asumir que el texto/ficha ya redactado para una tienda encaja en el formulario de otra, aunque el
producto sea el mismo. Verificar contra la documentación oficial de cada tienda antes de rellenar:

- Límites de longitud de campos (p. ej. una descripción corta puede ser de 70 caracteres en un catálogo y de
  270 en otro).
- Campos exigidos por una tienda que la otra no tiene (licencia, web oficial, nacionalidad del desarrollador,
  etc.).
- Formatos de artefacto aceptados por plataforma (p. ej. un catálogo puede exigir `.dmg` y rechazar
  `.app.tar.gz` para macOS).
- **Cobertura real de plataformas del catálogo** — no asumir que un catálogo genérico "de apps" acepta todas
  las plataformas que tu proyecto compila; verificarlo leyendo su ayuda oficial (un catálogo real consultado
  en este proyecto solo admite Android/Windows/macOS, sin Linux, pese a distribuir binarios Linux por otros
  canales).

## 5. Coexistencia de canales de distribución

Cuando el mismo producto se distribuye simultáneamente self-hosted y en una tienda curada:

- Ambos canales pueden coexistir sin conflicto técnico si tienen **identidad de paquete distinta** (p. ej.
  un instalador NSIS y un MSIX son instalaciones independientes en el sistema del usuario).
- Documentar explícitamente, de cara al usuario final, qué mecanismo de actualización usa cada canal — un
  usuario que instale desde la tienda no debería ver ni depender del botón de actualización manual del canal
  self-hosted (ver lección 6 en [`NATIVE_DESKTOP_APPS.md`](./NATIVE_DESKTOP_APPS.md)).
- Si se hace un rebrand o cambio de identificador de producto entre versiones, evaluar explícitamente el
  riesgo de que los usuarios existentes vean una instalación **nueva y paralela** en vez de una actualización
  in-place — es un riesgo real y ya observado, no hipotético.

## 6. Checklist de envío (genérico, adaptar por tienda)

1. Reservar/verificar el nombre e identidad del producto en la consola de la tienda.
2. Copiar la identidad real (publisher ID, identificador de paquete) a la configuración de empaquetado del
   proyecto — verificar que el manifiesto generado coincide **exactamente** con lo mostrado en la consola de
   la tienda antes de enviar.
3. Regenerar el paquete final con esa identidad.
4. Publicar/enlazar una política de privacidad si la tienda la exige (casi todas la exigen si la app accede
   a datos de cualquier forma, aunque sea solo a petición explícita del usuario).
5. Rellenar la ficha de producto (descripción, capturas, categoría, edad recomendada) **con los límites
   reales de esa tienda**, no reutilizando sin revisar el texto de otra.
6. Verificar todos los assets generados automáticamente (§3).
7. (Recomendado) Pasar cualquier kit de certificación local que ofrezca la tienda antes de enviar.
8. Enviar a certificación. Si es rechazado, leer la política citada literalmente antes de asumir la causa —
   la causa raíz puede no ser obvia desde la descripción del rechazo (en este proyecto, "tile con imagen por
   defecto" resultó ser un fallo silencioso de generación de assets, no una omisión de subida).
