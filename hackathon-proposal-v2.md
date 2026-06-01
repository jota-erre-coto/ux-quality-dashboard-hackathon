# UX Quality Dashboard
## Civitatis mide el negocio. Nadie mide la experiencia.

**Jose Ramón Coto Gallego · Head of UX**
Formato: 2 días · Equipo: UX + Engineering + Data
Pilar: 1 — Ofrecer una experiencia excepcional para el cliente final

---

## El problema · 01

### Cuando el AOV cayó de €155 a €135, nadie supo por qué.

En el MBR de Marzo la respuesta fue textual: *"no tenemos una idea clara de por qué".*

No faltaban datos de negocio. Lo que faltaba era la capa intermedia.

**Lo que sí teníamos:**
- AOV cayendo semana a semana en Brain
- NPS B2C en 9,3 — sin moverse hacia el target de 9,4 (0% de progreso OKR)
- AOV B2C sin moverse hacia €142 (0% de progreso OKR)
- CSAT en 94% pero sin desglose por feature ni por surface

**Lo que no teníamos — y no existe en ningún sitio:**

```
feature lanzada → diseñador responsable → dimensión UX intervenida → métrica antes/después
```

Cada semana se lanzan features que intervienen el checkout, la PDP, el buscador. Ninguna tiene un registro de qué dimensión de experiencia intentaba mejorar ni qué métrica se esperaba mover.

Cuando el equipo de Pilar 1 entra al MBR, puede ver que el AOV cayó. No puede ver si en ese mismo periodo se lanzaron tres cambios en el checkout que aumentaron la fricción en Usable, o si la función de favoritos (que no aporta a conversión: 0% de trips con reserva tenían favoritos previos) está drenando Findable en App.

**El patrón de los últimos 4 MBRs de Pilar 1:**
- NPS > 9.4: métricas definidas · progreso **0%**
- ACV B2C €137→€142: métricas definidas · progreso **0%**
- CSAT por región: métricas definidas · progreso **0%**

No falta ambición de medir. Falta la capa de instrumentación que conecta las decisiones de diseño con los datos.

---

## La oportunidad · 02

### Brain ya tiene los datos. Nadie los mira con lente de experiencia.

Durante la preparación de este reto, exploramos Brain en detalle. El hallazgo cambia el punto de partida:

**Brain ya integra:**
- BigQuery con CR, AOV, NR, NPS, CES, sesiones Web/App actualizados automáticamente
- Zendesk con CSAT desglosado B2C/B2B (94% B2C · 95% B2B)
- VoC con 50 tags de Zendesk clusterizados por tema y sentimiento — incluyendo `#accesibilidad`, `#problemas_para_el_pago`, `#informacion_erronea_en_web`
- Funnel Web y App (estructura completa, datos en camino)
- Impact Tracker con el progreso de todos los OKRs en tiempo real

**El problema no es que los datos no existan. Es que nadie los organiza bajo una lente de experiencia.**

Brain mide el negocio con excelencia. No tiene ningún artefacto que diga:
*"la dimensión Usable de B2C Web está en riesgo porque el abandono en checkout subió y en ese mismo periodo se lanzaron dos cambios en el flujo de pago."*

Esa capa no existe. Este reto la construye.

---

## La solución propuesta · 03

### Una lente UX sobre Brain. Dos vistas, dos propósitos.

El UX Quality Dashboard **no construye un ETL nuevo** — lee de Brain (que ya bebe de BigQuery + Zendesk + VoC) y añade lo único que Brain no tiene: el mapeo de cada dato a una dimensión de experiencia, y el registro que vincula cada decisión de diseño con su impacto medido.

---

### Vista 1 — Salud UX: el termómetro de la experiencia

Estado permanente de las **7 dimensiones del framework Honeycomb** (Útil, Usable, Encontrable, Deseable, Creíble, Accesible, Valioso) para los tres segmentos: **B2C Web · B2C App · B2B**.

