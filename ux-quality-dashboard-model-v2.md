# UX Quality Dashboard — Modelo de medición v2

> Resultado del grill-me del 1 jun 2026. Cierra el árbol de decisión del dashboard y
> define el corazón del producto: el set único de KPIs por dimensión (ideal = tabla
> completa, realista = tabla filtrada por disponibilidad), el cómputo del índice, la
> arquitectura de datos y el plan de las 48h.
>
> Owner: Jota Coto · Vive junto a `ux-quality-dashboard-prd-v2.md`
> Para las tablas de KPIs completas y actualizadas con datos Brain, ver PRD-v2 §6.

---

## 0. Tracción de chapter (por qué esto importa más allá del hackathon)

Tu propio **PD Model v0.7 roadmap** ya pide este dashboard sin nombrarlo:

- **H0-2 · "PD sin Gate"** (blocker): el PD no firma ningún gate; si PM y PE lanzan algo
  con mala UX, no hay mecanismo para registrarlo. → **Vista 2 (Gate 3) es ese mecanismo.**
- **H1-1 · "Gate de Calidad UX antes de Gate 3"** (riesgo alto): pide un checkpoint con
  CES, Error Rate y una métrica cualitativa donde el PD entrega veredicto. → **Vista 1
  (Salud UX) es ese veredicto estructurado.**

El dashboard no es una herramienta nueva que justificar: es el artefacto que dos puntos
abiertos de tu modelo operativo ya están reclamando.

---

## 1. Decisiones cerradas

| # | Decisión | Resultado |
|---|---|---|
| 1 | Estructura Vista 1 | Dos columnas por dimensión: **Negocio** vs **Experiencia**. El gap entre ambas es el producto. |
| 2 | Regla de dimensión | Mín. **1 métrica de negocio + mín. 2 señales de experiencia**. |
| 3 | Clasificador | 3 etiquetas: **Clase** (Negocio/Experiencia) · **Madurez** (🟢🟡🔴) · **Señal** (Conductual/Actitudinal, solo en experiencia). |
| 4 | Termómetro | **UX Health Index** sobre estado de dimensiones + **Cobertura X/7** + tendencia ↑↓→ por dimensión con drill-down a métrica. |
| 5 | Tendencia | Dos cortes reales: **Abril guardado vs Mayo**. Tendencia real en la demo. |
| 6 | Mantenimiento | **No se mantiene, se consulta**: auto-pull al cargar. Elimina el riesgo de adopción. |
| 7 | Acceso a datos | **Brain-first**: Brain (brain.civitatis.tech) ya integra BigQuery + Zendesk + VoC. Conectar Brain directamente antes que ir a las fuentes sueltas. Resto por `api:*`. Goal: todo lo disponible = conexión real. |
| 8 | Arquitectura | **Brain-first**, resto por API/MCP. Capa de adaptadores: cada métrica declara conector (`brain:*` \| `api:*` \| `cached` \| `none`); `cached`→`live` sin tocar UI. |
| 9 | Plan 48h | **Dos pistas paralelas** (Data / Producto) con JSON de contrato como interfaz. |
| 10 | Capa MCP | **A** se construye (lectura), **B** es la narrativa de demo (correlación AOV↔UX en vivo), **C** (escritura) fuera del MVP. |
| 11 | Set ideal | **Filtro de 3 condiciones**. Un artefacto, dos vistas: realista = ideal filtrado por disponibilidad. |

---

## 2. El clasificador (regla del chapter)

**Test de Clase (una línea):** una métrica es *Experiencia* si mide algo que le pasa al
usuario **durante la interacción** (esfuerzo, error, éxito de tarea, percepción). Es
*Negocio* si mide algo que le pasa a Civitatis **después** (ingreso, conversión, volumen).
Si una decisión de diseño puede moverla pero el dato no describe la interacción → Negocio.

