# Brain Data Map — Hackathon UX Quality Dashboard
## Dónde está cada dato, quién lo tiene y cómo extraerlo

> Generado el 1 jun 2026 a partir de exploración directa de `brain.civitatis.tech`.
> Uso: el equipo del hackathon consulta este doc para saber exactamente dónde encontrar
> cada métrica, quién tiene acceso y qué conector usar en el dashboard.
>
> Conectores: `brain:*` = leer de Brain (que ya bebe de BigQuery/Zendesk) ·
> `api:*` = llamar API directa · `none` = no existe aún

---

## 1. Segmento B2C Web

### Dimensión 🎯 Useful

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| NR alternativas sin disponibilidad | +190% YoY | Analytics → Dashboard Pilar 1 | Andres Spitzer (WAVE) | BigQuery vía Brain API | `brain:pillar/1` |
| Perfect Memories rate (5★/total) | 70,7% global · 60,5% L12M | Analytics → Executive → Perfect Memories | Arturo Moreno (Supply) | BigQuery histórico desde 2010 | `brain:executive/perfect-memories` |
| Perfect Memories 2026 | 3,14M generadas | Misma vista | Arturo Moreno | Ídem | `brain:executive/perfect-memories` |
| Total reviews L12M | 830K | Misma vista | Arturo Moreno | Ídem | `brain:executive/perfect-memories` |
| Zero-results buscador web | No existe | — | — | Evento GA4 `search_no_results` (pendiente de instrumentar) | `none` |

### Dimensión 🖱️ Usable

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| CR B2C global | 4,12% (-0,9%) | Dashboard → Pilar 1 (semanal/mensual) | Javi García (CI-1634) | BigQuery | `brain:pillar/1` |
| Sesiones con compra | 81.492 (-6,4%) | Misma vista | Javi García | BigQuery | `brain:pillar/1` |
| Sesiones Web total | 1.643.902 | Dashboard → Pilar 1 | Javi García | BigQuery | `brain:pillar/1` |
| Abandono checkout por paso | Estructura existe (mock) | Analytics → Executive → Funnels | Javi García / Solvey Prada | BigQuery (dato no live aún — Funnels en Design Mode) | `none` (próximamente `brain:executive/funnels`) |
| Ratio PLP→PDP | 57% (OKR CI-2112) | OKR Canvas / Impact | Solvey Prada (FLOW) | BigQuery | `brain:pillar/1` |
| Tasa de error en formularios | No existe | — | — | Evento GA4 `form_error` (pendiente) | `none` |
| CES global | 9,7 (Dashboard) / 10 (Pilar 1 semanal) | Dashboard ejecutivo + Pilar 1 | Domingo Martín / Javi García | BigQuery / Encuesta | `brain:pillar/1` |

> ⚠️ Nota: el Funnel completo por paso (Acquisition→Discovery→Consideration→Purchase→Conversion)
> existe en Brain (`/analytics/executive/funnels`) con la estructura correcta para Web y App,
> pero está en **Mock Data (Design Mode)**. Cuando se active con datos reales será la fuente
> principal de Usable sin necesidad de GA4 directo.

### Dimensión 🔍 Findable

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| Posición media CAT Landings | 5,1 (era 17,8) | OKR Canvas / Impact | Hector Castillo (WAVE/SEO) | BigQuery / GSC | `brain:pillar/1` |
| Buscador Typesense activado | 100% (CIVI-3262, lanzado 21 may) | WBR → Deliveries (OTROS) | Hector Castillo | — (feature lanzada, sin métricas de impacto aún) | Gate 3 pendiente |
| CTR buscador interno | No existe | — | — | Evento GA4 `search_result_clicked` (pendiente) | `none` |
| CTR upsellings | 4,4% (solo en A/B activos) | — | Javi García (Optimizely) | api:optimizely | `api:optimizely` |
| Sesiones orgánicas | 561.940 (-1% vs LY) | Dashboard → Pilar 1 | Hector Castillo | BigQuery | `brain:pillar/1` |