**Qué muestra por dimensión:**
- Mín. 1 métrica de negocio + mín. 2 señales de experiencia (conductual + actitudinal)
- Semáforo 🟢🟡🔴 + tendencia ↑↓→ vs mes anterior
- Badge de fuente: `Brain live` · `API live` · `sin instrumentar`
- Gap notes visibles: lo que no se mide es información, no ausencia

**El UX Health Index:**
No promedia métricas crudas (eso es estadísticamente indefendible). Agrega el **estado ordinal de las 7 dimensiones** → 0–100%. Junto al índice, un segundo número: **Cobertura X/7** — cuántas dimensiones tienen señal real. Un índice alto con cobertura baja miente; el segundo número lo impide.

**Qué permite que hoy es imposible:**
Entrar al MBR de Pilar 1 y decir: *"el NPS bajó 0,1 puntos esta semana. En ese mismo periodo, la dimensión Usable empeoró — el abandono de checkout subió y se lanzaron dos features sobre ese flujo (Gate 3). La hipótesis es que la fricción nueva en el paso de pago está detrás del movimiento."*

Eso no es causalidad demostrada. Es la hipótesis estructurada que hoy no puede formularse.

**Datos reales ya disponibles para el MVP (fuente: Brain):**
- CR Web 4,12% · AOV €135 · NPS 9,3 · CES 9,7 · CSAT B2C 94% · CSAT B2B 95%
- Perfect Memories rate 70,7% (830K reviews L12M) → señal viva para Útil
- Uninstalls iOS +63,5% vs 2025 → alerta activa en Deseable App
- NPS B2B = 35 (OKR target >8 = 0% progreso) → Deseable B2B
- VoC `#accesibilidad` + `#problemas_para_el_pago` → Accesible y Creíble vivos
- PIX Brasil + Mercado Pago México atascados → riesgo Creíble LATAM activo

---

### Vista 2 — Gate 3: el registro de decisiones de diseño

Una entrada por feature lanzada, creada por el diseñador el día del lanzamiento.

| Campo | Descripción |
|---|---|
| Feature + Jira | Nombre y enlace |
| Designer | Quién la diseñó |
| Surface | Web / App iOS / App Android / Checkout / PLP / PDP / B2B |
| Dimensión Honeycomb | Cuál de las 7 era el objetivo de la mejora |
| Hipótesis | "Si cambiamos X, esperamos que [métrica] mejore un Z% porque [razón]" |
| Métrica antes/después | Valor previo al lanzamiento → valor a los 14/30 días |
| Estado | open (pendiente de cierre) / closed |

Las entradas `open` con más de 30 días sin cerrar generan alerta automática.

**Por qué Gate 3 y no una herramienta independiente:**
Gate 3 ya existe en el modelo operativo v0.7. Hoy evalúa si una feature tuvo impacto usando únicamente métricas de negocio. Este dashboard no crea una nueva ceremonia — completa una que ya está definida añadiendo la capa UX que falta.

También responde dos blockers del PD Model v0.7:
- **H0-2** ("PD sin Gate"): el PD no firma ningún gate → esta Vista es el mecanismo de registro
- **H1-1** ("Gate de Calidad UX"): checkpoint con métricas UX antes de Gate 3 → esta Vista es ese veredicto

**Con suficientes entradas, responde preguntas hoy imposibles:**
- ¿Qué dimensión mejora más con los recursos actuales del chapter?
- ¿Qué surface tiene más deuda de experiencia acumulada?
- ¿Qué decisiones de diseño de los últimos 3 meses explican el movimiento del NPS?

---

### Capa MCP: Brain habla con Claude en el MBR

La capa MCP permite a Claude consultar el dashboard directamente desde una sesión de conversación:

> *"¿Por qué pudo caer el AOV en mayo?"*
>
> Claude responde: *"La dimensión Usable bajó 6 puntos en el mismo periodo. Se lanzaron 2 features sobre checkout (Gate 3, Solvey Prada). La señal conductual de abandono empeoró mientras la actitudinal se mantuvo — hipótesis: fricción nueva en el paso de pago, no insatisfacción general."*

