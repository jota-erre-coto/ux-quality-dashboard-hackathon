# Design Proof — Remotion Video Demo Context
## Ghost Signal · Hackathon Civitatis 2026

> Este archivo es el prompt de contexto para generar el video demo de Design Proof con Remotion.
> Formato pensado para ser leído por un agente de IA (Claude Code) que construirá la composición.
>
> Setup: `npx create-video@latest` → Blank template → Tailwind → Skills
> Luego: abrir Claude Code en el directorio y pasar este archivo como contexto.

---

## Qué es el video

Un demo de **90 segundos** (2700 frames a 30fps) que cuenta la historia de Design Proof:
el dashboard de calidad UX que Civitatis no tenía. Pensado para presentar al jurado del
hackathon al final del día 2. Tono: pitch de startup, preciso, sin piedad con el relleno.

---

## Identidad visual

```
Brand colors:
  --primary:    #EA0558   (Civitatis pink — el acento)
  --navy:       #2F3C7E   (Civitatis navy — estructural)
  --dark-bg:    #0f0f1e   (fondo hero oscuro)
  --dark-mid:   #1a0a2e
  --dark-accent:#2a0a1e
  --pink-glow:  #FF80B0   (glow sobre fondos oscuros)
  --green-data: #7DFFC0   (datos positivos en terminal)
  --purple-dim: #7C7CFF   (prompt de terminal)
  --white-85:   rgba(255,255,255,.85)
  --white-40:   rgba(255,255,255,.40)
  --white-22:   rgba(255,255,255,.22)

Typography:
  Montserrat — headlines 800/900, body 400/600
  JetBrains Mono — terminal, código, datos

Group name:   Ghost Signal
Project name: Design Proof
Tagline:      La capa de calidad UX que Brain no tiene.
```

---

## Composición principal

```typescript
// src/Root.tsx
<Composition
  id="DesignProofDemo"
  durationInFrames={2700}   // 90s × 30fps
  fps={30}
  width={1920}
  height={1080}
  component={DesignProofDemo}
/>
```

---

## Escenas — estructura de la composición

### ESCENA 1 — El problema (frames 0–270 · 9s)

**Propósito:** enganchar. La pregunta que nadie puede responder.

**Visual:**
- Fondo: `linear-gradient(135deg, #0f0f1e 0%, #1a0a2e 50%, #2a0a1e 100%)`
- Dos blobs radiales animados con `spring()` al entrar
- Texto central: aparece línea a línea con `spring()` staggered

**Contenido (texto):**
```
[línea 1, frame 0→30]   "El AOV cayó de 155 € a 135 €."
[línea 2, frame 20→60]  "CSAT bajó. NPS no sube."
[línea 3, frame 50→90]  "Nadie supo por qué."
[pausa frames 90→150]
[línea 4, frame 150→210] — fade in lento, color #FF80B0 —
                          "Faltaba la capa intermedia."
```

**Typography:**
```css
línea 1-3: Montserrat 800, clamp(52px, 5.5vw, 72px), color white, letter-spacing -0.03em
línea 4:   Montserrat 600, 32px, color #FF80B0
```

**Animación:** `spring({ frame, fps: 30, config: { damping: 14 } })` para translateY + opacity

---

### ESCENA 2 — El dato (frames 270–480 · 7s)

**Propósito:** las cifras que nadie quiere ver. Hacer visible el problema.

**Visual:**
- Misma base oscura con transición suave desde Escena 1 (`interpolate`)
- 3 tarjetas tipo "KPI glass" que aparecen en cascada

**KPI cards (liquid glass):**
```
Card 1: "0%"    → "NPS B2C · target >9,4"    → "4 MBRs consecutivos sin avance"
Card 2: "0%"    → "AOV B2C · target 142 €"   → "Actual: 135 €. Era 155 €."
Card 3: "0%"    → "NPS B2B · target >8"      → "Real: 35/100 (VoC Brain)"
```

