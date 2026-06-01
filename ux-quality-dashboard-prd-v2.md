# PRD — UX Quality Dashboard v2

| Campo | Valor |
|---|---|
| **Producto** | UX Quality Dashboard — la lente de experiencia sobre Brain |
| **Owner** | Jota Coto (Head of UX, Civitatis) |
| **Estado** | In Discovery — Hackathon Proposal (jun 2026) |
| **Gate de referencia** | Gate 3 — Post-launch (Modelo Operativo v0.7) |
| **Relación con el modelo operativo** | Resuelve los blockers H0-2 ("PD sin Gate") y H1-1 ("Gate de Calidad UX") del PD Model v0.7 |

---

## 0. Índice de artefactos del directorio

> Todos los archivos en `~/Documents/Claude/Projects/Hackathon/Dashboard-UX/`

| Archivo | Para qué |
|---|---|
| `ux-quality-dashboard-prd-v2.md` | **Este archivo** — PRD completo |
| `ux-quality-dashboard-model-v2.md` | Modelo de medición: clasificador, Health Index, KPIs, JSON contrato, plan 48h |
| `ux-quality-dashboard-knowledge-export-v2.md` | Continuidad de sesión — todo el conocimiento generado |
| `ux-quality-dashboard.html` | Dashboard funcional v0.3 — para abrir en navegador |
| `brain-data-map.md` | Mapa operativo: dónde está cada dato, quién lo tiene, cómo extraerlo |
| `brain-data-map-viewer.html` | Viewer HTML interactivo del brain-data-map — filtrable |
| `hackathon-proposal-v2.md` | Propuesta completa en markdown |
| `hackathon-proposal.html` | One-pager visual para presentar en el hackathon |
| `hackathon-brain-panel.md` | Versión texto para pegar en el panel de retos de Brain |

---

## 1. Problema

Cuando un diseñador pregunta si lo que ha diseñado "¿mejoró la experiencia?", nadie lo sabe
con certeza: los datos están fragmentados (GA4, Zendesk, Clarity, Adjust, Tableau) y nadie los
conecta a la decisión de diseño que los provocó, o directamente no existen (cero métricas de
accesibilidad). Todo lo que se lanza se evalúa con métricas de **negocio** (CR, AOV, NR, NPS,
CSAT) como promedios globales, sin desglose por feature, surface ni momento del journey, y sin
ninguna capa de **evidencia de experiencia**.

El ejemplo canónico: en el MBR de Marzo el AOV cayó de 155€ a 137,7€ y la respuesta fue *"no
tenemos una idea clara de por qué"*. No falta dato de negocio — falta la capa intermedia que
vincula `feature lanzada → diseñador → dimensión UX intervenida → métrica antes/después`.

**Civitatis mide el negocio con excelencia. Nadie mide la experiencia con la misma granularidad.**

---

## 2. Insight que reordena el proyecto: Brain ya es la capa de datos

Durante el discovery (1 jun 2026, acceso directo a `brain.civitatis.tech`) se confirmó que
**Brain ya integra las fuentes de datos** que este dashboard necesitaba conectar:

| Capacidad de Brain (ya existe) | Qué expone | Implicación para el dashboard |
|---|---|---|
| **Executive · Conectado a BigQuery** (auto-actualizado) | CR B2C 4,12 % · AOV 135€ · NR 1,03M€ · NPS 9,3 · CES 9,7 · Usuarios 58.980 · Sesiones Web 1,64M / App 335K | Toda la columna **Negocio** nace 🟢 *live* sin ETL propio |
| **Product Surface → Website / App / API** | Deep dives: Sales & Bookings, Devices, **User Behaviour**, Typology & Hubs | Datos a nivel de **surface**, que es nuestra jerarquía |
| **Customer Service Stats ↔ Zendesk** | CSAT desglosado B2C / B2B / OPS + volumen de tickets + tendencia | CSAT por canal, no global |
| **VoC · Customer Feedback Explorer** | Tickets clusterizados por **tema estratégico** con **sentimiento**, filtrable por mes. Tags incl. `#accesibilidad`, `#dudas_proceso_de_reserva`, `#consulta_precios`, `#idioma`, `#punto_de_encuentro` | Capa cualitativa por dimensión Honeycomb, viva |

**Consecuencia estratégica:** el UX Quality Dashboard **no construye un ETL nuevo** (eso es el
proyecto de Growth paralelo, que integra Adjust/Stores/Braze/Admin para KPIs de adquisición).
Este dashboard **lee de Brain** (BigQuery + Zendesk + VoC) y **añade la capa que Brain no tiene**:

1. El **mapeo Honeycomb+HEART** de cada métrica a una dimensión de experiencia.
2. Las **señales de experiencia que faltan** (eventos Findable, a11y técnica, checkout por paso).
3. El **registro Gate 3** que vincula decisión de diseño ↔ impacto medido.

> El dashboard es **la lente UX sobre Brain**, no un Brain paralelo.

---

## 3. Objetivos

**Primario:** capa de evidencia UX gestionada por el chapter que conecta cada decisión de diseño
con su impacto medido, sin depender de informes ad-hoc de Data.

**Secundarios:**
- Integrar la evidencia UX en Gate 3 (cierra H0-2 y H1-1 del PD Model v0.7).
- Termómetro permanente de salud de experiencia por dimensión Honeycomb, con cadencia MBR.
- Dar a las sesiones de Pilar 1 la capa intermedia que explica movimientos de NPS/CSAT/AOV/CR
  desde decisiones de diseño.

**Métricas de éxito del propio dashboard:**
- Q3 2026: 100 % de features con impacto UX tienen entrada Gate 3 con antes/después.
- Vista Salud UX se consulta (no se mantiene a mano) con datos vivos de Brain.
- Usado como fuente en ≥1 Gate 3 y ≥1 MBR de Pilar 1 por PI.

---

## 4. Jerarquía del producto

```
Segmento (B2C Web · B2C App · B2B)
  └── Superficie de producto (filtro: Home · Buscador · PLP · PDP · Checkout · Post-venta …)
        └── 7 dimensiones Honeycomb (siempre la espina dorsal)
              └── KPIs (mín. 1 Negocio + mín. 2 señales de Experiencia)
```

La **superficie es un filtro**, no un nivel rígido: KPIs globales del segmento (NPS, CSAT, AOV)
llevan `surface: global` y se ven siempre; el resto se filtra por superficie.

---

## 5. Modelo de medición

### 5.1 Clasificador (3 etiquetas por métrica)

- **Clase** — `Negocio` (resultado para Civitatis) vs `Experiencia` (lo que le pasa al usuario en la interacción).
  *Test:* si mide lo que pasa **durante** la interacción → Experiencia; si mide lo que pasa **después** (ingreso, conversión, volumen) → Negocio.
- **Madurez** — 🟢 `Directa` · 🟡 `Proxy` · 🔴 `Gap`.
- **Señal** (solo Experiencia) — `Conductual` (lo que hace) vs `Actitudinal` (lo que siente). Una dimensión sana tiene las dos alineadas; su divergencia es hallazgo de diseño.

**Filtro de admisión al set IDEAL (3 condiciones):** una métrica entra solo si nombra (1) qué
mide a nivel de interacción, (2) con qué instrumento concreto podría producirse, (3) de qué tipo
de señal es. **Un artefacto, dos vistas:** la versión REALISTA es la IDEAL filtrada por
disponibilidad (conector ≠ none).

### 5.2 UX Health Index (termómetro)

Agrega el **estado ordinal** de las 7 dimensiones (no promedia métricas crudas):

```
🟢 = 2 · 🟡 = 1 · 🔴 crítico = 0 · 🔴 gap (sin dato) = excluido del denominador
Index = puntos / (dimensiones con dato × 2)  → 0–100 %
```

Se muestran **dos números**: `UX Health Index` + `Cobertura X/7`. Cada dimensión lleva semáforo
+ tendencia ↑↓→ vs periodo anterior (Abril guardado vs Mayo), con **drill-down** a la métrica que
arrastró la dimensión, y de ahí (Fase 2) a las features de Gate 3 del periodo.

---

## 6. Set único de KPIs por dimensión

> IDEAL = tabla completa · REALISTA (MVP) = filas con `connector ≠ none`.
> Conectores: `brain:<dashboard>` (vía Brain/BigQuery/Zendesk/VoC) · `api:<tool>` · `none` (sin instrumentar)

### 6.1 B2C Web

> Datos Mayo 2026. Fuente primaria: Brain (brain.civitatis.tech), explorado el 1 jun 2026.