Eso es el "no tenemos idea de por qué" del MBR de Marzo, resuelto en vivo.

---

## A quién da respuesta · 04

| Usuario | Qué gana |
|---|---|
| **Product Designer** | Evidencia de impacto de su trabajo, documentada con datos. Acreditable en performance review. |
| **Product Manager en Gate 3** | Correlación entre movimiento de métricas de negocio y decisiones de diseño concretas |
| **Pilar 1 en MBR** | Hipótesis estructurada cuando NPS baja, AOV cae o conversión se mueve |
| **Chapter de Diseño** | Fuente de verdad propia sobre la calidad de la experiencia que produce |
| **Claude en sesión de MBR** | Consulta directa del estado de Salud UX vía MCP, sin abrir el navegador |

---

## Enfoque técnico · 05

### Arquitectura

```
[Diseñadores] ──formulario Gate 3──►
[PM / Liderazgo] ──Vista Salud UX──►   UX Quality Dashboard    ──lee──► BRAIN
[Claude / MBR] ──MCP query──────────►  (lente UX sobre Brain)           ├─ BigQuery (CR, AOV, NR, NPS, CES)
                                                                          ├─ Zendesk (CSAT B2C/B2B 94%/95%)
                                        Lo que añade el dashboard:        └─ VoC (50 tags, sentimiento, tendencia)
                                        · Mapeo Honeycomb por métrica
                                        · Señales UX que faltan            ──api──► Optimizely · Play Console · Braze
                                        · Registro Gate 3
```

**El dashboard no hace ETL.** Coordina con el reto de Dashboard 360º App (que sí construye el ETL de Adjust/Stores/Braze/Admin) y consume sus tablas en BigQuery. Delimitación clara: ese reto construye la capa de adquisición; este reto añade la lente de experiencia encima.

### Stack

| Capa | Elección | Por qué |
|---|---|---|
| Frontend | React + Tailwind + Vercel | Deploy inmediato, ecosistema conocido |
| Datos | JSON de contrato (día 1) → conectores Brain/BigQuery (día 2) | El dashboard es data-driven: el JSON es la interfaz entre Data y Producto |
| MCP | Endpoint REST simple | Consulta desde Claude en MBR |
| Gate 3 | Formulario → Airtable / estado local | Funcional en 48h |

**Capa de adaptadores:** cada métrica declara su conector (`brain:*` | `api:*` | `cached` | `none`). Pasar de `cached` a `live` no toca la UI — solo se reapunta el conector. La demo nunca está en riesgo; las conexiones reales son upside.

### Quién hace el trabajo

**Humano:** diseñadores rellenando entradas Gate 3, juicio sobre qué dimensión Honeycomb corresponde a cada feature.

**Agente:** validación de coherencia en entradas Gate 3, síntesis del VoC de reviews, resumen de Salud UX para el MBR, detección de entradas abiertas sin cerrar pasados 30 días.

---

## Plan de implementación · 06

### Hora 0 (inicio del hackathon): acordar el JSON de contrato

El equipo acuerda en 30 minutos el shape del JSON de datos. A partir de ahí, **dos pistas paralelas que no se bloquean entre sí.**

---

### Día 1 — El dashboard funciona con datos reales precargados

**Pista Data (persona de Data):**
- Confirmar qué expone Brain vía API o BigQuery directamente
- Preguntas críticas: ¿GA4→BigQuery activo? ¿Zendesk/NPS aterrizan en BQ? ¿dbt tiene modelos de CR/AOV/NR?
- Configurar Strategic Themes del VoC en Brain (10 min — desbloquea señal de Credible y Accessible)
- Conectar Play Console para crash rate y ANR rate
- Entregar JSON de contrato poblado con datos reales de Mayo