### Dimensión 💜 Desirable

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| NPS B2C | 9,3 (target >9,4 = **0% progreso** ⚠️) | Dashboard Pilar 1 + OKR CI-1634 | Javi García | BigQuery / encuesta NPS | `brain:pillar/1` |
| Recurrent customers | 31.254 / semana (+8% vs LY) | Dashboard → Pilar 1 | Javi García | BigQuery | `brain:pillar/1` |
| Satisfacción global | 9,3 (+0,3% vs semana anterior) | Dashboard ejecutivo | Javi García | BigQuery | `brain:analytics` |
| Temas VoC negativos recurrentes | Taxonomía completa disponible (50 tags) | Contacts → VoC → Configure | Domingo Martín (Customer Care) | Brain VoC API / Zendesk | `brain:voc` |
| VoC Anécdotas B2C | Infraestructura lista, **Strategic Themes sin configurar** | Contacts → VoC → B2C Anecdotes | Domingo Martín | Configurar temas en `/contacts/voc/configuration` (10 min) | `brain:voc` |

### Dimensión 🛡️ Credible

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| CSAT B2C | **94%** (+0,3pp vs año anterior) | Contacts → Customer Service Stats → Mayo | Domingo Martín | Brain CS Stats API / Zendesk | `brain:cs-stats` |
| Volumen tickets B2C | 6.360 (Mayo) | Misma vista | Domingo Martín | Ídem | `brain:cs-stats` |
| CES | 9,7 | Dashboard ejecutivo | Domingo Martín / Javi García | BigQuery | `brain:analytics` |
| Tag VoC `#problemas_para_el_pago` | Existe en taxonomía | VoC → Configure | Domingo Martín | Zendesk tag → Brain VoC | `brain:voc` |
| Tag VoC `#informacion_erronea_en_web` | Existe en taxonomía | VoC → Configure | Domingo Martín | Zendesk tag → Brain VoC | `brain:voc` |
| PIX Brasil (Mercado Pago MX) | **Atascado** 🔴 (CIVI-2092, CIVI-2089) | WBR → Deliveries → Payments & Billing | Hector Castillo | — (incidencia activa) | Señal de riesgo Credible LATAM |
| Auth Rate LATAM | 82% (target 85%) | OKR / Brain | — | BigQuery | `brain:pillar/1` |
| Abandono en pago + motivo | No existe | — | — | GA4 + survey de salida (pendiente) | `none` |

### Dimensión ♿ Accessible

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| Tag VoC `#accesibilidad` | Existe en taxonomía | VoC → Configure → grupo "actividades" | Domingo Martín | Zendesk tag → Brain VoC | `brain:voc` |
| Tag VoC `#idioma` | Existe en taxonomía | Misma vista | Domingo Martín | Ídem | `brain:voc` |
| Lighthouse a11y score | No existe | — | Juan Cobo (Design Ops) | Lighthouse CI (pendiente de instrumentar) | `none` |
| % elementos ARIA / alt text | No existe | — | Juan Cobo | axe-core en CI (pendiente) | `none` |

### Dimensión 💰 Valuable

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| NR B2C total (semanal) | €1,034M (+1,3%) | Dashboard → Pilar 1 | Javi García | BigQuery | `brain:pillar/1` |
| NR Web Mayo | €5,608M (-5,4% vs LY) | Product Surface → Website | Arturo Moreno | BigQuery | `brain:product-surface/website` |
| AOV B2C | €135 (+4,8%) | Dashboard Pilar 1 (semanal) | Javi García (target €142 = **0%** ⚠️) | BigQuery | `brain:pillar/1` |
| LTV 12 meses | €30,2 (target €32,5 = 25% ⚠️) | OKR CI-1849 | Laura Llamas | BigQuery | `brain:impact` |
| Feature Adoption Rate | No existe | — | — | GA4 (% uso feature nueva en 4 semanas) | `none` |

---

## 2. Segmento B2C App