**Tipo de señal (solo en Experiencia):**
- **Conductual** — lo que el usuario *hace* (observado: abandono, zero-results, error rate, task success). No miente, no explica el porqué.
- **Actitudinal** — lo que el usuario *siente/dice* (NPS, CES, CSAT, reviews). Explica el porqué, tiene sesgo de auto-reporte.
- Una dimensión sana de verdad tiene **conductual y actitudinal alineadas**. Cuando divergen → hallazgo de diseño.

**Madurez (semáforo de instrumentación):**
- 🟢 **Directa** — mide lo que dice medir, con desglose por surface/feature.
- 🟡 **Proxy** — se aproxima con un dato existente (a menudo de negocio) porque no hay señal UX directa todavía.
- 🔴 **Gap** — no se mide en absoluto. Casilla honesta, visible, no ocultada.

**Filtro de admisión al set IDEAL (3 condiciones):** una métrica entra solo si nombra
(1) qué mide a nivel de interacción, (2) con qué **instrumento concreto** podría producirse
—aunque hoy no exista—, (3) de qué tipo de señal es. Si no sabes nombrar el instrumento,
es un deseo, no un KPI.

---

## 3. UX Health Index — cómputo

**No se promedian métricas crudas** (mezclar un CES con un % de abandono es basura
estadística). Se agrega el **estado ordinal** de las 7 dimensiones:

```
🟢 = 2 pts   🟡 = 1 pt   🔴 crítico = 0 pts
🔴 Gap (sin dato) = NO puntúa → se excluye del denominador

Index = puntos obtenidos / (dimensiones con dato × 2)   → 0–100 %
```

Se muestran **dos números arriba, no uno**:

> **UX Health Index: 58 %** *(En riesgo)* · ↓ −6 pts vs Abril
> **Cobertura de medición: 4/7 dimensiones** *(Accessible, Useful, Findable sin señal directa)*

La cobertura impide el truco perverso de que el índice "suba" porque ignoras lo que no mides.

**Drill-down:** Index baja → ¿qué dimensión (↓)? → ¿qué métrica la arrastró? → (Fase 2)
¿qué features de Gate 3 se lanzaron sobre esa dimensión en el periodo?

---

## 4. Set único de KPIs por dimensión — B2C Web

> **IDEAL** = tabla completa. **REALISTA (MVP)** = filas con conector ≠ `none`.
> Conectores: `brain:*` (Brain/BigQuery/Zendesk/VoC) · `api:*` (API directa) · `none` (sin instrumentar)
> Datos: Mayo 2026. Ver PRD-v2 §6.1 para la tabla completa con valores y URLs.

### 🎯 Useful — ¿resuelve necesidades reales?
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| NR alternativas sin disponibilidad | Negocio | — | Brain/BigQuery | `brain:pillar/1` | 🟢 (+190% YoY) |
| **Perfect Memories rate** | Experiencia | Actitudinal | Brain Executive | `brain:executive/perfect-memories` | 🟡 **70,7% global · 830K reviews · live** |
| Zero-results rate buscador | Experiencia | Conductual | Evento GA4 `search_no_results` | `none` | 🔴 Gap |

### 🖱️ Usable — ¿completa la tarea sin fricción?
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| CR web | Negocio | — | Brain/BigQuery | `brain:pillar/1` | 🟡 4,12% (target 4,3% = 73% OKR) |
| CES global | Experiencia | Actitudinal | Brain dashboard | `brain:analytics` | 🟡 **9,7** (sin desglose por flujo) |
| Abandono checkout por paso | Experiencia | Conductual | Brain Funnels | `none` | 🔴 ⚠️ **Funnel en Mock Data** — no live |
| Tasa de error en formularios | Experiencia | Conductual | Evento GA4 `form_error` | `none` | 🔴 Gap |