**Estilo de cada card:**
```css
background: rgba(255,255,255,.05)
border: 1px solid rgba(255,255,255,.12)
border-radius: 18px
padding: 28px 32px
KPI value: Montserrat 900, 56px, color #FF80B0
Label:     Montserrat 600, 14px, color rgba(255,255,255,.55)
Sub:       Montserrat 400, 12px, color rgba(255,255,255,.30)
```

**Animación:**
- Card 1: entrada en frame 280 con `spring({ frame: frame - 280 })`
- Card 2: entrada en frame 330
- Card 3: entrada en frame 380
- Cada card: `translateY(40px → 0) + opacity(0 → 1)`

---

### ESCENA 3 — La oportunidad (frames 480–660 · 6s)

**Propósito:** el giro. Brain ya tiene los datos.

**Visual:**
- Transición: fondo oscuro se mantiene, aparece una card grande centrada
- Badge animado con pulse

**Contenido:**
```
[badge superior] "Brain · brain.civitatis.tech · live"
[H1 grande]      "Brain ya tiene los datos."
[subtítulo]      "97 KPIs. Sin lente de experiencia."
[pausa]
[aparece lista] — items en cascada, icono ✓ verde —
  ✓ BigQuery auto-actualizado: CR 4,12% · AOV 135 € · NPS 9,3 · CES 9,7
  ✓ Zendesk: CSAT B2C 94% · CSAT B2B 95% · tickets por canal
  ✓ VoC: 50 tags Zendesk con sentimiento (incl. #accesibilidad)
  ✓ Impact Tracker: OKRs con progreso real en tiempo real
```

**Typography:**
```css
H1:   Montserrat 800, clamp(48px, 5vw, 68px), color white
Sub:  Montserrat 600, 24px, color #FF80B0
List: Montserrat 500, 16px, color rgba(255,255,255,.70)
Check icons: color #7DFFC0
```

---

### ESCENA 4 — Design Proof · Vista 1 (frames 660–1020 · 12s)

**Propósito:** mostrar qué es. La Salud UX dashboard.

**Visual:**
- Transición a fondo claro: `background: #F8F8FC`
- Simular la Vista 1 del dashboard con datos reales

**Estructura:**
```
[frames 660→720] Título sección:
  "DESIGN PROOF" — badge — "Vista 1 · Salud UX"
  Subtítulo: "El termómetro permanente de la experiencia"

[frames 720→900] UX Health Index + Coverage aparecen:
  Index circle: "58%" — label "En riesgo" — color #D97706
  Coverage bar: "4/7 dimensiones con señal"
  Tendencia: "↓ 2 dimensiones degradadas vs Abril"

[frames 900→1020] Grid de 7 dimensiones Honeycomb aparecen en cascada:
  🎯 Útil      — 🟡 En riesgo   — Perfect Memories 70,7%
  🖱️ Usable   — 🔴 Crítico    — Abandono checkout 77%
  🔍 Findable  — 🔴 Gap         — CTR buscador: sin instrumentar
  💜 Deseable  — 🔴 Alerta      — Ratings App 2,9 · Uninstalls iOS +63,5%
  🛡️ Credible — 🟡 En riesgo   — CSAT 94% · PIX atascado
  ♿ Accesible — 🟡 Señal viva  — VoC #accesibilidad activo
  💰 Valioso   — 🟡 En riesgo   — AOV 135€ (0% OKR)
```

**Colores de semáforo:**
```
🟢 #16A34A (bg: #DCFCE7)  — Directa
🟡 #D97706 (bg: #FEF3C7)  — Proxy / En riesgo
🔴 #DC2626 (bg: #FEE2E2)  — Gap / Crítico
```

**Cada dimension card:**
```css
background: white
border-radius: 14px
border-top: 3px solid [color semáforo]
padding: 16px 18px
box-shadow: 0 1px 3px rgba(0,0,0,.06)
```

---

### ESCENA 5 — Design Proof · Vista 2 Gate 3 (frames 1020–1260 · 8s)

**Propósito:** el registro. Cada decisión de diseño documentada.

**Visual:**
- Fondo blanco se mantiene
- Simular dos entradas Gate 3 reales apareciendo

