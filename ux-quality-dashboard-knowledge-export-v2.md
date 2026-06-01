# Knowledge Export v2.1 — UX Quality Dashboard
## Sesión 1 jun 2026 · grill-me + exploración completa de Brain

> Documento de continuidad. Recoge todo el conocimiento generado en la sesión del 1 jun 2026.
> Sustituye al knowledge-export v1 (sin Brain) y al v2 inicial (Brain explorado parcialmente).
> Esta versión incluye la exploración completa de todas las secciones de Brain:
> Executive (todas las subsecciones), Product Surface (Web/App/API), Supply, Impact,
> WBR, Business Events, Customer Service Stats, VoC B2C (configuración + anécdotas), VoC B2B,
> OKR Canvas. Datos corregidos vs documentos anteriores.

---

## 1. EL HALLAZGO QUE LO REORDENA TODO: Brain ya es la capa de datos

Acceso directo a `https://brain.civitatis.tech` el 1 jun 2026 (vía navegador con sesión VPN).
Brain **ya integra** lo que el dashboard pensaba conectar:

| Sección de Brain | URL | Qué expone | Estado |
|---|---|---|---|
| Sección de Brain | URL | Qué expone (datos Mayo 2026) | Estado |
|---|---|---|---|
| **Executive · BigQuery** | `/analytics` | CR B2C **4,12%** · AOV **€135** · NR **€1,03M** semanal · NPS **9,3** · CES **9,7** · Usuarios 58.980 · Sesiones Web 1,64M / App 335K · NR FT 190K / PT 843K | 🟢 Auto-actualizado |
| **Executive → Funnels** | `/analytics/executive/funnels` | Funnel Web+App (Acquisition→Discovery→Consideration→Purchase→Conversion) | ⚠️ **Mock Data (Design Mode) — no live** |
| **Executive → Perfect Memories** | `/analytics/executive/perfect-memories` | Rate 5★: **70,7%** global · 60,5% L12M · 830K reviews · 3,14M PM 2026 · histórico desde 2010 | 🟢 Live |
| **Executive → By Market** | `/analytics/executive/production/market` | Revenue por zona geográfica (Last Updated: Live) | 🟢 Live |
| **Product Surface → Overview** | `/analytics/executive/production/device` | NR total **€7,23M** · Web €5,61M (-5,4%) · App **€1,32M** (+8,1%) · API **€310K** (+27,5%) | 🟢 Live (usar Mayo) |
| **Product Surface → App → Installs** | `/analytics/executive/production/device/App?section=installs` | iOS installs **34.946** (↓20,1%) · Android **98.265** (↓7,9%) · **iOS uninstalls 19.187 (+63,5% ⚠️)** · Active Android **856.026** (+32%) | 🟢 Live |
| **Product Surface → App → User Behaviour** | `/analytics/executive/production/device/App?section=behaviour` | Ratio trip→reserva **90,8%** · Trips con reserva 265.618 · **Favoritos→conversión 0,0% ⚠️** · MAU/new-users pendientes | 🟢 Live (parcial) |
| **Pilar 1 deep-dive** | `/analytics/pillar/1` | Serie semanal W11–W22 de NPS/CES/AOV/NR/Sesiones/CR · OKRs con progreso | 🟢 Live |
| **Impact Tracker** | `/impact` | OKRs todos los pilares · CI-1634: NPS 0% · AOV 0% ⚠️ · CI-1781: NPS B2B 0% ⚠️ | 🟢 Live |
| **WBR** | `/mechanisms/wbr` | Revenue semanal + deliveries por squad (próximos 10 días / pasados 14) + highlights/callouts | 🟢 Live |
| **Business Events** | `/analytics/executive/business-events` | Log de incidentes SEV1/SEV2 + Feature Launches histórico · Timeouts calendario (29 may) · Errores pago (mar 2026) | 🟢 Live |
| **CS Stats ↔ Zendesk** | `/contacts/customer-service/stats` | Tickets Mayo: B2C **6.360** · B2B **1.206** · OPS 1.659 · CSAT: B2C **94%** · B2B **95%** · OPS 100% | 🟢 Live (Sync Zendesk manual) |
| **VoC → Configuración** | `/contacts/voc/configuration` | **50 tags Zendesk** mapeados por grupos temáticos. ⚠️ Strategic Themes vacíos — anécdotas no filtran hasta que se configuren | 🟡 Taxonomía lista, config pendiente |
| **VoC B2B** | `/contacts/voc/b2b` | NPS B2B: **35** (escala 0–100) · Hot topics: Invoicing (sentiment 40 negativo) · API (75 positivo) · Pain point #1: Invoice Generation | 🟢 Live (datos limitados) |
| **OKR Canvas** | `/okr-canvas` | Jerarquía completa OKRs 2026, filtrable por equipo. NPS B2B OKR: >8 · 0% progreso ⚠️ | 🟢 Live |