### 🔍 Findable — ¿encuentra lo que busca?
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| Posición media CAT Landings | Negocio | — | Brain/BigQuery | `brain:pillar/1` | 🟢 5,1 (era 17,8) |
| CTR upsellings | Experiencia | Conductual | Optimizely | `api:optimizely` | 🟡 4,4% (solo en A/B activos) |
| CTR buscador interno | Experiencia | Conductual | Evento GA4 `search_result_clicked` | `none` | 🔴 Gap (confirmado en Confluence) |

### 💜 Desirable — ¿genera apego, quiero volver?
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| Usuarios recurrentes (semana) | Negocio | — | Brain/BigQuery | `brain:pillar/1` | 🟡 31.254 |
| NPS B2C | Experiencia | Actitudinal | Brain Pilar 1 | `brain:pillar/1` | 🟡 **9,3** — target >9,4 = 0% OKR ⚠️ |
| CSAT B2C | Experiencia | Actitudinal | Brain CS Stats (Zendesk) | `brain:cs-stats` | 🟡 **94%** · 6.360 tickets mayo |
| Temas VoC negativos | Experiencia | Actitudinal | Brain VoC | `brain:voc` | 🟡 50 tags disponibles |

### 🛡️ Credible — ¿el usuario confía?
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| Auth Rate LATAM | Negocio | — | Brain/BigQuery | `brain:pillar/1` | 🟡 82% (target 85%) |
| CES global | Experiencia | Actitudinal | Brain | `brain:analytics` | 🟡 9,7 |
| VoC #problemas_para_el_pago / #informacion_erronea | Experiencia | Actitudinal | Brain VoC | `brain:voc` | 🟡 activo |
| PIX Brasil + Mercado Pago MX | Experiencia | Conductual | WBR/Jira | `none` | 🔴 ⚠️ atascados (CIVI-2092/2089) |
| Abandono en pago + motivo | Experiencia | Conductual | GA4 + survey | `none` | 🔴 Gap |

### ♿ Accessible — ¿funciona para todos?
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| VoC #accesibilidad | Experiencia | Actitudinal | Brain VoC | `brain:voc` | 🟡 **activo — Accessible ya no es hueco** |
| Lighthouse a11y score | Experiencia | Conductual | Lighthouse CI | `none` | 🔴 Gap · EAA obligatorio |
| % elementos ARIA / alt text | Experiencia | Conductual | axe-core en CI | `none` | 🔴 Gap |

### 💰 Valuable — ¿valor mutuo usuario y negocio?
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| NR Web Mayo | Negocio | — | Brain Product Surface | `brain:product-surface/website` | 🟡 €5,61M (−5,4% vs LY) |
| AOV B2C | Negocio | — | Brain/BigQuery | `brain:pillar/1` | 🟡 **€135** — target €142 = 0% OKR ⚠️ |
| LTV 12M | Negocio | — | Brain Impact | `brain:impact` | 🟡 €30,2 (target €32,5 = 25%) |
| Feature Adoption Rate | Experiencia | Conductual | GA4 (% uso en 4 sem) | `none` | 🔴 Gap |

---

## 5. Set único de KPIs por dimensión — B2C App

> Datos: Mayo 2026. Fuente: Brain Product Surface → App (Installs & Devices + User Behaviour).
> Ver PRD-v2 §6.2 para la tabla completa.

### 🎯 Useful
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| NR App Mayo | Negocio | — | Brain Product Surface | `brain:product-surface/app` | 🟢 **€1,32M (+8,1% vs LY)** |
| Installs iOS / Android | Negocio | — | Brain Product Surface | `brain:product-surface/app` | 🟡 34.946 / 98.265 (↓20,1% / ↓7,9%) |
| Active users Android | Negocio | — | Brain Product Surface | `brain:product-surface/app` | 🟢 **856.026 (+32%)** |
| Zero-results buscador in-app | Experiencia | Conductual | Evento GA4 app | `none` | 🔴 Gap |