### Dimensión 🎯 Useful

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| Installs iOS | 34.946 (↓20,1% vs 2025) | Product Surface → App → Installs & Devices | Laura Sánchez-Crespo (Apps Squad) | Brain App API / Adjust | `brain:product-surface/app` |
| Installs Android | 98.265 (↓7,9% vs 2025) | Misma vista | Laura Sánchez-Crespo | Ídem | `brain:product-surface/app` |
| Active users iOS | 110.649 (↑1412% — app nueva desde ene 2026) | Misma vista | Laura Sánchez-Crespo | Ídem | `brain:product-surface/app` |
| Active users Android | 856.026 (↑32%) | Misma vista | Laura Sánchez-Crespo | Ídem | `brain:product-surface/app` |
| Sesiones App | 335.357 (+9% vs LY) | Dashboard Pilar 1 | Javi García | BigQuery | `brain:pillar/1` |
| Zero-results buscador in-app | No existe | — | — | Evento GA4 app (pendiente) | `none` |

### Dimensión 🖱️ Usable

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| CR App | 6,73% | Dashboard Pilar 1 / OKR | Javi García | BigQuery | `brain:pillar/1` |
| Ratio usuario con trip → reserva | **90,8%** | Product Surface → App → User Behaviour | Laura Sánchez-Crespo | BigQuery | `brain:product-surface/app` |
| Ratio trip → reserva | **88,8%** | Misma vista | Laura Sánchez-Crespo | Ídem | `brain:product-surface/app` |
| Trips con reserva Mayo | 265.618 | Misma vista | Laura Sánchez-Crespo | Ídem | `brain:product-surface/app` |
| Trips con favoritos previos | **25 de 265.618 (0,0%)** ⚠️ | Misma vista — "Flujo de trips con reserva" | Laura Sánchez-Crespo | BigQuery | `brain:product-surface/app` |
| Funnel por paso App | Estructura existe (mock) | Executive → Funnels → App Funnel | Javi García | Mock — igual que Web, no live aún | `none` |
| MAU / New users | **Pendiente** ("data source not yet wired") | Product Surface → App → User Behaviour | Laura Sánchez-Crespo | — | `none` |
| Crash rate | 0,21% (pico 1,4% el 13 may) | Play Console (no en Brain) | Apps Squad | api:playconsole | `api:playconsole` |
| ANR rate | 0,52% ⚠️ (umbral Play Store: 0,47%) | Play Console | Apps Squad | api:playconsole | `api:playconsole` |
| A/B Test Login Obligatorio vs Guest Checkout | En curso (CIVI-2684) | WBR → Apps Squad | Laura Sánchez-Crespo | Optimizely | `api:optimizely` |

> ⚠️ Dato crítico: **los favoritos no aportan a conversión** (100% de trips con reserva
> reservaron sin tener favoritos guardados). La migración de 860.000 favoritos en abril 2026
> infla los datos de ese mes — usar datos desde mayo en adelante.

### Dimensión 🔍 Findable

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| **Favoritos → conversión** | **0,0%** ⚠️ (de 265.618 trips con reserva, solo 25 tenían favoritos) | Product Surface → App → User Behaviour → "Flujo de trips con reserva" | Laura Sánchez-Crespo | Brain App API / BigQuery | `brain:product-surface/app` |
| CTR buscador in-app | No existe | — | — | Evento GA4 app (pendiente) | `none` |
| Zero-results in-app | No existe | — | — | Evento GA4 app (pendiente) | `none` |
| A/B Test filtros PLP App (CIVI-2706) | En curso | WBR → Apps Squad | Laura Sánchez-Crespo | Optimizely | `api:optimizely` |
| A/B Test layout PLP 4 variantes (CIVI-2693) | Lanzado 24 may | WBR → Apps Squad | Laura Sánchez-Crespo | Optimizely | `api:optimizely` |

> ⚠️ La función de favoritos es la señal Findable más crítica de la App: es la principal herramienta de descubrimiento y guardado, y no aporta NADA a la conversión. La migración de 860K favoritos en abril infla ese mes — usar datos desde mayo.