**Datos corregidos respecto a documentos anteriores:**
- ✅ **CSAT B2C: 94%** (no 87,7% — ese dato era de encuesta Tableau diferente)
- ✅ **CES: 9,7** (no 9,64)
- ✅ **NPS B2C: 9,3** (no 9,36)
- ✅ **AOV: €135 semanal** (€137,7 era el dato del MBR de Marzo, mes cerrado)
- ✅ **CR web: 4,12%** (no 3,7%)
- ✅ **Funnel Brain = Mock Data** — no disponible para el hackathon
- ✅ **B2B tiene señales reales**: NPS 35, CSAT 95%, tickets 1.206, pain point facturación
- ✅ **Perfect Memories = señal live para Useful** (no gap)

**Consecuencias estratégicas (sin cambio):**
1. El dashboard **NO construye ETL**. Es **la lente UX sobre Brain**.
2. Toda la columna **Negocio** nace 🟢 *live* sin trabajo de integración.
3. **Accessible deja de ser cero:** tag `#accesibilidad` vivo, pero requiere configurar Strategic Themes.
4. El riesgo de governance queda **muy mitigado**: se lee de Brain, no de cada herramienta suelta.

---

## 2. Decisiones cerradas (grill-me)

1. **Vista 1 = dos columnas por dimensión** (Negocio vs Experiencia). El gap entre ambas es el producto.
2. **Regla de dimensión:** mín. 1 Negocio + mín. 2 Experiencia.
3. **Clasificador de 3 etiquetas:** Clase · Madurez (🟢🟡🔴) · Señal (Conductual/Actitudinal).
4. **UX Health Index** sobre estado de dimensiones + **Cobertura X/7** + tendencia ↑↓→ con drill-down a métrica.
5. **Dos cortes reales** (Abril guardado vs Mayo) → tendencia de verdad.
6. **No se mantiene, se consulta:** auto-pull al cargar (ahora trivial: Brain ya tiene los datos).
7. **Tres anillos de acceso** → reemplazado en gran parte: la mayoría es `brain:*` directo.
8. **BigQuery-first vía Brain**, resto por API. Capa de adaptadores: `brain:*`|`api:*`|`cached`|`none`.
9. **48h en dos pistas** (Data / Producto) con JSON de contrato como interfaz, acordado en hora 0.
10. **MCP:** A se construye · B narrativa de demo (correlación AOV↔UX↔VoC en vivo) · C fuera.
11. **Set ideal = filtro de 3 condiciones** (qué mide / instrumento nombrable / tipo de señal). Un artefacto, dos vistas: realista = ideal filtrado por disponibilidad.
12. **Jerarquía:** Segmento → Superficie (filtro) → 7 dimensiones → KPIs. Globales con `surface:global`.

---

## 3. Tracción de chapter (por qué importa más allá del hackathon)

El PD Model v0.7 roadmap ya lo pide:
- **H0-2 "PD sin Gate"** (blocker) → la Vista 2 (Gate 3) es el mecanismo de registro.
- **H1-1 "Gate de Calidad UX antes de Gate 3"** (riesgo alto) → la Vista 1 es el veredicto estructurado.

---

## 4. KPIs por dimensión — hallazgos y correcciones

### B2C Web (datos corregidos y enriquecidos)
- **Useful:** Perfect Memories rate 70,7% (🟡 live `brain:executive/perfect-memories`) — antes 🔴 gap. Cero-results buscador sigue sin instrumentar.
- **Usable:** CR 4,12% (no 3,7%). Funnel Brain en **Mock Data** — abandono checkout (77,19%) es dato precargado, no live. CES global 9,7 disponible en `brain:analytics`.
- **Findable:** Buscador Typesense activado al 100% (CIVI-3262, 21 may) sin métricas — candidato urgente para Gate 3.
- **Desirable:** NPS 9,3 (target >9,4 = 0% OKR ⚠️) · **CSAT real = 94%** (Zendesk/Brain, no 87,7%).
- **Credible:** PIX Brasil + Mercado Pago MX atascados (CIVI-2092/2089) — riesgo Credible LATAM activo. VoC tags `#problemas_para_el_pago` + `#informacion_erronea_en_web` disponibles.
- **Accessible:** VoC `#accesibilidad` vivo pero **requiere configurar Strategic Themes** (10 min, Domingo Martín). Sin eso, la card no muestra anécdotas.
- **Valuable:** AOV €135 (target €142 = 0% OKR ⚠️) · LTV €30,2 (target €32,5 = 25% OKR).