**Contenido:**
```
[frames 1020→1080] Título:
  "Vista 2 · Gate 3"
  "El registro de cada decisión de diseño"

[frames 1080→1180] Entry 1 aparece:
  Feature:    "Checkout 2 pasos v4.7.0"
  Designer:   "Solvey Prada"
  Surface:    "Web · Checkout"
  Dimensión:  "🖱️ Usable + 💰 Valioso"
  Hipótesis:  "Si reducimos el nº de pasos, el ratio PDP→compra mejorará"
  Resultado:  [antes] 13% → [después] 22% · ↑ +68% · ✅ Validado

[frames 1180→1260] Entry 2 aparece:
  Feature:    "Upsell Free Tour → Tour Privado"
  Designer:   "Squad Web"
  Surface:    "Web · PDP"
  Dimensión:  "🎯 Útil + 💰 Valioso"
  Resultado:  CR funcionalidad 21,39% · NR generado 17.357 € · ✅ Validado
```

**Estilo entry card:**
```css
background: white
border-radius: 16px
border: 1px solid #E8E8F0
padding: 20px 24px
box-shadow: 0 4px 12px rgba(0,0,0,.08)
Verdict badge: background #DCFCE7, color #16A34A, "✅ Validado"
```

---

### ESCENA 6 — La narrativa MCP (frames 1260–1620 · 12s)

**Propósito:** el momento estrella. Claude responde en el MBR.

**Visual:**
- Fondo vuelve al oscuro (`#0f0f1e`) con transición suave
- Terminal mock que escribe en tiempo real (efecto typewriter)

**Terminal header:**
```
● ● ●  "Design Proof · MCP · sesión MBR Pilar 1"
```

**Secuencia de escritura (typewriter effect):**
```
[frame 1260→1320]  prompt aparece:
  "jcoto@mbr-pilar1 ~ " (color #7C7CFF)
  "¿Por qué pudo caer el AOV en mayo?" (color white)

[frame 1320→1380]  respuesta label:
  "Design Proof · Análisis de correlación" (color #FF80B0)

[frame 1380→1440]  línea 1 aparece:
  "Dimensión Usable B2C Web ↓6 puntos vs Abril." (color #7DFFC0)

[frame 1440→1500]  líneas de features:
  "Features Gate 3 sobre Checkout en el periodo:" (color #7DFFC0)
  "  → CIVI-3164: Adaptación PDP a Cobrandings · Solvey · 1 jun" (color rgba(255,255,255,.35))
  "  → CIVI-2459: Motor Promocional en checkout 🔴 Atascada" (color rgba(255,255,255,.35))

[frame 1500→1560]  análisis aparece:
  "Señal conductual: abandono checkout ↑." (color #7DFFC0)
  "Señal actitudinal: CES estable." (color #7DFFC0)

[frame 1560→1620]  conclusión:
  "Hipótesis: fricción nueva en el paso de pago, no insatisfacción general." (color #7DFFC0)
```

**Nota implementación typewriter:**
```typescript
// Para el efecto typewriter, usar interpolate para revelar chars progresivamente:
const charsToShow = Math.floor(interpolate(frame, [startFrame, endFrame], [0, text.length]));
const visibleText = text.slice(0, charsToShow);
```

---

### ESCENA 7 — El resultado (frames 1620–1890 · 9s)

**Propósito:** volver al problema y cerrarlo. Antes vs ahora.

**Visual:**
- Split screen animado: izquierda (antes, gris) / derecha (ahora, verde)
- Aparece con `spring()` desde centro

**Left side (antes):**
```
Background: #F3F3F9
Label: "ANTES"
Text (grande, tachado): "No tenemos una idea clara de por qué."
Icono: ❓
```

**Right side (ahora):**
```
Background: #DCFCE7 (success light)
Label: "AHORA"
Text (grande): "Hipótesis accionable en 8 segundos."
Icono: ✓ (color #16A34A)
Sub: "Usable ↓ · Checkout · 2 features lanzadas · Gate 3 open"
```

**Animación:**
- Split entra desde centro: left slide left, right slide right
- Texto de cada lado con `spring()` staggered

---

### ESCENA 8 — Cierre y branding (frames 1890–2700 · 27s)