### Dimensión 💜 Desirable

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| Uninstalls iOS | 19.187 (↑**63,5%** vs 2025 ⚠️) | Product Surface → App → Installs & Devices | Laura Sánchez-Crespo | Brain App API / Adjust | `brain:product-surface/app` |
| Uninstalls Android | 83.746 (↓1,1%) | Misma vista | Laura Sánchez-Crespo | Ídem | `brain:product-surface/app` |
| App ratings Play Store | ~2,9 ⚠️ (benchmark: 4,7–4,9) | Play Console | Apps Squad | api:playconsole | `api:playconsole` |
| NPS segmento app | No existe como segmento | — | Javi García | Configurar filtro canal en encuesta NPS | `none` |
| Push engagement (Braze) | Activo desde v4.6.0 | — | Javi García | api:braze | `api:braze` |
| Usuarios recurrentes / reactivados | 31.254 / 4.249 (semana) | Dashboard Pilar 1 | Laura Llamas / Javi García | BigQuery | `brain:pillar/1` |

### Dimensión 🛡️ Credible

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| CSAT canal App | No segmentado por canal | CS Stats muestra B2C global 94% sin filtro canal | Domingo Martín | Zendesk → filtro `channel=app` (pendiente de configurar) | `none` → `brain:cs-stats` |
| Solicitud reviews (CIVI-2660) | Lanzado 24 may | WBR → Apps Squad | Laura Sánchez-Crespo | Play Console | `api:playconsole` |

### Dimensión ♿ Accessible

| Dato | Valor | Dónde | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| % pantallas con accessibilityLabel / contentDescription | No existe | — | Juan Cobo | Xcode / Android Studio (pendiente) | `none` |
| Dynamic Type / font scaling | No existe | — | Juan Cobo | Test manual | `none` |
| VoiceOver / TalkBack | No existe | — | Juan Cobo | Test manual mensual | `none` |

### Dimensión 💰 Valuable

| Dato | Valor Mayo 2026 | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| NR App Mayo | €1,317M (+8,1% vs LY) | Product Surface → App | Laura Sánchez-Crespo / Javi García | BigQuery | `brain:product-surface/app` |
| NR por canal App: Orgánico | €525K (-25,4%) | Misma vista | Isma García | BigQuery | `brain:product-surface/app` |
| NR por canal App: Marketing | €426K (+35,3%) | Misma vista | Isma García | BigQuery | `brain:product-surface/app` |
| NR por canal App: Retención | €310K (+110,4%) | Misma vista | Laura Llamas | BigQuery | `brain:product-surface/app` |
| LTV 12M | €30,2 (target €32,5) | OKR CI-1849 | Laura Llamas | BigQuery | `brain:impact` |
| Churn | 30% progreso (target <40,5%) | OKR CI-1849 | Laura Llamas | BigQuery | `brain:impact` |

---

## 3. Segmento B2B — Panel Agencias y Afiliados

### Estado general: casi todo 🔴 sin métricas de experiencia

| Dato | Valor | Dónde en Brain | Responsable | Cómo extraer | Conector |
|---|---|---|---|---|---|
| NR Agencias Mayo | €2M B2B Web (+6,3% vs LY) | Product Surface → Website → B2B Channels | Lucía Delgado | BigQuery | `brain:product-surface/website` |
| NR API (afiliados) | €310K (+27,5% vs LY) | Product Surface → API | Lucía Delgado | BigQuery | `brain:product-surface/api` |
| Nº agencias activas | 21.600 (target 26.000 = 62%) | OKR CI-1781 / OKR Canvas | Lucía Delgado | BigQuery | `brain:impact` |
| NPS B2B (OKR objetivo) | **>8 · 0% progreso** ⚠️ | OKR Canvas → CI-1781 | Lucía Delgado | Encuesta NPS B2B (pendiente de activar) | `none` |
| NPS B2B (VoC) | **35** (escala 0–100) | Contacts → VoC → B2B | Lucía Delgado / Domingo Martín | Brain VoC B2B | `brain:voc/b2b` |
| CSAT B2B | **95%** | Contacts → CS Stats → Mayo | Domingo Martín | Brain CS Stats / Zendesk | `brain:cs-stats` |
| Tickets B2B Mayo | 1.206 | Misma vista | Domingo Martín | Ídem | `brain:cs-stats` |
| Pain point #1 B2B | **Facturación** (sentiment 40, negativo) | VoC B2B → Hot Topics | Lucía Delgado | Brain VoC B2B | `brain:voc/b2b` |
| Pain point #2 B2B | API Rate Limits (sentiment 75, positivo) | Misma vista | Lucía Delgado | Brain VoC B2B | `brain:voc/b2b` |
| NPS post-primera compra AGE | Ready to Launch (CIVI-2981) ⚠️ Retrasado | WBR → B2B Revenue & Sales Ops | Lucía Delgado | — (pendiente de lanzamiento) | `none` → próximamente |
| Buscador semántico IA (lanzado may 2026) | Sin métricas de adopción/precisión | WBR → B2B Squad / PSPA | Marta Diaz | — | `none` |
| Incidencias panel (postmortem 25/05) | Panel en blanco, errores traducción BR/EN | WBR → B2B Squad (CIVI-2469, etc.) | Marta Diaz | Jira | Bug técnico, no métrica UX |