| Dim | Métrica | Valor Mayo 2026 | Clase | Señal | Conector | Madurez |
|---|---|---|---|---|---|---|
| 🎯 Useful | NR alternativas sin dispo | +190% YoY | Negocio | — | brain:pillar/1 | 🟢 |
| | **Perfect Memories rate** | **70,7% global · 60,5% L12M · 830K reviews** | Exp | Actitudinal | brain:executive/perfect-memories | 🟡 *(live — ya no es gap)* |
| | Zero-results buscador | — | Exp | Conductual | none | 🔴 GA4 `search_no_results` no instrumentado |
| 🖱️ Usable | CR web | **4,12%** (-0,9%) · target 4,3% (73% OKR) | Negocio | — | brain:pillar/1 | 🟡 |
| | Sesiones con compra | 81.492 (-6,4%) | Negocio | — | brain:pillar/1 | 🟡 |
| | Abandono checkout por paso | 77,19% (sin desglose por paso) | Exp | Conductual | none | 🔴 ⚠️ Funnel Brain en **Mock Data** — no live |
| | Ratio PLP→PDP | 57% | Exp | Conductual | brain:pillar/1 | 🟡 |
| | CES global | **9,7** | Exp | Actitudinal | brain:analytics | 🟡 sin desglose por flujo |
| | Tasa de error en formularios | — | Exp | Conductual | none | 🔴 GA4 `form_error` no instrumentado |
| 🔍 Findable | Posición media CAT Landings | 5,1 (era 17,8) | Negocio | — | brain:pillar/1 | 🟢 |
| | Sesiones orgánicas web | 561.940 (-1% vs LY) | Negocio | — | brain:pillar/1 | 🟡 |
| | Ratio PLP→PDP | 57% | Exp | Conductual | brain:pillar/1 | 🟡 |
| | CTR upsellings | 4,4% (solo en A/B activos) | Exp | Conductual | api:optimizely | 🟡 |
| | **Buscador Typesense** (CIVI-3262) | Lanzado 21 may — sin métricas de impacto | Exp | Conductual | none | 🔴 candidato Gate 3 |
| | CTR buscador interno | — | Exp | Conductual | none | 🔴 GA4 `search_result_clicked` no instrumentado |
| 💜 Desirable | Usuarios recurrentes (semana) | 31.254 | Negocio | — | brain:pillar/1 | 🟡 |
| | NPS B2C | **9,3** · target >9,4 = **0% OKR** ⚠️ | Exp | Actitudinal | brain:pillar/1 | 🟡 sin desglose surface |
| | **CSAT B2C** | **94%** (+0,3pp vs año ant.) | Exp | Actitudinal | brain:cs-stats | 🟡 ⚠️ Valor anterior (87,7%) era de encuesta distinta — fuente correcta es Zendesk/Brain |
| | Temas VoC negativos recurrentes | 50 tags disponibles | Exp | Actitudinal | brain:voc | 🟡 requiere configurar Strategic Themes (10 min) |
| 🛡️ Credible | Auth Rate LATAM | 82% · target 85% | Negocio | — | brain:pillar/1 | 🟡 |
| | **CES global** | **9,7** | Exp | Actitudinal | brain:analytics | 🟡 |
| | VoC desconfianza (`#problemas_para_el_pago`, `#informacion_erronea_en_web`) | activo | Exp | Actitudinal | brain:voc | 🟡 |
| | **PIX Brasil + Mercado Pago MX** | **atascados** ⚠️ (CIVI-2092, CIVI-2089) | Exp | Conductual | none | 🔴 riesgo Credible LATAM activo |
| | Abandono en pago + motivo | — | Exp | Conductual | none | 🔴 motivo no capturado |
| ♿ Accessible | **VoC `#accesibilidad`** | activo (tag Zendesk confirmado) | Exp | Actitudinal | brain:voc | 🟡 señal viva — Accessible ya no es hueco |
| | VoC `#idioma` | activo | Exp | Actitudinal | brain:voc | 🟡 |
| | Lighthouse a11y score | — | Exp | Conductual | none | 🔴 Hito #11 Design Ops vacío · EAA |
| | % elementos ARIA / contraste | — | Exp | Conductual | none | 🔴 axe-core en CI no instrumentado |
| 💰 Valuable | NR Web Mayo | **€5,61M** (-5,4% vs LY) | Negocio | — | brain:product-surface/website | 🟡 |
| | AOV B2C | **€135** · target €142 = **0% OKR** ⚠️ | Negocio | — | brain:pillar/1 | 🟡 ⚠️ Valor anterior (€137,7) era mensual de MBR Mar; serie semanal fluctúa 119–138€ |
| | LTV 12M | €30,2 · target €32,5 = 25% OKR ⚠️ | Negocio | — | brain:impact | 🟡 |
| | Feature Adoption Rate | — | Exp | Conductual | none | 🔴 GA4 (% uso feature en 4 sem) |