### 🖱️ Usable
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| CR App | Negocio | — | Brain/BigQuery | `brain:pillar/1` | 🟢 6,73% |
| **Ratio trip → reserva** | Experiencia | Conductual | Brain App User Behaviour | `brain:product-surface/app` | 🟢 **90,8%** |
| Crash rate / ANR rate | Experiencia | Conductual | Play Console | `api:playconsole` | 🔴 ANR 0,52% > umbral 0,47% ⚠️ |
| Abandono checkout por paso app | Experiencia | Conductual | Brain Funnels | `none` | 🔴 ⚠️ Funnel en Mock Data |

### 🔍 Findable
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| **Favoritos → conversión** | Experiencia | Conductual | Brain App User Behaviour | `brain:product-surface/app` | 🔴 **0,0%** ⚠️ de 265.618 trips, solo 25 con favoritos |
| CTR / zero-results buscador in-app | Experiencia | Conductual | Evento GA4 app | `none` | 🔴 Gap |

### 💜 Desirable
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| **Uninstalls iOS** | Experiencia | Conductual | Brain Product Surface | `brain:product-surface/app` | 🔴 **19.187 (+63,5% vs 2025)** ⚠️ ratio 55% |
| App ratings Play Store | Experiencia | Actitudinal | Play Console | `api:playconsole` | 🔴 ~2,9 (benchmark 4,7–4,9) ⚠️ |
| Push engagement (Braze) | Experiencia | Conductual | Braze | `api:braze` | 🟡 activo desde v4.6.0 |
| NPS segmento app | Experiencia | Actitudinal | Encuesta filtro canal | `none` | 🔴 Gap |

### 🛡️ Credible
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| CSAT B2C global (incluye app) | Negocio | — | Brain CS Stats | `brain:cs-stats` | 🟡 **94%** |
| CSAT canal app | Experiencia | Actitudinal | Zendesk filtro `channel=app` | `none` | 🔴 Gap (filtro no configurado) |

### ♿ Accessible
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| VoC #accesibilidad (canal app) | Experiencia | Actitudinal | Brain VoC | `brain:voc` | 🟡 activo |
| accessibilityLabel / Dynamic Type / VoiceOver | Experiencia | Conductual | Xcode / Android Studio | `none` | 🔴 Gap |

### 💰 Valuable
| Métrica | Clase | Señal | Instrumento | Conector | Madurez |
|---|---|---|---|---|---|
| NR App Mayo | Negocio | — | Brain Product Surface | `brain:product-surface/app` | 🟢 **€1,32M (+8,1%)** |
| LTV 12M | Negocio | — | Brain Impact | `brain:impact` | 🟡 €30,2 (target €32,5 = 25%) |
| Time-to-first-booking | Experiencia | Conductual | Adjust + GA4 | `none` | 🔴 Gap |

---

## 5b. B2B — Panel Agencias y Afiliados (resumen)

> Ver PRD-v2 §6.3 para la tabla completa. Más datos de lo esperado gracias a la exploración de Brain.

| Dato clave | Valor Mayo 2026 | Conector |
|---|---|---|
| NR Agencias | €2M (+6,3% vs LY) | `brain:product-surface/website` |
| NR API Afiliados | €310K (+27,5% vs LY) | `brain:product-surface/api` |
| Nº agencias activas | 21.600 (target 26.000 = 62% ON TRACK) | `brain:impact` |
| CSAT B2B | **95%** (superior al B2C) | `brain:cs-stats` |
| NPS B2B (VoC) | **35/100** — OKR >8 = 0% ⚠️ | `brain:voc/b2b` |
| Pain point #1 | Facturación (sentiment 40, negativo) | `brain:voc/b2b` |
| Buscador semántico IA (CIVI-3261) | Sin métricas de adopción | `none` |

---

## 6. JSON de contrato (interfaz entre Pista Data y Pista Producto)

> Acordar la primera hora del día 1. Es lo que desacopla a Data del resto del equipo.