---

## 4. Datos de negocio globales (contexto para todas las dimensiones)

| Dato | Valor | Dónde en Brain | Responsable | Conector |
|---|---|---|---|---|
| Revenue semanal total | €1,62M (W22, -24,4% vs budget) | Mechanisms → WBR | Domingo Martín / Isma García | `brain:wbr` |
| Revenue YTD | €36,69M (32% del objetivo anual) | Weekly Report | Isma García | `brain:weekly-report` |
| Total usuarios semana | 58.980 (-4,1%) | Dashboard Pilar 1 | Javi García | `brain:pillar/1` |
| Nuevos compradores B2C YTD | 106.800 (target 1,7M = 0% ⚠️) | OKR CI-1626 | Isma García | `brain:impact` |
| Retención canal (revenue) | +140,8% YoY ✅ | WBR | Laura Llamas | `brain:wbr` |
| Afiliado revenue | -26,5% YoY ⚠️ | WBR | Lucía Delgado | `brain:wbr` |

---

## 5. OKRs AT RISK relacionados con experiencia (para la narrativa de demo)

Estos son los OKRs del Pilar 1 con **0% de progreso** — exactamente lo que el dashboard tiene que explicar:

| OKR | Target | Progreso | Owner | Relevancia UX |
|---|---|---|---|---|
| NPS B2C > 9,4 | 9,4 | **0%** 🔴 | Javi García (CI-1634) | Desirable |
| ACV/AOV B2C de 137€ a 142€ | 142€ | **0%** 🔴 | Javi García (CI-1634) | Valuable |
| NPS B2B > 8 | 8 | **0%** 🔴 | Lucía Delgado (CI-1781) | Desirable B2B |
| Nuevos compradores B2C a 1,7M | 1,7M | **0% YoY** 🔴 | Isma García (CI-1626) | Useful/Findable |
| CR B2C de 3,9 a 4,3 (+40bps) | 4,3% | 73% 🟡 | Javi García | Usable |
| LTV 12M de 29,36€ a 32,5€ | 32,5€ | 25% 🔴 | Laura Llamas (CI-1849) | Valuable |
| Calidad media tours > 9,2 | 9,2 | 20% 🔴 | Arturo Moreno (CI-1614) | Useful |

---

## 6. Alertas activas confirmadas en Brain (señales de alerta para el dashboard)

| Alerta | Severidad | Fuente | Relevancia dimensión |
|---|---|---|---|
| Uninstalls iOS +63,5% vs 2025 | 🔴 Alta | Product Surface → App → Installs | Desirable App |
| ANR rate App 0,52% > umbral Play Store 0,47% | 🔴 Alta | Play Console | Usable App |
| App ratings Play Store ~2,9 (benchmark 4,7–4,9) | 🔴 Alta | Play Console | Desirable App |
| PIX Brasil + Mercado Pago México atascados | 🔴 Alta | WBR → Payments | Credible LATAM |
| Afiliado -26,5% YoY · -31,3% vs budget | 🟡 Media | WBR | Valuable |
| Favoritos → conversión 0,0% (feature no funciona) | 🟡 Media | App User Behaviour | Findable/Usable App |
| VoC Strategic Themes sin configurar | 🟡 Media | VoC Configure | Credible/Accessible (dato bloqueado) |
| NPS post-primera compra AGE retrasado | 🟡 Media | WBR B2B | Desirable B2B |

---