### B2C App (datos nuevos de Brain User Behaviour)
- **Useful:** NR App €1,32M (+8,1%) 🟢. Installs iOS ↓20,1%, Android ↓7,9%.
- **Usable:** Ratio trip→reserva **90,8%** (🟢 dato Brain). **Favoritos→conversión 0,0% ⚠️** — la feature de favoritos no aporta a conversión (265.618 trips con reserva, solo 25 con favoritos previos). ANR rate 0,52% > umbral Play Store 0,47% ⚠️.
- **Desirable:** Uninstalls iOS **+63,5%** vs 2025 ⚠️ (ratio uninstall/install = 55%). Ratings 2,9 ⚠️.
- **Credible:** CSAT B2C global 94% (sin filtro canal app). CSAT canal app sigue sin segmentar.
- El Funnel `init_app`→compra existe en Brain pero está en **Mock Data** — no live para el hackathon.

### B2B (mucho más datos de lo esperado)
- **NPS B2B:** 35 (escala 0–100) vía `brain:voc/b2b`. OKR target >8 (escala 0–10) = 0% progreso ⚠️.
- **CSAT B2B:** 95% (superior al B2C) vía `brain:cs-stats`. Divergencia NPS/CSAT = hallazgo de diseño.
- **Pain point #1:** Facturación (sentiment 40, negativo). "Mi factura no muestra el VAT number." No es usabilidad del panel.
- **NR Agencias:** €2M (+6,3%) · **NR API:** €310K (+27,5%). B2B crece mientras Web B2C cae.
- **NPS post-primera compra AGE (CIVI-2981):** Ready to Launch (retrasado desde 29 may) — cuando active, primera señal NPS B2B real.

---

## 5. Proyecto Growth paralelo (a coordinar, no duplicar)

Otro reto del hackathon propone un **ETL automático** (Apache NiFi / Fivetran / Zapier → PostgreSQL/BigQuery → Tableau/Power BI) que extrae de **Adjust, App/Play Store, Braze, Admin** 2×/mes, con KPIs:
- Performance: Installs, Reinstalls, Uninstalls, Coste
- CRM: usuarios únicos, DAU, MAU
- Producto: funnel `init_app`→compra, Ticket Medio, LTV, CAC
- + alertas de anomalías

**Delimitación:** ese proyecto construye el **ETL de adquisición/App**; nuestro dashboard lo
**consume** y añade la lente de experiencia (Honeycomb, señales UX, Gate 3). Sus fuentes son
fiables → usar como conector `api:*` para B2C App. Coordinar acceso a sus tablas en BigQuery.

---

## 6. Estado de los artefactos

> Directorio: `~/Documents/Claude/Projects/Hackathon/Dashboard-UX/`
> Última auditoría: 1 jun 2026.

| Archivo | Estado | Uso principal |
|---|---|---|
| `ux-quality-dashboard.html` | **v0.3** ✅ | Dashboard funcional data-driven · 3 segmentos · conectores `brain:*` · datos reales Mayo 2026 · KPIs actualizados (Perfect Memories, Uninstalls iOS, favoritos 0%, NPS B2B, CSAT B2B) |
| `ux-quality-dashboard-prd-v2.md` | **v2.1** ✅ | PRD completo: §6 tablas de KPIs con datos Brain · §14 modelo de datos TypeScript · §15 requisitos no funcionales · §16 alcance MVP en/fuera scope |
| `ux-quality-dashboard-model-v2.md` | **v2** ✅ | Modelo de medición: decisiones cerradas · clasificador 3 etiquetas · Health Index · tablas KPIs actualizadas con conectores `brain:*` · B2B resumen · JSON contrato · plan 48h |
| `ux-quality-dashboard-knowledge-export-v2.md` | **v2.1** ✅ | Este documento — continuidad de sesión con todo el conocimiento generado |
| `brain-data-map.md` | ✅ | Mapa operativo de datos Brain: dónde, quién, cómo extraer, dimensión Honeycomb. 97 KPIs. Para el equipo del hackathon. |
| `brain-data-map-viewer.html` | ✅ | Viewer HTML interactivo del brain-data-map — filtrable por segmento/dimensión/conector. Para consultar durante el hackathon. |
| `hackathon-proposal-v2.md` | ✅ | Propuesta markdown completa con datos reales, plan por días, métricas de éxito, equipo, riesgos. |
| `hackathon-proposal.html` | ✅ | One-pager visual (Odisea design system) de la propuesta — para presentar en el hackathon. |
| `hackathon-brain-panel.md` | ✅ | Versión texto para pegar en Brain (panel de retos del hackathon). Formato igual al [CURRENT] original. |

