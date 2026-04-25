# CLAUDE.md — Neural Global Landing Page

> Contexto persistente para asistentes de IA trabajando en este proyecto.
> Última actualización: abril 2026

---

## 1. El cliente y el proyecto

**Neural Global** es la práctica clínica privada del **Dr. Antonello**, sexólogo clínico colegiado en CPPCR (Costa Rica). Sede en **Escazú, San José**, con atención presencial y online.

**Objetivo del sitio:** captar consultantes adultos que buscan un espacio profesional, discreto y libre de prejuicio para consultas de sexología y salud mental. La landing es una sola página (one-pager) — no es un sitio multipágina ni un blog.

**Audiencia:**
- Adultos profesionales (25–55) que valoran discreción y rigor clínico.
- Pacientes en Costa Rica (presencial Escazú) y costarricenses/latinos en el extranjero (online).
- Probable consulta de pareja además de individual.

**Tono editorial:**
- Calmo, contemplativo, clínico pero humano.
- **Sin** lenguaje publicitario agresivo, sin emojis, sin "transforma tu vida".
- Frases cortas, blanco entre ideas, lectura pausada.
- Confidencialidad explícita pero sin parecer defensivo.

---

## 2. Referencia de diseño: Colnago

El cliente eligió como referencia visual la página de Colnago (fabricante italiano de bicicletas de alta gama). El **inspiration moodboard** está en `uploads/screencapture-colnago-it-it-2026-04-23-13_30_09.pdf`.

**Lo que tomamos de Colnago:**
- **Editorial, no comercial:** layouts tipo revista, hairlines finas, márgenes generosos.
- **Hero full-bleed con video atmosférico atenuado** + headline serif italic gigante encima.
- **Mono ALL CAPS** para etiquetas, números de figura, "Fig. 01 / …".
- **Paleta restringida:** un dark slate-teal, un cream cálido, un sand/tan acento. Sin gradientes saturados.
- **Numeración de secciones** ("01 — Servicios", "02 — Manifesto") para dar ritmo de catálogo.
- **Density:** mucho aire, copy disciplinado, jerarquía tipográfica clara.

**Lo que NO replicamos de Colnago:**
- Carruseles agresivos de productos.
- Cualquier elemento "ecommerce".
- Fotografía de producto pura — acá la fotografía es ambiental/humana.

---

## 3. Sistema de diseño implementado

### Paleta (variables CSS en `Landing Page.html`)
```css
--dark:    #1C2B2D    /* slate-teal — bg hero, footer, manifesto */
--cream:   #F5F0E8    /* cream cálido — bg principal */
--sand:    #C8B89A    /* tan/sand — acentos, eyebrows, líneas */
--ink:     #1C1A15    /* casi-negro cálido — texto principal */
--line:    rgba(28, 26, 21, .12)
```

### Tipografía (Google Fonts)
- **Display:** `Fraunces` (serif moderna, opsz 144) — headlines, h-mega, marca.
- **Serif italic:** `Cormorant Garamond` light italic — citas, palabras-acento ("*para*", "*despacio*").
- **Sans:** `Inter` — body, UI, navegación.
- **Mono:** `JetBrains Mono` — eyebrows, números de figura, "Fig. 01", labels técnicos.

> Nota: Inter está en la lista de "fuentes sobreusadas" del briefing interno. Si más adelante el cliente quiere algo más distintivo, considerar Söhne, Söhne Mono, Neue Haas Grotesk Display, o GT America. Por ahora Inter funciona y es gratis.

### Componentes / patrones recurrentes
- **`.eyebrow`** — mono 10–11px, ALL CAPS, letterspacing .18em, color sand. Marca cada sección.
- **`.h-mega`** — display ~120px, line-height .9, mezcla romano + italic.
- **`.btn-solid-cream` / `.btn-ghost-cream`** — pill outline en hero (sobre dark).
- **`.btn-solid-dark` / `.btn-ghost-dark`** — pill solid en cream sections.
- **Hairlines** — `1px solid var(--line)` para dividir secciones, nunca shadows pesadas.
- **Numeración:** `01 —`, `02 —`, etc. en mono al inicio de cada sección.

### Layouts
- Hero: full-bleed, video bg, copy en grid 3-col footer (Dr. Antonello / Disponibilidad / CTAs).
- Feature strip: split 1.15fr / 1fr — imagen del Dr. izq, copy der.
- Servicios: grid 3-col con tarjetas hairline.
- Pricing: 2-col, sin badges "popular".
- Process: timeline horizontal con números mono grandes.
- Footer: grid editorial, créditos discretos.

---

## 4. Estructura de archivos

