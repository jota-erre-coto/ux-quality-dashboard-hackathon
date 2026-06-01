# UX Quality Dashboard — Civitatis mide el negocio. Nadie mide la experiencia.

**Jose Ramón Coto Gallego · Head of UX**

---

## El problema

Cuando un diseñador pregunta si lo que ha diseñado "¿mejoró la experiencia?", la respuesta honesta es que nadie lo sabe con certeza. Los datos que lo responderían están fragmentados entre GA4, Zendesk, Clarity, Adjust y los informes de Data — y nadie los conecta a la decisión de diseño concreta que los provocó — o directamente no existen (no hay ninguna métrica de accesibilidad).

Todo lo que lanzamos a producción se evalúa con métricas de negocio (NR en Brain, datos de conversión en GA4, NPS, CSAT en Zendesk) pero como promedios globales sin desglose por feature o surface, por momento del journey o por feature lanzada. No existe ningún artefacto donde ver una capa de evidencia UX.

En el MBR de Marzo la conclusión fue que el AOV cayó de 155 € a 135 € y la respuesta fue "no tenemos una idea clara de por qué". Eso no es falta de datos de negocio — los equipos tienen métricas de input y de output. Falta la capa intermedia que vincula cada decisión de diseño con su impacto medido.

La conexión que falta tiene esta forma: feature lanzada → diseñador responsable → dimensión UX intervenida → métrica antes/después.

Si miramos los OKRs de experiencia del Pilar 1, el patrón es el mismo: NPS >9,4 con 0% de progreso, AOV 137€→142€ con 0% de progreso, NPS B2B >8 con 0% de progreso. No falta ambición de medir — falta la instrumentación que conecta las decisiones de diseño con los datos.

El dato que lo cambia todo: durante el discovery de este reto se exploró Brain en profundidad y se confirmó que ya integra BigQuery, Zendesk y un VoC con 50 tags de sentimiento por tema. Brain ya tiene los datos. Nadie los mira con lente de experiencia.

Civitatis tiene métricas de negocio muy bien instrumentadas. Lo que no tiene es una capa de calidad UX con la misma granularidad — y ese es el problema que este reto propone resolver.

---

## Solución propuesta

El UX Quality Dashboard es la lente UX sobre Brain. No construye un ETL nuevo — lee de Brain (que ya bebe de BigQuery + Zendesk + VoC) y añade lo único que Brain no tiene: el mapeo de cada dato a una dimensión de experiencia y el registro que vincula cada decisión de diseño con su impacto medido.

**Dos vistas, dos propósitos distintos.**

La primera es Salud UX: un estado permanente de las 7 dimensiones del framework Honeycomb+HEART (Útil, Usable, Encontrable, Deseable, Creíble, Accesible, Valioso) para los tres segmentos B2C Web, B2C App y B2B. Cada dimensión muestra mínimo 1 métrica de negocio y 2 señales de experiencia (conductual y actitudinal), un semáforo con el estado actual y una tendencia ↑↓→ vs el mes anterior. El UX Health Index agrega el estado de las 7 dimensiones — no promedia métricas crudas — y va acompañado de un segundo número: Cobertura X/7, que impide que el índice suba ignorando lo que no se mide.

Lo que permite esta vista que hoy no existe: entrar a una sesión de Pilar 1 con un diagnóstico de experiencia estructurado. Cuando el NPS baja o el AOV cae, la Vista 1 puede mostrar si una dimensión UX se degradó en el mismo periodo y qué features se lanzaron sobre ella — pasando de "no tenemos idea de por qué" a una hipótesis estructurada.

Los datos ya disponibles para el MVP desde Brain: CR web 4,12%, AOV 135 €, NPS 9,3, CES 9,7, CSAT B2C 94% y CSAT B2B 95% (Zendesk), Perfect Memories rate 70,7% (830K reviews), Uninstalls iOS +63,5% (señal de alerta Desirable App), NPS B2B 35/100 (VoC Brain), tags VoC #accesibilidad y #problemas_para_el_pago disponibles como señal cualitativa viva.

La segunda es Gate 3 · Impacto por Feature: el diseñador crea una entrada el día del lanzamiento con cinco campos — feature + Jira, diseñador responsable, surface, dimensión Honeycomb primaria intervenida e hipótesis con métrica antes/después. El campo valor_después empieza en nulo y crea un estado open/closed por entrada. Las entradas abiertas más de 30 días generan alerta.

Por qué Gate 3 y no una herramienta independiente: Gate 3 ya existe en el modelo operativo v0.7 como el gate de post-launch donde se evalúa si una feature tuvo impacto. Hoy esa evaluación usa únicamente métricas de negocio. El dashboard no crea una nueva ceremonia — completa una que ya está definida. Además resuelve dos blockers del PD Model v0.7: H0-2 ("PD sin Gate", blocker) y H1-1 ("Gate de Calidad UX", riesgo alto).

Con suficientes entradas, la vista permite responder preguntas hoy imposibles: qué dimensión mejora más con los recursos actuales, qué surface tiene más deuda de experiencia acumulada, y qué decisiones de diseño de los últimos 3 meses explican el movimiento del NPS.