---

## 7. Próximos pasos

### Antes del hackathon (hoy / mañana temprano)
- [ ] **Configurar Strategic Themes en Brain VoC** (`/contacts/voc/configuration`) — 10 min · Owner: Domingo Martín. Sin esto, las anécdotas B2C no filtran por tema.
- [ ] Confirmar con owner de Brain: ¿hay API/endpoint para leer métricas, o vamos a BigQuery directo?
- [ ] Coordinar con el equipo Growth el acceso a sus tablas BigQuery del ETL App.
- [ ] Sacar el corte de **Abril** en Brain (filtrar por mes: Apr 2026) para alimentar la tendencia real del JSON.

### Hora 0 del hackathon (30 min)
- [ ] Acordar JSON de contrato con persona de Data (shape + valores Mayo precargados).
- [ ] Arrancar las **dos pistas paralelas**: Data (Brain/BigQuery) + Producto (dashboard contra JSON).
- [ ] Briefar con `brain-data-map.md`: dónde está cada dato y quién tiene acceso.

### Durante el hackathon
- [ ] Pista Data: confirmar GA4→BigQuery activo, conectar Play Console, VoC programático.
- [ ] Pista Producto: 2 entradas Gate 3 reales precargadas (Checkout 2 pasos; Upsell Free Tour→Privado).
- [ ] Narrativa MCP: ensayar *"¿por qué cayó el AOV?"* con Claude respondiendo en vivo.

### Post-hackathon (roadmap)
- [ ] Instrumentar gaps Findable: 3 eventos GA4 (`search_no_results`, `search_result_clicked`, `form_error`).
- [ ] Activar filtro `channel=app` en Zendesk para CSAT canal app.
- [ ] Reviews App clusterizadas mensualmente (App Store Connect API + Google Play API + Claude).
- [ ] Accessible técnico: Lighthouse CI + axe-core + VoiceOver/TalkBack mensual.
- [ ] Ownership por métrica → PDIs del chapter.
- [ ] B2B deep-dive: usuario real · flujo de reserva · adopción buscador IA · NPS post-primera compra AGE.

---

## 8. Datos reales capturados de Brain (semana 25-31 may 2026)

CR B2C 4,12 % (-0,9 %) · AOV 135€ (+4,8 %) · NR B2C 1.034.626€ (+1,3 %) · NPS 9,3 · CES 9,7 ·
Satisfacción 9,3 (+0,3 %) · Usuarios 58.980 (-4,1 %) · Sesiones 1.979.259 (-5,5 %; Web 1.643.902 /
App 335.357) · Sesiones con compra 81.492 (-6,4 %) · NR FT 190.799€ / PT 843.827€.

> Nota: estos son los valores vivos del widget Executive. Para el corte mensual cerrado de Mayo y
> el de Abril (tendencia), filtrar por mes en Brain antes de poblar el JSON de contrato.

---

---

## 9. Evidencias de Confluence (verificadas, no inferidas)

> Trasladadas del knowledge-export v1. Citas literales de páginas leídas en sesión anterior.

**Sobre el buscador web:**
> *"Solamente tenemos el dato de las sesiones, no tenemos el dato de los clics por lo que no podemos tener un CTR real de lo que se ha conseguido este año."*
> — Pág. 1875673092, "Acquisition & Discovery Web: Visión, Scope y Plan del Equipo", §4.1, feb 2026. Autor: Andres Spitzer (WAVE).

**Sobre medición del buscador:**
> *"No lo tenemos medido actualmente — crear flujo para medir eventos."*
> — Mismo documento, §4.2, columna "Fuente", fila del buscador.

**Sobre inconsistencia de medición en GA4:**
> *"Porque ahora mismo la medición es inconsistente, dificultades para sacar resultados y priorizar, no tenemos buena medición de la PLP antigua."*
> — Mismo documento, §6, fila "Limpieza de medición".