```typescript
type Connector = `brain:${string}` | `api:${string}` | 'cached' | 'none'
// brain:pillar/1 | brain:product-surface/website | brain:product-surface/app |
// brain:product-surface/api | brain:cs-stats | brain:voc | brain:voc/b2b |
// brain:executive/perfect-memories | brain:impact | brain:analytics
type Clase     = 'negocio' | 'experiencia'
type Senal     = 'conductual' | 'actitudinal' | null
type Madurez   = 'green' | 'yellow' | 'red'          // red incluye gap (value=null)
type Dim = 'useful'|'usable'|'findable'|'desirable'|'credible'|'accessible'|'valuable'

type Metric = {
  id: string
  dimension: Dim
  segment: 'b2c_web' | 'b2c_app' | 'b2b'
  name: string
  clase: Clase
  senal: Senal
  instrument: string            // nombre del instrumento, SIEMPRE presente (filtro ideal)
  connector: Connector          // de dónde se lee hoy
  value: number | string | null
  value_prev: number | string | null   // corte de Abril → habilita tendencia
  unit: string
  target: string | null
  madurez: Madurez
  note: string | null
}

type DimensionState = {           // derivado, no se escribe a mano
  dimension: Dim
  status: 'green' | 'yellow' | 'red' | 'gray'  // gray = sin dato
  trend: 'up' | 'down' | 'flat'
}

type Snapshot = {
  period: string                  // "2026-05"
  metrics: Metric[]
  index: number                   // calculado: ver §3
  coverage: string                // "4/7"
}
```

---

## 7. Plan 48h — dos pistas paralelas

**Hora 0 (día 1):** las dos pistas acuerdan el JSON de contrato (§6). A partir de ahí, nadie bloquea a nadie.

| Pista | Quién | Día 1 | Día 2 |
|---|---|---|---|
| **Data** | Persona de Data | ¿Brain expone API REST o hay que ir a BigQuery raw? · Configurar Strategic Themes VoC (10 min, Domingo Martín) · Play Console API · Confirmar si reto Growth 360º App expone tablas BQ. Entrega el contrato poblado. | Encender conectores: `cached`→`brain:*`/`api:*`. Cada conexión que llega es upside. |
| **Producto** | Jota + front | Dashboard completo contra JSON de contrato con **datos reales de Brain precargados** (CR 4,12% · AOV €135 · NPS 9,3 · CSAT 94% · Perfect Memories 70,7% · Uninstalls iOS +63,5%). Vista 1 + Index + tendencia vs Abril + drill-down. Demo-able al final del día 1. | Reapuntar conectores a live, montar endpoint MCP, 2 entradas Gate 3 reales, ensayar narrativa de demo. |

**Goal:** todo lo que Brain/API expongan = conexión real. Lo que no, `cached` con badge
(`Brain live` / `sin instrumentar`). La demo nunca está en riesgo; lo live es upside.

**Narrativa de demo (MCP-B):** ante el jurado, preguntar a Claude *"¿por qué pudo caer el AOV
en mayo?"* → Claude cruza Salud UX (Usable ↓6) + Gate 3 (features sobre checkout) y devuelve
hipótesis estructurada. Es el "no tenemos idea de por qué" del MBR de Marzo resuelto en vivo.

---

## 8. Pre-trabajo crítico (antes del día 1)

- [ ] **Configurar Strategic Themes del VoC en Brain** (`/contacts/voc/configuration`) — 10 min, owner Domingo Martín. Sin esto las señales Credible/Accessible no muestran anécdotas.
- [ ] Confirmar con Data: ¿Brain expone API REST o hay que ir a BigQuery raw?
- [ ] Sacar el corte de **Abril** filtrando por mes en Brain (habilita la tendencia real).
- [ ] Acordar el JSON de contrato (ver §6) en la hora 0 — es el desacoplador de pistas.
- [ ] 2 entradas Gate 3 reales precargadas: Checkout 2 pasos v4.7.0 (Solvey, Usable) + Upsell Free Tour→Privado (Useful+Valuable).
- [ ] Briefar al equipo con `brain-data-map.md` (dónde está cada dato) + §7 (dos pistas).