## 7. Taxonomía VoC — tags de Zendesk mapeados a dimensiones

Brain tiene 50 tags de Zendesk disponibles. Mapeo a Honeycomb:

| Tag Zendesk | Grupo Brain | Dimensión | Segmento |
|---|---|---|---|
| `#accesibilidad` | Preventa → actividades | ♿ Accessible | B2C Web/App |
| `#idioma` | Preventa → actividades | ♿ Accessible | B2C Web/App |
| `#problemas_para_el_pago` | Problemas web | 🛡️ Credible | B2C Web |
| `#problemas_app` | Problemas web | 🖱️ Usable | B2C App |
| `#problemas_actividades_integradas` | Problemas web | 🖱️ Usable | B2C Web/App |
| `#informacion_erronea_en_web` | Problemas web | 🛡️ Credible | B2C Web |
| `#metodos_de_pago_disponibles` | Medios de pago | 🛡️ Credible | B2C Web/App |
| `#punto_de_encuentro` | Preventa → actividades | 🔍 Findable | B2C Web/App |
| `#dudas_proceso_de_reserva_` | Preventa → actividades | 🖱️ Usable | B2C Web/App |
| `#consulta_precios` | Preventa → actividades | 🛡️ Credible | B2C Web/App |
| `#baja_calidad_del_servicio` | Post-Travel → Reclamaciones | 🎯 Useful | B2C |
| `#incumplimiento_del_servicio` | Post-Travel → Reclamaciones | 🎯 Useful | B2C |
| `#punto_de_encuentro_incorrecto` | Post-Travel → Reclamaciones | 🔍 Findable | B2C |
| `#estado_del_reembolso` | Postventa → Pagos | 🛡️ Credible | B2C |
| `#no_show_transfer` | Post-Travel | 🖱️ Usable | B2C |

**Acción para el hackathon:** entrar en `brain.civitatis.tech/contacts/voc/configuration`,
seleccionar los tags relevantes y asignarlos como "Strategic Themes" para que aparezcan
las anécdotas filtradas en `/contacts/voc/b2c/anecdotes`. Tiempo estimado: 10 minutos.
Responsable: Domingo Martín o cualquier persona con acceso a Brain.

---

## 8. Fuentes confirmadas para el JSON de contrato (día 1 hackathon)

| Prioridad | Fuente | Quién gestiona acceso | Estado |
|---|---|---|---|
| 🟢 P1 | Brain Analytics API (BigQuery) | Owner de Brain + persona Data del equipo | Live, auto-actualizado |
| 🟢 P1 | Brain CS Stats (Zendesk) | Domingo Martín | Live con sync manual |
| 🟢 P1 | Brain VoC B2B | Domingo Martín / Lucía Delgado | Live (datos limitados) |
| 🟡 P2 | Play Console API | Apps Squad | Requiere service account |
| 🟡 P2 | Optimizely | Javi García | Requiere token |
| 🟡 P2 | Adjust / Braze | Clara Martínez / Javi García | Requiere token |
| 🔴 P3 | VoC B2C Anécdotas | Domingo Martín | Requiere configurar Strategic Themes (10 min) |
| 🔴 P3 | Funnel por paso (Web/App) | Javi García | Mock — no disponible aún |
| 🔴 P3 | NPS por canal (app vs web) | Arturo Moreno / Javi García | Sin segmentar |

---

## 9. Preguntas a resolver en la hora 0 del hackathon

1. **¿Brain tiene API REST/GraphQL que podamos llamar directamente**, o hay que ir a BigQuery raw? (preguntar al owner de Brain / persona Data)
2. **¿El VoC permite query programática** por tag + mes + sentimiento? (persona Data)
3. **¿El Funnel de Brain tiene fecha estimada de activación** con datos reales? (Javi García)
4. **¿Play Console service account disponible** para crash rate, ANR, ratings? (Apps Squad)
5. **¿Configuramos Strategic Themes del VoC** antes del hackathon? (Domingo Martín)

---

*Generado el 1 jun 2026 · Exploración directa de brain.civitatis.tech (sesión VPN)*
*Autor: Claude (Cowork) + Jota Coto · jcoto@civitatis.com*