**Sobre el gap institucional de métricas UX:**
- Pág. 1636007947 — "Metricas y Dashboards" (Hito #13, DP space): **página vacía**. Owner: Jota Coto.
- Pág. 864616450 — "Accessibility" (Hito #11, DP space): **página vacía**. Owner: Juan Cobo.

**OKR con valor cero (patrón de 4 MBRs consecutivos):**
- CI-1683: Puntuación media actividades = 0/9 en todos los MBRs
- CI-2043, CI-2048, CI-2049: CSAT por región (ES/IT/BR) = 0/100 en todos los MBRs

---

## 10. Rationale del framework (por qué Honeycomb)

**Elegido:** Honeycomb (Morville) como columna vertebral + HEART (Google) como clasificador del tipo de medición dentro de cada dimensión.

**Descartados:**
- **Garrett 5 Elementos:** demasiado procesual (capas de diseño, no KPIs de experiencia)
- **AttrakDiff/Hassenzahl:** queda implícito en Honeycomb (Desirable cubre lo hedónico)
- **Fogg BM:** útil como diagnóstico interno, no como KPI medible
- **DEC/Asociación DEC:** orientado a CX global, no a UX digital por feature

**Por qué Honeycomb:** es el único framework que cubre explícitamente las 7 dimensiones relevantes para Civitatis incluyendo **Accessible** y **Credible**, que son gaps críticos documentados (EAA junio 2025, confianza en proceso de pago LATAM).

---

## 11. Textos clave aprobados (session Cowork, versión definitiva)

**Problem statement (versión final aprobada):**

> Cuando un diseñador pregunta si lo que ha diseñado "¿mejoró la experiencia?", la respuesta honesta es nadie lo sabe con certeza porque los datos que lo responderían están fragmentados entre GA4, Zendesk, Clarity, Adjust y los informes de Data — y nadie los conecta a la decisión de diseño concreta que los provocó — o directamente no existen (no hay ninguna métrica de accesibilidad).
>
> Todo lo que lanzamos a producción se evalúa con métricas de negocio (NR en reporting de Data, datos de conversión en GA4, NPS en Tableau detrás de 2FA compartido, CSAT en Zendesk) pero la mayoría como promedios globales sin desglose por feature o surface (web/app), por momento del journey o por feature lanzada y no existe ningún artefacto donde ver una capa de evidencia UX.
>
> La conexión que falta tiene esta forma: `feature lanzada → diseñador responsable → dimensión UX intervenida → métrica antes/después`.

**Resumen de la solución (versión aprobada):**

> El UX Quality Dashboard es una capa de evidencia UX gestionada por el chapter de diseño, integrada en el modelo operativo existente de Civitatis, que hace dos cosas que hoy no hace ningún artefacto: mantener un termómetro permanente de la salud de la experiencia y registrar el impacto concreto de cada decisión de diseño en el momento en que se lanza a producción.

---

## 12. Páginas de Confluence clave (IDs para referencia rápida)

CloudId Civitatis: `53c3ec94-9a0b-42ce-8e81-41719567106f`

| ID | Título | Space | Relevancia |
|---|---|---|---|
| 1875673092 | Acquisition & Discovery Web: Visión, Scope y Plan | WAVE | Fuente de los 3 quotes sobre buscador y medición |
| 1636007947 | Metricas y Dashboards (Hito #13) | DP | Página vacía — confirma el gap institucional |
| 864616450 | Accessibility (Hito #11) | DP | Página vacía — confirma zero en accesibilidad |
| 1596850207 | CES y NPS | DP | Confirma NPS/CES en Tableau |
| 2392850433 | Software Tools Index | Comex | Inventario completo de herramientas con owners |
| 2661844455 | Mayo 2026 — Pilar 1 MBR | Comex | MBR más reciente (28/05/2026) |
| 2544795654 | Abril 2026 — Pilar 1 MBR | Comex | MBR anterior |
| 2432106523 | Marzo 2026 — Pilar 1 MBR | Comex | MBR con el ejemplo AOV 155→135€ |
| 2649718789 | A/B Testing en Apps — Guía Operativa Optimizely | PAPPS | Confirma eventos trackeados en app |
| 2663088154 | Android · Análisis bajón descargas mayo 2026 | PAPPS | UAD/UAM 7.58%, ratings 2.9, ANR rate |
| 2232123404 | Releases 2026 | PAPPS | Historial de versiones con Braze, Clarity, Adjust, Optimizely |

---

*Knowledge Export v2.1 — Claude (Cowork) + Jota Coto · 1 jun 2026*
*Brain explorado en su totalidad: Executive (todas las subsecciones) · Product Surface · Supply · Impact · WBR · Business Events · CS Stats · VoC B2C/B2B · OKR Canvas*
*CloudId Confluence: 53c3ec94-9a0b-42ce-8e81-41719567106f*
