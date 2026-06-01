# UX Quality Dashboard · Civitatis Hackathon 2026

> **Hackathon: 2–3 junio 2026 · Pilar 1 — Ofrecer una experiencia excepcional para el cliente final**
>
> Reto: medir la calidad de la experiencia de usuario con la misma granularidad con la que Civitatis ya mide el negocio.

---

## ¿Qué es esto?

El **UX Quality Dashboard** es la lente UX sobre Brain. No construye un ETL nuevo — lee de Brain (que ya integra BigQuery + Zendesk + VoC) y añade lo que Brain no tiene: el mapeo de cada dato a una dimensión de experiencia Honeycomb y el registro que conecta cada decisión de diseño con su impacto medido.

**Dos vistas, dos propósitos:**
- **Salud UX** — termómetro permanente de las 7 dimensiones Honeycomb (Útil, Usable, Encontrable, Deseable, Creíble, Accesible, Valioso) para B2C Web, B2C App y B2B.
- **Gate 3 · Impacto por Feature** — registro del diseñador en el momento del lanzamiento: qué dimensión se intervino, qué métrica se esperaba mover, cuánto se movió a los 14/30 días.

**Owner:** Jose Ramón Coto Gallego ([@jota-erre-coto](https://github.com/jota-erre-coto)) · Head of UX, Civitatis

---

## Links rápidos (GitHub Pages)

| Herramienta | URL | Uso |
|---|---|---|
| 🏥 **Hackathon Proposal** | [Ver propuesta](https://jota-erre-coto.github.io/ux-quality-dashboard-hackathon/hackathon-proposal.html) | Propuesta completa visual para presentar al jurado |
| 🧠 **Brain Data Map Viewer** | [Ver mapa de datos](https://jota-erre-coto.github.io/ux-quality-dashboard-hackathon/brain-data-map-viewer.html) | Referencia interactiva: filtrar dónde está cada dato en Brain |

---

## Archivos del repositorio

### Punto de entrada

| Archivo | Qué es | Cuándo usarlo |
|---|---|---|
| [`index.html`](index.html) · [🌐 ver en web](https://jota-erre-coto.github.io/ux-quality-dashboard-hackathon/) | Landing de GitHub Pages — enlaza todas las herramientas y documentación del reto con una descripción de cada una | **Empezar aquí** — compartir esta URL con el equipo como punto de acceso único |

### Documentación de referencia

| Archivo | Qué es | Cuándo leerlo |
|---|---|---|
| [`ux-quality-dashboard-prd-v2.md`](ux-quality-dashboard-prd-v2.md) | PRD completo v2.1 — problema, solución, KPIs por dimensión, modelo de datos, requisitos, alcance MVP | Base de referencia del producto. Incluye §0 índice de todos los archivos. |
| [`ux-quality-dashboard-model-v2.md`](ux-quality-dashboard-model-v2.md) | Modelo de medición — clasificador de 3 etiquetas, Health Index, tablas de KPIs, JSON de contrato, plan 48h | Para entender cómo funciona el índice y qué mide cada dimensión. |
| [`ux-quality-dashboard-knowledge-export-v2.md`](ux-quality-dashboard-knowledge-export-v2.md) | Knowledge export — todo el conocimiento generado en la sesión de discovery (1 jun 2026), incluyendo exploración completa de Brain, decisiones cerradas del grill-me, evidencias de Confluence y rationale del framework | Para retomar el contexto en una nueva sesión sin repetir trabajo. |

### Datos y mapeo

| Archivo | Qué es | Cuándo usarlo |
|---|---|---|
| [`brain-data-map.md`](brain-data-map.md) | Mapa de 97 KPIs: para cada dato — dónde está en Brain, quién es el responsable, cómo extraerlo y a qué dimensión Honeycomb pertenece. Con OKRs AT RISK, alertas activas y taxonomía VoC. | **Referencia principal durante el hackathon** — consultar antes de buscar un dato en cualquier herramienta. |
| [`brain-data-map-viewer.html`](https://jota-erre-coto.github.io/ux-quality-dashboard-hackathon/brain-data-map-viewer.html) | Viewer interactivo del brain-data-map — filtrable por segmento (B2C Web / App / B2B), dimensión y estado del conector (live / sin instrumentar) | Abrir en el navegador el día del hackathon para localizar datos rápidamente. |

### Propuesta del reto

| Archivo | Qué es | Cuándo usarlo |
|---|---|---|
| [`hackathon-proposal-v2.md`](hackathon-proposal-v2.md) | Propuesta completa en markdown — problema con datos reales, oportunidad Brain, solución (2 vistas + MCP), plan 48h, métricas de éxito, equipo, riesgos | Fuente de verdad de la propuesta en texto plano. |
| [`hackathon-proposal.html`](https://jota-erre-coto.github.io/ux-quality-dashboard-hackathon/hackathon-proposal.html) | One-pager visual (Odisea design system) — para presentar al jurado. Hero, secciones con datos reales, terminal MCP, plan 48h, equipo. | Abrir en el navegador para la presentación al jurado. |
| [`hackathon-brain-panel.md`](hackathon-brain-panel.md) | Versión texto para pegar en el panel de retos de Brain — mismo formato que el resto de retos del hackathon | Copiar y pegar en Brain antes del 2 de junio. |

---

## Guía de uso para el día del hackathon

### Antes de empezar (hora 0)

1. Abrir el [Brain Data Map Viewer](https://jota-erre-coto.github.io/ux-quality-dashboard-hackathon/brain-data-map-viewer.html) en el navegador — es la referencia de datos durante todo el hackathon.
2. Acordar con el equipo el **JSON de contrato** (ver `ux-quality-dashboard-model-v2.md` §6) — es la interfaz entre la Pista Data y la Pista Producto.
3. Resolver las **5 preguntas de la hora 0** (ver `brain-data-map.md` §9 o el viewer §05).
4. Arrancar las **dos pistas en paralelo** — nadie bloquea a nadie.

### Pista Data (persona de Data)

- Confirmar: ¿Brain expone API REST o hay que ir a BigQuery directamente?
- Configurar Strategic Themes del VoC en Brain (`/contacts/voc/configuration`) — 10 min, owner Domingo Martín.
- Conectar Play Console API para crash rate, ANR rate y ratings App.
- Poblar el JSON de contrato con datos reales de mayo de 2026.

### Pista Producto (UX + Engineering)

- Dashboard data-driven contra el JSON de contrato con datos reales de Brain precargados.
- Al final del día 1: demo-able con todos los datos reales, aunque ningún conector esté live.
- Día 2: encender conectores `cached` → `brain:*` / `api:*`, endpoint MCP, narrativa de demo.

### Narrativa de demo (día 2)

> Abrir Claude, preguntar: *"¿Por qué pudo caer el AOV en mayo?"*
>
> El asistente cruza Salud UX (qué dimensiones bajaron en el periodo) + Gate 3 (qué features se lanzaron sobre esas dimensiones) y devuelve una hipótesis estructurada en vivo. Eso es el "no tenemos idea de por qué" del MBR de Marzo, resuelto en tiempo real.

---

## Datos corregidos respecto a versiones anteriores

Si alguien tiene documentos anteriores de este reto, estos son los valores corregidos con datos reales de Brain (1 jun 2026):

| Métrica | Valor anterior | **Valor correcto** | Fuente |
|---|---|---|---|
| CSAT B2C | 87,7% | **94%** | Brain CS Stats (Zendesk) |
| CES | 9,64 | **9,7** | Brain dashboard |
| NPS B2C | 9,36 | **9,3** | Brain Pilar 1 |
| AOV B2C | €137,7 | **€135** | Brain Pilar 1 (semanal) |
| CR web | 3,7% | **4,12%** | Brain Pilar 1 |
| Funnel checkout por paso | Disponible | **Mock Data (no live)** | Brain Executive Funnels |

---

## Stack del MVP

```
React + Tailwind + Vercel
↑
JSON de contrato (data-driven)
↑
Capa de adaptadores: brain:* | api:* | cached | none
↑
BRAIN (BigQuery + Zendesk + VoC) — fuente primaria
+ Play Console API (crash, ANR, ratings)
+ Optimizely API (CTR upsellings, A/B tests)
+ Braze API (push engagement)
```

---

## Framework de medición

**Honeycomb (Morville)** como columna vertebral — 7 dimensiones: Útil, Usable, Encontrable, Deseable, Creíble, Accesible, Valioso.

**HEART (Google)** como clasificador del tipo de medición dentro de cada dimensión.

**Clasificador de 3 etiquetas por métrica:**
- **Clase** — Negocio (resultado para Civitatis) vs Experiencia (lo que le pasa al usuario en la interacción)
- **Madurez** — 🟢 Directa · 🟡 Proxy · 🔴 Gap
- **Señal** (solo en Experiencia) — Conductual (lo que hace) vs Actitudinal (lo que siente)

---

*Repositorio generado el 1 jun 2026 · Jose Ramón Coto Gallego · jcoto@civitatis.com*