**Pista Producto (Jota + front):**
- Dashboard completo con los 3 segmentos (B2C Web / App / B2B) y las 7 dimensiones Honeycomb
- Datos reales de Brain precargados en el JSON (CR 4,12% · AOV €135 · NPS 9,3 · CSAT 94% · Perfect Memories 70,7% · Uninstalls iOS +63,5% ...)
- UX Health Index + cobertura X/7 + tendencia ↑↓→ vs Abril
- Drill-down: Index → dimensión → métrica
- Badges de fuente por métrica (Brain live / API live / sin instrumentar)
- 2 entradas Gate 3 reales precargadas: Checkout 2 pasos (Solvey) + Upsell Free Tour→Privado

**Resultado al final del día 1:** dashboard demo-able completo aunque ningún conector esté live.

---

### Día 2 — Conectores reales + narrativa de demo

**Pista Data:**
- Encender conectores: `cached` → `brain:*` / `api:*`
- Goal: todo lo disponible en Brain/BigQuery = conexión real en la demo
- Coordinación con reto Dashboard 360º App para acceder a sus tablas de Adjust/Stores/Braze en BigQuery

**Pista Producto:**
- Reapuntar conectores a live (sin tocar UI)
- Endpoint MCP básico: lectura de estado de Salud UX + historial Gate 3
- Ensayar la narrativa de demo del MBR: *"¿por qué cayó el AOV?"* → Claude responde en vivo
- Pulir badges y gap notes de cada dimensión

**Resultado al final del día 2:** demo con conexiones reales de Brain + narrativa MCP funcionando.

---

## Métricas de éxito del reto · 07

| Métrica | Target demo |
|---|---|
| Dimensiones con dato real (no mock) | ≥ 5 de 7 en B2C Web |
| Conectores `brain:*` live | ≥ 3 (pillar/1, product-surface, cs-stats) |
| Entradas Gate 3 precargadas | 2 con datos reales |
| Narrativa MCP demo | 1 pregunta respondida en vivo |
| UX Health Index calculado | Sí, sobre datos reales de Mayo |

**Métricas de éxito post-hackathon (Q3 2026):**
- 100% de features con impacto UX lanzadas desde el chapter tienen entrada Gate 3 cerrada
- Vista Salud UX se consulta en ≥1 MBR de Pilar 1 por PI
- Dashboard usado como referencia en ≥1 sesión de Gate 3

---

## Riesgos y mitigaciones · 08

| Riesgo | Mitigación |
|---|---|
| Brain no expone API directa (solo BigQuery raw) | Persona de Data conecta BigQuery — el JSON de contrato es el buffer |
| Funnel por paso en Brain sigue en Mock Data | Lo marcamos como 🔴 en el dashboard con nota "próximamente" — no bloquea la demo |
| VoC Anécdotas B2C sin configurar | Configurar Strategic Themes en Brain antes del hackathon (10 min, Domingo Martín) |
| Solapamiento con reto Dashboard 360º App | Delimitación clara desde la hora 0: ellos construyen el ETL, nosotros consumimos sus tablas |
| Adopción del ritual Gate 3 post-hackathon | Incorporarlo al proceso de Gate 3 del modelo operativo, no solo como herramienta disponible |

---

## Equipo necesario · 09

| Perfil | Rol en el hackathon |
|---|---|
| **Head of UX (Jota)** | Criterio UX, mapeo Honeycomb, narrative Gate 3, presentación |
| **Product Designer** | Diseño del formulario Gate 3, entradas precargadas, QA visual |
| **Frontend Engineer** | React + Tailwind + conectores + MCP endpoint |
| **Data Engineer/Analyst** | Conexión Brain/BigQuery, JSON de contrato, Play Console API |

**Coordinación externa:**
- Domingo Martín → acceso Brain VoC (configurar Strategic Themes)
- Javi García → token Optimizely para CTR upsellings
- Apps Squad → Play Console service account (crash rate, ANR, ratings)

---

*Propuesta preparada para el Hackathon Civitatis 2–3 jun 2026*
*Owner: Jota Coto · jcoto@civitatis.com*