**Propósito:** identidad, call to action, créditos.

**Subesenas:**

**8a — Ghost Signal (frames 1890→2070 · 6s):**
```
Fondo: gradient dark
Centro: logotype animado

"GHOST SIGNAL"  — Montserrat 900, clamp(52px, 6vw, 80px), white
                   letter-spacing 0.08em, texto que entra letra a letra

"presenta"       — Montserrat 400, 18px, rgba(255,255,255,.40)

"Design Proof"  — Montserrat 700, 32px, color #FF80B0
                   aparece debajo con spring delay
```

**8b — La tesis (frames 2070→2340 · 9s):**
```
Fondo: var(--primary) #EA0558
Centro: card blanca con texto grande

"Civitatis tiene métricas de negocio
 muy bien instrumentadas."

"Lo que Design Proof añade es la capa
 que las convierte en diagnóstico."

Typography: Montserrat 600, clamp(20px, 2.2vw, 28px), color #0F0F1E
Strong words: font-weight 800
```

**8c — Créditos finales (frames 2340→2700 · 12s):**
```
Fondo: gradient dark (mismo que hero)

[centrado, aparece en cascada]
Logo: "civitatis" (SVG inline, fill: white, 24px)
Badge: "Hackathon 2026 · Pilar 1"

Equipo:
"Ghost Signal"
"Jota Coto · Head of UX"
"2–3 junio 2026"

GitHub repo: "github.com/jota-erre-coto/ux-quality-dashboard-hackathon"
```

---

## Transiciones entre escenas

```typescript
// Usar interpolate con easing para todas las transiciones de fondo
const bgOpacity = interpolate(
  frame,
  [sceneEnd - 15, sceneEnd],
  [1, 0],
  { extrapolateLeft: 'clamp', extrapolateRight: 'clamp' }
);

// Spring para entradas de elementos
import { spring } from 'remotion';
const entrance = spring({
  frame: frame - delayFrames,
  fps,
  config: { damping: 14, stiffness: 120 }
});
const translateY = interpolate(entrance, [0, 1], [30, 0]);
const opacity = entrance;
```

---

## Audio (opcional)

Si se añade audio, timing sugerido:
- 0–9s: silencio o tono bajo
- 9–30s: música ligera de fondo (sin melodía dominante)
- 30–72s: música continua, ligeramente más energía
- 72–90s: fade out gradual

---

## Datos reales para usar (Mayo 2026 · fuente: Brain)

```
NPS B2C:          9,3     (target: >9,4)
AOV B2C:          135 €   (era 155 €)
CSAT B2C:         94%     (Zendesk)
CSAT B2B:         95%     (Zendesk)
NPS B2B:          35/100  (Brain VoC)
CR web:           4,12%
CES global:       9,7
Uninstalls iOS:   +63,5% vs 2025
ANR rate Android: 0,52% (umbral: 0,47%)
App ratings:      2,9 (benchmark: 4,7-4,9)
NR Web Mayo:      5,61M€ (-5,4% vs LY)
NR App Mayo:      1,32M€ (+8,1% vs LY)
Perfect Memories: 70,7% rate · 3,14M en 2026
Favoritos→CR App: 0,0% (de 265.618 trips, 25 tenían favoritos)
```

---

## Setup rápido para el agente

```bash
# 1. Crear el proyecto
npx create-video@latest
# Opciones: Blank template · Tailwind: yes · Skills: yes

# 2. Entrar al directorio
cd design-proof-demo

# 3. Instalar dependencias
npm install

# 4. Iniciar preview
npm run dev

# 5. Abrir Claude Code en el mismo directorio
claude

# 6. Dar este archivo como contexto al agente:
# "Lee @remotion-demo-context.md e implementa la composición DesignProofDemo"
```

---

## Comando de render final

```bash
npx remotion render DesignProofDemo out/design-proof-demo.mp4 \
  --codec=h264 \
  --crf=18 \
  --fps=30
```

---

*Contexto generado el 3 jun 2026 · Ghost Signal · Jota Coto · jcoto@civitatis.com*
*Referencia: remotion.dev/docs/ai/coding-agents*