> **Representación de Accessible (3 capas):** (1) señal cualitativa viva = tickets `#accesibilidad`
> del VoC Brain (volumen + tendencia + sentimiento) — **requiere configurar Strategic Themes**;
> (2) baseline técnico = Lighthouse CI + axe-core *(roadmap)*;
> (3) auditoría manual VoiceOver/TalkBack mensual *(roadmap)*. La card muestra "1 señal viva + 2
> instrumentos pendientes", nunca un hueco.

### 6.2 B2C App

> Datos Mayo 2026. Fuentes: Brain (Product Surface/App · User Behaviour) + Play Console.

| Dim | Métrica | Valor Mayo 2026 | Clase | Señal | Conector | Madurez |
|---|---|---|---|---|---|---|
| 🎯 Useful | NR App | **€1,32M** (+8,1% vs LY) | Negocio | — | brain:product-surface/app | 🟢 |
| | Active users Android | **856.026** (+32% vs 2025) | Negocio | — | brain:product-surface/app | 🟢 |
| | Installs iOS | 34.946 (↓20,1%) | Negocio | — | brain:product-surface/app | 🟡 |
| | Installs Android | 98.265 (↓7,9%) | Negocio | — | brain:product-surface/app | 🟡 |
| | Zero-results buscador in-app | — | Exp | Conductual | none | 🔴 |
| 🖱️ Usable | CR App | **6,73%** | Negocio | — | brain:pillar/1 | 🟢 |
| | **Ratio trip → reserva** | **90,8%** ↑ | Exp | Conductual | brain:product-surface/app | 🟢 dato Brain User Behaviour |
| | **Trips con reserva Mayo** | **265.618** (0,0% tenían favoritos previos ⚠️) | Exp | Conductual | brain:product-surface/app | 🟢 |
| | Crash rate Android | 0,21% (pico 1,4% el 13 may) | Exp | Conductual | api:playconsole | 🟡 |
| | ANR rate Android | 0,52% ⚠️ (umbral Play Store: 0,47%) | Exp | Conductual | api:playconsole | 🔴 |
| | Funnel `init_app`→compra por paso | Mock Data ⚠️ | Exp | Conductual | none | 🔴 Funnel Brain en Design Mode — no disponible |
| 🔍 Findable | Sesiones App (semana) | 335.357 | Negocio | — | brain:pillar/1 | 🟡 |
| | **Favoritos → conversión** | **0,0%** ⚠️ | Exp | Conductual | brain:product-surface/app | 🔴 100% de trips con reserva no tenían favoritos |
| | CTR buscador in-app | — | Exp | Conductual | none | 🔴 |
| | Zero-results in-app | — | Exp | Conductual | none | 🔴 |
| 💜 Desirable | Usuarios recurrentes + reactivados (semana) | 35.503 | Negocio | — | brain:pillar/1 | 🟡 |
| | **Uninstalls iOS** | **19.187 (+63,5% vs 2025)** ⚠️ | Exp | Conductual | brain:product-surface/app | 🔴 ratio uninstall/install iOS = 55% |
| | App ratings Play Store | **2,9** ⚠️ (benchmark 4,7–4,9) | Exp | Actitudinal | api:playconsole | 🔴 crónico desde ene 2026 |
| | NPS segmento app | — | Exp | Actitudinal | none | 🔴 no existe como segmento |
| | Push engagement (Braze) | activo | Exp | Conductual | api:braze | 🟡 no en dashboard UX |
| 🛡️ Credible | CSAT B2C global (incluye app) | **94%** | Negocio | — | brain:cs-stats | 🟡 |
| | CSAT canal app | — | Exp | Actitudinal | none | 🔴 Zendesk sin filtro channel=app |
| 💰 Valuable | NR App Mayo | **€1,32M** (+8,1%) | Negocio | — | brain:product-surface/app | 🟢 |
| | LTV 12M | €30,2 · target €32,5 = 25% OKR | Negocio | — | brain:impact | 🟡 |
| | Time-to-first-booking | — | Exp | Conductual | none | 🔴 |
| ♿ Accessible | VoC `#accesibilidad` (incluye canal app) | activo | Exp | Actitudinal | brain:voc | 🟡 |
| | accessibilityLabel / contentDescription | — | Exp | Conductual | none | 🔴 |
| | Dynamic Type / font scaling | — | Exp | Conductual | none | 🔴 |
| | VoiceOver/TalkBack: reserva completable | — | Exp | Conductual | none | 🔴 test manual mensual |