La capa MCP permite a Claude consultar el dashboard directamente desde una sesión de Pilar 1: "¿por qué pudo caer el AOV en mayo?" → Claude cruza Salud UX (qué dimensiones bajaron) + Gate 3 (qué features se lanzaron en ese periodo sobre esas dimensiones) y devuelve una hipótesis estructurada en vivo. Es el "no tenemos idea de por qué" del MBR de Marzo resuelto en tiempo real.

---

## Enfoque técnico

**Quién hace el trabajo**

Decisión: ambos. Se requiere juicio y contexto para analizar la experiencia de usuario y vincular decisiones de diseño con métricas, pero también scripts para la recopilación estructurada de datos desde Brain/BigQuery, Zendesk, Play Console y Optimizely.

**Quién consume**

Decisión: UI + MCP (mismo backend). La solución requiere una interfaz para que los diseñadores registren entradas Gate 3 y visualicen el estado de experiencia por dimensión. La capa MCP permite que Claude consulte el dashboard directamente durante una sesión de MBR del Pilar 1, sin abrir el navegador: "¿cuál es el estado actual de la dimensión Usable y qué features se lanzaron en las últimas 4 semanas?"

**Coste del error**

Tier: medio. Human-in-the-loop obligatorio para las entradas Gate 3: el diseñador es responsable de la coherencia entre la feature descrita, la dimensión Honeycomb seleccionada y la métrica antes/después. El sistema puede sugerir la dimensión basándose en la descripción de la feature, pero no reemplaza ese juicio.

**Anti-patrones verificados**

- ✅ UI para algo que solo consumirán agentes — no aplica: la interfaz la utilizarán diseñadores, PMs y liderazgo.
- ✅ Agente para un script determinista — no aplica: se necesita comprensión del contexto de diseño para vincular decisiones con métricas.
- ✅ UI-only cuando la capacidad es claramente componible — no aplica: requiere UI + API/MCP para integración desde sesiones de MBR.
- ✅ ETL propio cuando los datos ya existen — no aplica: el dashboard lee de Brain. Brain ya integra BigQuery + Zendesk + VoC. El reto del Dashboard 360º App construye el ETL de adquisición; este dashboard consume sus tablas y añade la lente UX.

**Stack sugerido**

React + Tailwind para la UI con dos vistas (Salud UX / Gate 3), filtros por segmento, dimensión y surface. Datos en JSON de contrato data-driven con capa de adaptadores (brain:* | api:* | cached | none) — pasar de datos precargados a conexión real no toca la UI. Hosting en Vercel (deploy inmediato). Capa MCP con endpoint REST para consulta desde Claude. Fuente primaria: Brain (BigQuery vía brain.civitatis.tech) + Zendesk + VoC + Play Console API para crash rate, ANR y ratings App.

**Plan de implementación**

Hora 0: el equipo acuerda el JSON de contrato en 30 minutos. A partir de ahí, dos pistas paralelas que no se bloquean entre sí.

Día 1 · Pista Data: confirmar qué expone Brain vía API o BigQuery directo, configurar Strategic Themes del VoC (10 min, owner Domingo Martín), conectar Play Console, entregar JSON de contrato con datos reales de mayo. Pista Producto: dashboard completo con los 3 segmentos y las 7 dimensiones, datos Brain precargados (CR 4,12% · AOV 135 € · NPS 9,3 · CSAT 94% · Perfect Memories 70,7% · Uninstalls iOS +63,5%), UX Health Index + tendencia vs abril, 2 entradas Gate 3 reales (Checkout 2 pasos v4.7.0 y Upsell Free Tour→Privado). Demo-able al final del día 1 aunque ningún conector esté live.

Día 2: encender conectores cached→live sin tocar UI, endpoint MCP básico, ensayar la narrativa de demo "¿por qué cayó el AOV?" con Claude respondiendo en vivo.

**Complejidad estimada**

M — dos vistas con lógicas distintas (estado permanente vs. registro histórico con alertas), formulario Gate 3, capa de adaptadores de datos con conectores múltiples, endpoint MCP y datos reales de Civitatis desde Brain.

**Riesgos abiertos**

- Funnel Brain en Mock Data (Design Mode): el abandono de checkout por paso no está disponible como dato live. Se muestra como precargado con badge "sin instrumentar". No bloquea la demo.
- VoC sin Strategic Themes configurados: las anécdotas B2C no filtran hasta configurarlos. Acción previa al hackathon (10 min, Domingo Martín).
- Brain sin API REST directa: si Brain no expone endpoint, la persona de Data va a BigQuery raw. El JSON de contrato es el buffer entre pistas.
- Solapamiento con Dashboard 360º App: delimitar desde la hora 0 — ellos construyen el ETL de adquisición, este dashboard consume sus tablas en BigQuery y añade la lente de experiencia.
- Adopción del ritual Gate 3 post-hackathon: requiere incorporarlo explícitamente al proceso de Gate 3 del modelo operativo, no solo como herramienta disponible.
