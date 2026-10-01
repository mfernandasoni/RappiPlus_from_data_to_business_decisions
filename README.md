# RappiPlus: From Data to Business Decisions

🌐 [English](#english) | [Español](#español)

---

<a name="english"></a>
## 🇬🇧 English

An end-to-end analysis evaluating RappiPlus's business performance — data quality, profitability, conversion funnel, cohort retention, an A/B test, and a BI dashboard — to support data-driven decisions.

### 1. Context / Problem
This was an individual capstone project developed as part of the **TripleTen Data Analyst certificate program**. The objective was to evaluate the performance of **RappiPlus** end to end, combining five data sources: order-level data (`rappiplus_orders_raw.csv`), a product catalog with costs (`rappiplus_catalog.csv`), marketing spend by channel (`rappiplus_marketing_spend.csv`), user behavior tables accessed via SQL (`events`, `users`, `user_activity`), and the results of a checkout UI A/B test (`experiment_checkout_ui.csv`).

The analysis followed a progressive structure: (1) assess whether the data could be trusted, (2) determine whether the business is profitable, (3) understand where users drop off in the conversion funnel, (4) evaluate whether users come back (cohort retention), (5) validate whether a checkout UI change actually improves conversion, and (6) communicate the results through a BI dashboard.

### 2. My Contribution
I was responsible for the full pipeline: data quality diagnosis and cleaning in Python, profitability and sales KPI calculations, SQL-based funnel and cohort retention analysis, the statistical A/B test, and building the Power BI dashboard.

### 3. Process and Decisions
- **Data quality first:** Before calculating any KPI, I validated date formats, checked numeric variables for negatives/zeros/nulls, verified amount consistency (`cantidad × precio_unitario − descuento` vs. `monto_total`), and checked for duplicates and categorical inconsistencies — since profitability numbers built on unreliable data would be meaningless.
- **Quantifying, not guessing, data quality issues:** Rather than dropping ambiguous records outright, I measured their impact first. For 80 records with missing cost information, I compared profit with and without them (a 0.58% difference) and decided to keep their revenue but exclude their cost, since there wasn't enough information to calculate it reliably. For 10 extreme quantity outliers (all the same product, 10,000–20,000 units), I quantified their effect on profit separately and found they would have more than doubled it — clear evidence they didn't belong in the main KPI calculation.
- **Recovering data instead of discarding it:** Where possible, I recovered missing values from other reliable sources instead of dropping records or imputing arbitrary values — for example, recovering missing `categoria_producto` from the catalog, missing `pais` from each user's purchase history, and missing marketing `canal` from the campaign ID pattern.
- **SQL for funnel and cohorts, Python for the rest:** I used SQL directly against the `events`, `users`, and `user_activity` tables for the funnel and cohort retention analysis, since these required set-based aggregation across millions of event-level rows, and used Python/pandas for the cleaning, KPI, and A/B test work.
- **Rigorous A/B testing:** For the checkout UI experiment, I explicitly stated the null and alternative hypotheses before testing, checked the balance of the control/treatment groups and key variables, and applied a two-proportion Z-test at α = 0.05 — concluding that the observed difference was not statistically significant, rather than over-interpreting a directional but non-significant result.
- **Separating "what happened" from "what should guide decisions":** Throughout, I kept the extreme outlier records and the ambiguous 80 records visible and quantified, instead of silently folding them into the headline numbers — so stakeholders can see both the clean business picture and the edge cases that could distort it.

### 4. Outcome / Learning
- **Data quality:** 95.25% of orders were arithmetically consistent; 100 duplicate orders were removed; 80 records (0.32%) had unrecoverable cost information; 10 extreme outlier records were identified and excluded from the main analysis.
- **Profitability:** RappiPlus is profitable. Total revenue of **$9,642,762.26**, product cost of **$3,828,818.41**, gross profit of **$5,813,943.85**, and after marketing spend ($2,871,843.53), a **net profit of $2,942,100.32**.
- **Sales behavior:** The average user places small orders (1.50 products per order, average ticket of $385.97). The best-selling product (Vacuum-Pro-Black) doesn't dominate — demand is fairly evenly spread across top products. Marketing spend is nearly balanced across Social, Organic, and Paid Search channels.
- **Outlier impact:** Including the 10 extreme outlier records would inflate profit by $3,049,500 — more than doubling the real figure — confirming they were correctly excluded from the main KPI calculation.
- **Conversion funnel:** 80.04% of users who visit the site complete a purchase. The main bottleneck is the `begin_checkout` → `add_payment_info` step (86.71% conversion, the lowest of all stages, losing 958 users); once a user enters payment info, 99.84% complete the purchase — the problem is reaching the payment form, not closing the sale.
- **Cohort retention:** Weekly retention is stable across cohorts (~85-87% week 1, ~77-80% week 2, ~63-65% week 3), but user loss accelerates sharply between week 2 and week 3, pointing to a critical re-engagement window around week 2.
- **A/B test:** The modified checkout UI did not produce a statistically significant improvement in conversion (z = -0.813, p = 0.4161) — recommendation was not to implement it based on this test alone, and to consider a longer test or additional metrics (abandonment, completion time, errors) if the change remains of strategic interest.
- **Dashboard:** Delivered a Power BI dashboard consolidating the cleaned data (orders, catalog, marketing) to communicate these findings visually.

### 5. Tools Used
- Python (pandas, NumPy, Matplotlib, statsmodels)
- SQL (PostgreSQL, via SQLAlchemy)
- Power BI

### 6. Evidence
- 📓 Project notebook (English version) with the full analysis: see repository
- 📊 Power BI dashboard (.pbix) and published link: see repository / [Google Drive folder](https://drive.google.com/drive/folders/18Z5Tzot72NF3Jxzcu2OroR-UOnMWOPjr?usp=sharing)

---
# RappiPlus de datos a decisiones de negocio

<a name="español"></a>
## 🇪🇸 Español

Un análisis integral que evalúa el desempeño de negocio de RappiPlus —calidad de datos, rentabilidad, funnel de conversión, retención por cohortes, un test A/B y un dashboard de BI— para apoyar decisiones basadas en datos.

### 1. Contexto / Problema
Este fue un proyecto final individual desarrollado como parte del **certificado de Data Analyst de TripleTen**. El objetivo era evaluar el desempeño de **RappiPlus** de principio a fin, combinando cinco fuentes de datos: datos a nivel de pedido (`rappiplus_orders_raw.csv`), un catálogo de productos con costos (`rappiplus_catalog.csv`), gasto de marketing por canal (`rappiplus_marketing_spend.csv`), tablas de comportamiento de usuario accedidas vía SQL (`events`, `users`, `user_activity`), y los resultados de un test A/B de la UI de checkout (`experiment_checkout_ui.csv`).

El análisis siguió una estructura progresiva: (1) evaluar si se podía confiar en los datos, (2) determinar si el negocio es rentable, (3) entender en qué punto del funnel de conversión se pierden los usuarios, (4) evaluar si los usuarios regresan (retención por cohortes), (5) validar si un cambio en la UI de checkout realmente mejora la conversión, y (6) comunicar los resultados mediante un dashboard de BI.

### 2. Mi Contribución
Fui responsable de todo el proceso: diagnóstico y limpieza de calidad de datos en Python, cálculo de KPIs de rentabilidad y ventas, análisis de funnel y retención por cohortes en SQL, el test estadístico A/B, y la construcción del dashboard en Power BI.

### 3. Proceso y Decisiones
- **La calidad de datos primero:** Antes de calcular cualquier KPI, validé los formatos de fecha, revisé las variables numéricas en busca de negativos/ceros/nulos, verifiqué la consistencia de montos (`cantidad × precio_unitario − descuento` vs. `monto_total`), y revisé duplicados e inconsistencias categóricas, ya que las cifras de rentabilidad construidas sobre datos poco confiables no tendrían sentido.
- **Cuantificar, no adivinar, los problemas de calidad de datos:** En lugar de descartar registros ambiguos directamente, primero medí su impacto. Para 80 registros con información de costo faltante, comparé el profit con y sin ellos (una diferencia de 0.58%) y decidí mantener su ingreso pero excluir su costo, ya que no había suficiente información para calcularlo de forma confiable. Para 10 outliers extremos de cantidad (todos del mismo producto, 10,000–20,000 unidades), cuantifiqué su efecto en el profit por separado y encontré que lo habrían más que duplicado, evidencia clara de que no correspondían al cálculo principal de KPIs.
- **Recuperar datos en lugar de descartarlos:** Cuando fue posible, recuperé valores faltantes a partir de otras fuentes confiables en lugar de eliminar registros o imputar valores arbitrarios; por ejemplo, recuperando `categoria_producto` faltante desde el catálogo, `pais` faltante desde el historial de compras de cada usuario, y `canal` de marketing faltante a partir del patrón del ID de campaña.
- **SQL para funnel y cohortes, Python para el resto:** Usé SQL directamente contra las tablas `events`, `users` y `user_activity` para el análisis de funnel y retención por cohortes, ya que requerían agregaciones a nivel de conjunto sobre millones de filas a nivel de evento, y usé Python/pandas para la limpieza, los KPIs y el test A/B.
- **Test A/B riguroso:** Para el experimento de la UI de checkout, planteé explícitamente las hipótesis nula y alternativa antes de testear, revisé el balance de los grupos control/tratamiento y de las variables clave, y apliqué una prueba Z de dos proporciones con α = 0.05, concluyendo que la diferencia observada no era estadísticamente significativa, en lugar de sobre-interpretar un resultado direccional pero no significativo.
- **Separar "qué pasó" de "qué debería guiar las decisiones":** A lo largo del proyecto, mantuve visibles y cuantificados los registros outlier extremos y los 80 registros ambiguos, en lugar de integrarlos silenciosamente en las cifras principales, para que los stakeholders puedan ver tanto el panorama limpio del negocio como los casos borde que podrían distorsionarlo.

### 4. Resultado / Aprendizaje
- **Calidad de datos:** El 95.25% de los pedidos fueron aritméticamente consistentes; se eliminaron 100 pedidos duplicados; 80 registros (0.32%) tenían información de costo no recuperable; se identificaron y excluyeron 10 registros outlier extremos del análisis principal.
- **Rentabilidad:** RappiPlus es rentable. Ingreso total de **$9,642,762.26**, costo de producto de **$3,828,818.41**, utilidad bruta de **$5,813,943.85**, y tras el gasto de marketing ($2,871,843.53), una **utilidad neta de $2,942,100.32**.
- **Comportamiento de ventas:** El usuario promedio hace pedidos pequeños (1.50 productos por pedido, ticket promedio de $385.97). El producto más vendido (Vacuum-Pro-Black) no domina; la demanda se reparte de forma bastante pareja entre los productos principales. El gasto de marketing está casi equilibrado entre los canales Social, Organic y Paid Search.
- **Impacto de los outliers:** Incluir los 10 registros outlier extremos infla el profit en $3,049,500, más del doble de la cifra real, confirmando que fueron correctamente excluidos del cálculo principal de KPIs.
- **Funnel de conversión:** El 80.04% de los usuarios que visitan el sitio completan una compra. El principal cuello de botella está en el paso `begin_checkout` → `add_payment_info` (conversión de 86.71%, la más baja de todas las etapas, con 958 usuarios perdidos); una vez que el usuario ingresa su información de pago, el 99.84% completa la compra — el problema está en llegar al formulario de pago, no en cerrar la venta.
- **Retención por cohortes:** La retención semanal es estable entre cohortes (~85-87% semana 1, ~77-80% semana 2, ~63-65% semana 3), pero la pérdida de usuarios se acelera notablemente entre la semana 2 y la semana 3, señalando una ventana crítica de re-enganche alrededor de la semana 2.
- **Test A/B:** La UI de checkout modificada no produjo una mejora estadísticamente significativa en la conversión (z = -0.813, p = 0.4161); la recomendación fue no implementarla únicamente con base en este test, y considerar una prueba más prolongada o métricas adicionales (abandono, tiempo de finalización, errores) si el cambio sigue siendo de interés estratégico.
- **Dashboard:** Se entregó un dashboard en Power BI que consolida los datos limpios (pedidos, catálogo, marketing) para comunicar estos hallazgos de forma visual.

### 5. Herramientas Utilizadas
- Python (pandas, NumPy, Matplotlib, statsmodels)
- SQL (PostgreSQL, vía SQLAlchemy)
- Power BI

### 7. Evidencias
- 📓 Notebook del proyecto (versión en español) con el análisis completo: ver repositorio
- 📊 Dashboard de Power BI (.pbix) y link publicado: ver repositorio / [carpeta de Google Drive](https://drive.google.com/drive/folders/18Z5Tzot72NF3Jxzcu2OroR-UOnMWOPjr?usp=sharing)