> ⚠️ **Dato crítico confirmado en Brain:** de los 265.618 trips con reserva en Mayo, solo **25 (0,0%)**
> tenían favoritos guardados. La feature de favoritos no aporta a conversión.
> El Funnel Brain (init_app→compra) existe en estructura pero está en **Mock Data (Design Mode)** —
> no estará live para el hackathon. MAU/new-users también pendientes ("data source not yet wired").

**Capa cualitativa de reviews (diferencial):** sin MCP de reviews en el registry → se construye con
App Store Connect API + Google Play Developer API; Claude clusteriza las reviews del último mes por
tema, las mapea a dimensión Honeycomb, etiqueta sentimiento y calcula **delta mes a mes**.
Alimenta Usable, Desirable y Credible.

### 6.3 B2B — Panel Agencias y Afiliados

> Datos Mayo 2026. Fuentes: Brain VoC B2B · CS Stats · Product Surface.
> Más datos de lo esperado — el segmento ya tiene señales reales.

| Dim | Métrica | Valor | Clase | Señal | Conector | Madurez |
|---|---|---|---|---|---|---|
| 🎯 Useful | NR Agencias (canal B2B Web) | **€2M** (+6,3% vs LY) | Negocio | — | brain:product-surface/website | 🟢 |
| | NR API (Afiliados) | **€310K** (+27,5% vs LY) | Negocio | — | brain:product-surface/api | 🟢 |
| | Nº agencias activas | **21.600** (target 26.000 = 62% OKR) | Negocio | — | brain:impact | 🟢 ON TRACK |
| | Tasa de éxito buscador semántico IA | — | Exp | Conductual | none | 🔴 lanzado may 2026 sin baseline |
| 🖱️ Usable | Tickets B2B Mayo | **1.206** | Negocio | — | brain:cs-stats | 🟡 |
| | Tasa de error / incidencias panel | — | Exp | Conductual | none | 🔴 solo postmortem panel en blanco 25/05 |
| 🔍 Findable | Éxito buscador semántico B2B | — | Exp | Conductual | none | 🔴 |
| 💜 Desirable | **NPS B2B (VoC Brain)** | **35** (escala 0–100) ⚠️ · OKR target >8 = **0%** | Exp | Actitudinal | brain:voc/b2b | 🔴 |
| | **CSAT B2B** | **95%** (+0,3pp) — superior al B2C | Exp | Actitudinal | brain:cs-stats | 🟢 divergencia NPS/CSAT = hallazgo de diseño |
| | NPS post-primera compra AGE (CIVI-2981) | Ready to Launch (retrasado) | Exp | Actitudinal | none | 🔴 cuando active → primera señal NPS B2B real |
| 🛡️ Credible | **Pain point Facturación (VoC B2B)** | **Hot topic #1 · sentiment 40 (negativo)** | Exp | Actitudinal | brain:voc/b2b | 🔴 "Mi factura no muestra el VAT number" |
| | API Rate Limits (VoC B2B) | sentiment 75 (positivo) | Exp | Actitudinal | brain:voc/b2b | 🟡 |
| | Errores traducción BR/EN panel | — | Exp | Conductual | none | 🔴 documentado en Confluence |
| ♿ Accessible | a11y panel agencias/afiliados | — | Exp | Conductual | none | 🔴 |
| 💰 Valuable | Revenue B2B total | **€2,31M** (Agencias €2M + API €310K) | Negocio | — | brain:product-surface | 🟢 B2B crece mientras Web B2C cae |
| | % CM1 agencias | 53% OKR · ON TRACK | Negocio | — | brain:impact | 🟢 |

**Hallazgo B2B relevante para el dashboard:** el CSAT B2B es 95% (superior al B2C) pero el NPS B2B
es 35 (muy por debajo del target >8). Esta divergencia conductual/actitudinal es exactamente el tipo
de señal que el dashboard debe hacer visible — los agentes resuelven sus tickets satisfactoriamente
pero no recomendarían la plataforma. El pain point #1 es **facturación**, no usabilidad del panel.

---

## 7. Mapa de cobertura y énfasis