```
/
├── CLAUDE.md                      ← este archivo
├── Landing Page.html              ← landing principal (única página de producción)
├── video-preview.html             ← scratch: comparación de candidatos de video (descartable)
├── images/
│   └── dr-antonello.jpg           ← retrato formal del Dr. (usado como poster del video y en feature strip)
└── uploads/
    ├── video-3-compressed.mp4     ← video del hero (0.83 MB, 4K trimmed) — referenciado desde el HTML
    ├── video-1.mp4                ← candidatos descartados (mantener temporalmente)
    ├── video-2.mp4
    ├── video-4.mp4
    ├── DS3-2.jpg                  ← moodboard / referencia
    └── screencapture-colnago-it-...pdf  ← referencia Colnago
```

### ⚠️ Notas de archivos
- **El video del hero está en `uploads/`, no en `images/`.** Razón: el serve layer del entorno de preview rechazaba archivos `.mp4` en `images/` por algún caché/regla. En producción (Vercel/Netlify) esto no será problema — recomendar mover a `images/hero-bg.mp4` y actualizar el `<source src="…">` antes de deployar.
- Los videos descartados (`video-1, -2, -4`) pueden borrarse cuando el cliente confirme.
- `video-preview.html` es scratch interno y puede borrarse antes de deployar.

---

## 5. Decisiones tomadas (no revertir sin discutir)

1. **Hero = video full-bleed + copy encima**, NO split con foto del Dr. al lado. El cliente comparó tres opciones (A: full-bleed video / B: video reemplaza retrato split / C: video atmosférico + foto del Dr. movida a feature strip) y eligió **C**.
2. **El retrato del Dr.** vive ahora en el **feature strip** (sección "Un lugar diseñado para hablar despacio") como su presentación formal con label "Fig. 01 / Dr. Antonello · Sexólogo clínico · Colegiado CPPCR".
3. **Sin emojis**, sin íconos decorativos. Si hace falta un ícono (confidencialidad, etc), usar SVG line-art muy fino, no íconos rellenos ni de Material/Heroicons.
4. **Manifesto firmado por "Dr. Antonello, Neural Global"** (no "Equipo clínico").
5. **Tratamiento del video:** `filter: grayscale(.25) contrast(1.05) brightness(.7)` + dos overlays slate-teal (uno radial top→bottom, otro lateral) para legibilidad del headline.

---

## 6. Estado actual

✅ Hero (con video)
✅ Manifesto / introducción
✅ Feature strip (retrato Dr.)
✅ Servicios
✅ Pricing
✅ Proceso
✅ Trust / pilares
✅ CTA final
✅ Footer

🔲 **Pendiente para producción:**
- Comprimir video a ~3 MB y mover a `images/hero-bg.mp4` con nombre limpio.
- Reemplazar `dr-antonello.jpg` placeholder con foto profesional real cuando el cliente la entregue.
- Confirmar copy legal (CPPCR número de colegiado, dirección exacta).
- Implementar formulario real del CTA "Agendar cita" (hoy es link `#agendar` muerto).
- Conectar WhatsApp con número real (`https://wa.me/506XXXXXXXX`).
- Meta tags OG / favicon / título SEO.
- Política de privacidad / aviso legal.

---

## 7. Reglas de trabajo para asistentes

- **Antes de agregar contenido** (secciones, copy, features), preguntar al usuario. La landing es deliberadamente concisa — no inflarla.
- **Antes de cambiar la paleta o tipografías**, preguntar. El sistema actual está validado con el cliente.
- **No introducir** gradientes saturados, sombras pesadas, animaciones decorativas, emojis, o íconos rellenos.
- **Sí está permitido** y bienvenido: refinar microtipografía, mejorar jerarquía, agregar hairlines/divisores que aporten ritmo editorial, micro-interacciones sutiles (hover de 200ms, fade-ins discretos al scroll).
- **Idioma del copy:** español rioplatense/neutro profesional. NO usar "checate", "wow", "increíble". Sí: "acompañar", "consultante", "proceso", "espacio".
- **Cuando dudes entre dos opciones**, presentá ambas como tweaks/variantes en lugar de elegir por el cliente.

---

## 8. Briefing original del cliente (resumen)

- Práctica privada, no clínica institucional.
- Quiere transmitir: profesionalismo, calidez, discreción.
- NO quiere transmitir: "terapia barata", "self-help", "wellness genérico".
- Le gustó Colnago porque "se siente como leer una revista buena, no como una página web vendiendo algo".
- Va a publicar en Vercel con dominio propio (probable: `neuralglobal.cr`).
- Trabajará el código en VS Code después de esta entrega.