> Actualizado con datos reales de Brain (1 jun 2026). ⚠️ = alerta activa.

| Dimensión | B2C Web | B2C App | B2B |
|---|---|---|---|
| 🎯 Useful | 🟡 Perfect Memories live · falta buscador | 🟡 NR/installs live · falta buscador | 🟢 NR Agencias + API live |
| 🖱️ Usable | 🟡 CR 4,12% · funnel en Mock ⚠️ | 🟢 trip→reserva 90,8% · 🔴 ANR>umbral ⚠️ | 🔴 solo incidencias técnicas |
| 🔍 Findable | 🔴 Typesense sin Gate 3 ⚠️ | 🔴 favoritos→conversión 0% ⚠️ | 🔴 |
| 💜 Desirable | 🟡 NPS 9,3 (0% OKR) · CSAT 94% | 🔴 uninstalls iOS +63,5% ⚠️ · ratings 2,9 ⚠️ | 🔴 NPS B2B 35 (0% OKR) ⚠️ · 🟢 CSAT 95% |
| 🛡️ Credible | 🟡 CSAT 94% · VoC vivo · PIX atascado ⚠️ | 🟡 CSAT sin filtro canal | 🔴 facturación pain point #1 |
| ♿ Accessible | 🟡 VoC `#accesibilidad` live · 🔴 técnico | 🟡 VoC live · 🔴 técnico | 🔴 |
| 💰 Valuable | 🟡 AOV 0% OKR ⚠️ · NR Web ↓5,4% | 🟢 NR App +8,1% · LTV 25% OKR | 🟢 Revenue B2B +6,3% · CM1 ON TRACK |

**Énfasis por orden de impacto:**
1. **Findable** — gap más urgente y accionable en ambos B2C; tres eventos GA4 lo resuelven; único gap con documentación literal interna.
2. **Usable** — desglose de checkout por paso convierte el síntoma (77,19 %) en diagnóstico.
3. **Desirable App** — segmentar NPS/CSAT por canal (configuración, no instrumentación nueva).
4. **Accessible** — gap más estructural (cero técnico en todos los segmentos) pero el de baseline más barato (VoC `#accesibilidad` ya vivo + Lighthouse CI + test VoiceOver mensual). Requisito legal EAA.
5. **VoC de reviews App** — la capa cualitativa más diferencial; no existe en ningún sitio; conecta quejas reales con dimensiones Honeycomb.

---

## 8. Vista 2 — Gate 3 · Impacto por Feature

El diseñador crea una entrada el día del lanzamiento. Campos: `feature_name` + Jira · `designer` ·
`surface` · `ux_dimension` (Honeycomb) · `hypothesis` · `metric_name` · `value_before` ·
`value_after` (null hasta 14/30 d) · `measurement_period` · `launch_date` · `status` (open/closed).
Entradas open >30 d → alerta. Exportable a CSV. Sugerencia asistida de dimensión por agente
(human-in-the-loop obligatorio: el diseñador valida coherencia feature↔dimensión↔métrica).

Pre-cargadas para la demo: **Checkout 2 pasos v4.7.0** (Solvey, Usable+Valuable) y **Upsell Free
Tour→Privado** (Useful+Valuable).

---

## 9. Capa MCP / Agente

- **A (se construye):** MCP de lectura — Claude consulta estado de Salud UX y historial Gate 3.
- **B (narrativa de demo):** ante el jurado, *"¿por qué pudo caer el AOV en mayo?"* → Claude cruza
  Salud UX (dimensión ↓) + Gate 3 (features del periodo) + VoC (temas que empeoraron) y devuelve
  hipótesis estructurada. El "no tenemos idea de por qué" resuelto en vivo.
- **C (fuera del MVP):** escritura de entradas Gate 3 por chat.

---

## 10. Enfoque técnico

```
[Diseñadores] ──formulario Gate 3──►┐
[PM / Liderazgo] ──Salud UX────────►│   UX QUALITY DASHBOARD   ┌── lee ──► BRAIN
[Claude / MBR] ──MCP query─────────►┘   (lente UX sobre Brain)  │            ├─ BigQuery (Executive, Product Surface)
                                                                 │            ├─ Zendesk (CS Stats, CSAT)
                                        capa propia del dashboard:│            └─ VoC Explorer (temas + sentimiento)
                                        - mapeo Honeycomb+HEART   ├── api ──► Optimizely · Clarity · Adjust · Stores · Braze
                                        - señales UX que faltan   └── api ──► App Store Connect + Google Play (reviews → Claude)
                                        - registro Gate 3
```

**Stack MVP (48h):** React + Tailwind + Vercel. Dashboard **data-driven**: renderiza un JSON de
contrato. Capa de adaptadores — cada métrica declara su conector (`brain:*` | `api:*` | `cached` |
`none`); `cached`→`live` sin tocar UI.

**Plan 48h — dos pistas paralelas** (acuerdan el JSON de contrato en la hora 0):
- *Pista Data* (persona de Data): credenciales + confirmar qué expone Brain/BigQuery/Zendesk/VoC; conectar lo disponible (goal: todo lo disponible = conexión real).
- *Pista Producto* (Jota + front): dashboard completo contra JSON de contrato con datos reales precargados; demo-able al final del día 1.

**Quién hace el trabajo:** humano (diseñadores rellenan Gate 3, juicio de dimensión) + agente
(validación de coherencia, síntesis VoC, resumen Salud UX para MBR, detección de entradas abiertas).

---

## 11. Riesgos

| Riesgo | Mitigación |
|---|---|
| Acceso a fuentes con fricción (Tableau 2FA, ownership GA4) | **Mitigado en gran parte:** Brain ya expone BigQuery+Zendesk+VoC. El dashboard lee de Brain, no de cada fuente. |
| **Funnel Brain en Mock Data (Design Mode)** | Marcado como 🔴 en el dashboard con badge "sin instrumentar". No bloquea la demo. El abandono de checkout (77,19%) se muestra como proxy precargado. |
| VoC B2C Anécdotas sin Strategic Themes configurados | Configurar antes del hackathon (10 min, Domingo Martín). Sin esto, los tags `#accesibilidad` y `#problemas_para_el_pago` no muestran anécdotas. |
| Periodo actual sin sincronizar (Zendesk sync, día 1 de mes) | Usar corte de Mayo (cerrado) para la demo; Abril como baseline de tendencia. |
| Adopción del ritual Gate 3 | Incorporarlo explícitamente al Gate 3 del modelo operativo (cierra H0-2/H1-1), no solo herramienta disponible. |
| Solapamiento con el proyecto Growth (ETL App) | Delimitar desde la hora 0: Growth construye el ETL de adquisición; este dashboard **consume** sus tablas en BigQuery y añade la lente UX. |
| Reviews App sin MCP en registry | Construir con App Store Connect API + Google Play API + clustering Claude. |
| CSAT B2C mal referenciado en docs anteriores | **Corregido:** CSAT real = 94% (Zendesk/Brain CS Stats). El 87,7% de documentos previos era de una encuesta distinta (Tableau). Actualizar cualquier referencia. |

---

## 12. Roadmap

```
Hackathon (jun 2026)  — MVP: Vista 1 leyendo de Brain (Negocio live) + Vista 2 Gate 3 + MCP básico + VoC #accesibilidad vivo
Fase 1 — Q3 2026       — Conectores api:* (Optimizely, Clarity, Adjust, Stores, Braze); reviews App clusterizadas; tendencia real multi-mes
Fase 2 — Q4 2026       — Instrumentar gaps Findable (3 eventos GA4) + checkout por paso + CES por flujo; segmentar NPS/CSAT por canal
Fase 3 — Q1 2027       — Accessible técnico (Lighthouse CI + axe + VoiceOver mensual); ownership por métrica → PDIs; B2B deep-dive
```

---

## 13. Preguntas abiertas

- [ ] ¿Brain expone API/endpoint para que el dashboard lea sus métricas, o hay que ir a BigQuery directamente? (preguntar al owner de Brain)
- [ ] ¿El VoC Explorer permite query programática de volumen/sentimiento por tema y mes?
- [ ] ¿El proyecto Growth de ETL App expondrá sus tablas en BigQuery para que las consumamos?
- [ ] ¿NPS/CSAT en Brain pueden segmentarse por surface (web/app) o requiere reconfigurar la encuesta?
- [ ] ¿Se incorpora el registro Gate 3 a la plantilla de cierre de iniciativa del modelo operativo?

---

---

## 14. Modelo de datos

```typescript
// Vista 1 — Indicador de Salud UX
type Connector = 'brain:pillar/1'|'brain:product-surface'|'brain:cs-stats'|'brain:voc'|
                 'brain:executive'|'brain:impact'|`api:${string}`|'cached'|'none'
type Clase     = 'negocio' | 'experiencia'
type Senal     = 'conductual' | 'actitudinal' | null
type Madurez   = 'green' | 'yellow' | 'red'   // red incluye gap (value=null)
type Dim = 'useful'|'usable'|'findable'|'desirable'|'credible'|'accessible'|'valuable'
type Segment   = 'b2c_web' | 'b2c_app' | 'b2b'

type Metric = {
  id:          string
  dimension:   Dim
  segment:     Segment
  surface:     string       // 'global' | 'Checkout' | 'PLP' | 'PDP' | etc.
  name:        string
  clase:       Clase
  senal:       Senal
  instrument:  string       // instrumento nombrable (filtro set IDEAL)
  connector:   Connector
  value:       number | string | null
  value_prev:  number | string | null   // corte Abril → habilita tendencia ↑↓→
  unit:        string
  target:      string | null
  madurez:     Madurez
  note:        string | null
}

type DimensionState = {
  dimension:  Dim
  status:     'green' | 'yellow' | 'red' | 'gray'
  trend:      'up' | 'down' | 'flat'
}

type Snapshot = {
  period:    string          // "2026-05"
  segment:   Segment
  metrics:   Metric[]
  index:     number          // 0–100, calculado sobre estados ordinales
  coverage:  string          // "4/7"
  states:    DimensionState[]
}

// Vista 2 — Entrada Gate 3
type FeatureEntry = {
  id:                 string
  feature_name:       string
  ticket_url:         string
  designer:           string
  surface:            string
  ux_dimension:       Dim
  hypothesis:         string
  metric_name:        string
  value_before:       number
  value_after:        number | null
  measurement_period: '7d' | '14d' | '30d'
  launch_date:        string    // ISO date
  closed_date:        string | null
  status:             'open' | 'closed'
}
```

---

## 15. Requisitos no funcionales

- **Acceso:** autenticación SSO con credenciales Civitatis (@civitatis.com). No requiere nuevo sistema de auth.
- **Disponibilidad:** herramienta interna, no crítica. SLA de best-effort.
- **Latencia:** Vista 1 con datos de hasta 7 días de desfase (Brain auto-actualizado); Gate 3 es entrada manual sin requisitos de latencia.
- **Exportación:** Vista 2 exportable a CSV para presentaciones y MBRs.
- **API/MCP:** el dashboard expone un endpoint MCP que permite a Claude consultar el estado de Salud UX y el historial de Gate 3 desde una sesión de conversación.
- **Sin ETL propio:** el dashboard lee de Brain (BigQuery + Zendesk + VoC). No tiene infraestructura de extracción propia.

---

## 16. Alcance MVP (Hackathon — 48h)

### En scope
- [x] Vista 1 data-driven (JSON de contrato) con 3 segmentos × 7 dimensiones × 2 columnas (Negocio/Experiencia)
- [x] Datos reales de Brain precargados (Mayo 2026 como corte principal, Abril como baseline de tendencia)
- [x] UX Health Index sobre estado de dimensiones + Cobertura X/7 + tendencia ↑↓→ por dimensión
- [x] Drill-down: Index → dimensión → métrica
- [x] Badges de fuente por métrica (`brain:*` live / `api:*` live / `sin instrumentar`)
- [x] Filtro por surface dentro de cada segmento
- [x] Formulario Gate 3 funcional con todos los campos
- [x] ≥2 entradas Gate 3 precargadas con casos reales (Checkout 2 pasos, Upsell Free Tour→Privado)
- [x] Capa MCP básica: endpoint de consulta de Salud UX + historial Gate 3
- [x] Narrativa de demo MCP: "¿por qué cayó el AOV?" → Claude responde con hipótesis estructurada

### Fuera de scope (MVP)
- Integración en tiempo real con GA4, Tableau (NPS segmentado), Play Console reviews
- Análisis automático de reviews App (VoC cualitativo) — Fase 1
- Autenticación SSO
- Alertas automáticas de entradas Gate 3 abiertas >30 días
- Segmentos Supply e Internal (Admin Tool)
- Exportación CSV de Gate 3

---

*PRD v2.1 — actualizado con exploración completa de Brain (1 jun 2026): Executive, Product Surface (Website/App/API), Supply, Impact, WBR, Business Events, Customer Service Stats, VoC B2C/B2B, OKR Canvas. Owner: Jota Coto · jcoto@civitatis.com*
