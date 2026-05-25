# TP1 — Análisis Exploratorio de Datos: Developer Burnout

**Alumno:** Gonzalo Zarazaga
**Diplomatura en Inteligencia Artificial — Universidad**
**Entrega:** 26 de mayo de 2026

---

## Objetivo

Desarrollar un análisis exploratorio de datos (EDA) sobre un dataset de burnout en developers, con el fin de identificar qué factores de trabajo y estilo de vida se asocian al agotamiento profesional. El análisis busca responder hipótesis concretas y extraer conclusiones que puedan orientar decisiones de gestión de equipos de desarrollo.

---

## Contexto del dataset

El dataset modela patrones de trabajo, niveles de estrés y riesgo de burnout en desarrolladores de software, a partir de variables de productividad y hábitos de vida. Captura relaciones realistas entre carga laboral, salud y estrés mental.

- **Filas:** 7.000
- **Columnas:** 12
- **Valores faltantes:** ~2% (distribuidos en 1.510 filas)
- **Variable objetivo:** `burnout_level` (Low / Medium / High)
- **Fuente:** [Kaggle — Developer Burnout Prediction Dataset](https://www.kaggle.com/datasets/asifxzaman/developer-burnout-prediction-dataset7000-samples)

> Dataset validado y autorizado por el docente para su uso en este trabajo.

---

## Diccionario de datos

| Variable | Tipo | Descripción |
|---|---|---|
| `age` | Numérica discreta | Edad del developer (20–44 años) |
| `experience_years` | Numérica discreta | Años de experiencia (0–19) |
| `caffeine_intake` | Numérica discreta | Tazas de café por día (0–7) |
| `bugs_per_day` | Numérica discreta | Bugs generados por día (0–19) |
| `commits_per_day` | Numérica discreta | Commits de Git por día (0–29) |
| `meetings_per_day` | Numérica discreta | Reuniones por día (0–9) |
| `daily_work_hours` | Numérica continua | Horas de trabajo por día (4–14) |
| `sleep_hours` | Numérica continua | Horas de sueño por día (4–9) |
| `screen_time` | Numérica continua | Tiempo total frente a pantalla en horas (5–19) |
| `exercise_hours` | Numérica continua | Actividad física diaria en horas (0–2) |
| `stress_level` | Numérica continua | Puntaje de estrés de 0 a 100 |
| `burnout_level` | **Categórica ordinal** | Variable objetivo: *Low* < *Medium* < *High* |

---

## Hipótesis analizadas

- **H1:** A mayor `stress_level`, mayor `burnout_level`.
- **H2:** A mayor cantidad de horas trabajadas (`daily_work_hours`), mayor nivel de estrés (`stress_level`).
- **H4:** Los developers que realizan más actividad física (`exercise_hours`) presentan menor nivel de estrés y burnout.
- **H5:** A mayor cantidad de reuniones por día (`meetings_per_day`), mayor nivel de estrés (`stress_level`).

---

## Metodología aplicada

### 1. Carga e inspección inicial
Exploración general del dataset: tipos de datos, rangos, distribución de la variable objetivo y valores únicos por columna. Se verificó que las variables numéricas continuas tienen alta cardinalidad (`stress_level`: 4.554 valores únicos) mientras que las discretas presentan rangos acotados y enteros.

### 2. Limpieza de datos
Se detectaron 140 valores nulos por columna, distribuidos en 1.510 filas con al menos un campo vacío:

| Nulos por fila | Filas |
|:-:|:-:|
| 0 (sin nulos) | 5.490 |
| 1 nulo | 1.352 |
| 2 nulos | 146 |
| 3 nulos | 12 |

**Estrategia adoptada:**
- Filas donde `burnout_level` es nulo → **eliminar** (la variable objetivo no puede imputarse)
- Nulos en columnas numéricas → **imputar con la mediana** de cada columna (más robusta que la media frente a valores atípicos)

Resultado: **6.860 filas limpias**, sin valores faltantes.

### 3. Detección de valores atípicos
Se utilizó el método **IQR (Rango Intercuartílico)** para identificar outliers en cada variable numérica. Un valor se considera atípico si cae fuera del rango `Q1 − 1.5×IQR` / `Q3 + 1.5×IQR`. Este método es más robusto que el Z-score porque no asume distribución normal.

**Resultado:** ninguna variable presentó outliers (0 en las 11 columnas numéricas). El dataset fue construido con rangos acotados y distribuciones sintéticas controladas, lo que explica la ausencia de valores extremos.

### 4. Análisis exploratorio (EDA)

**Visualizaciones realizadas:**

| Tipo | Cantidad | Variables / hipótesis |
|---|:-:|---|
| Histogramas con KDE | 5 | `stress_level`, `daily_work_hours`, `meetings_per_day`, `sleep_hours`, `exercise_hours` — todos segmentados por `burnout_level` |
| Histograma sin segmentar | 1 | `screen_time` — distribución general |
| Boxplots | 3 | `stress_level` (H1), `daily_work_hours` (H2), `exercise_hours` (H4) — todos por `burnout_level` |
| Scatterplots | 2 | `daily_work_hours` vs `stress_level` (H2), `exercise_hours` vs `stress_level` (H4) — con línea de tendencia |
| Torta | 1 | Distribución proporcional de `burnout_level` |
| Barras | 1 | Estrés promedio por cantidad de reuniones (H5) |
| Heatmap de correlaciones | 1 | Todas las variables numéricas |

**Principales correlaciones identificadas:**

| Par de variables | Pearson | Interpretación |
|---|:-:|---|
| `daily_work_hours` ↔ `screen_time` | **0.91** | Correlación muy fuerte: quien trabaja más horas pasa más tiempo en pantalla. Variables casi redundantes. |
| `daily_work_hours` ↔ `stress_level` | **0.59** | Confirma H2: más horas → más estrés |
| `screen_time` ↔ `stress_level` | **0.54** | El tiempo de pantalla también predice estrés |
| `bugs_per_day` ↔ `stress_level` | **0.48** | Generar más bugs se asocia a mayor estrés |
| `meetings_per_day` ↔ `stress_level` | **0.35** | Confirma H5: más reuniones → más estrés |
| `sleep_hours` ↔ `stress_level` | **−0.25** | Dormir más se asocia levemente a menos estrés |
| `exercise_hours` ↔ `stress_level` | **−0.10** | Correlación negativa débil — confirma H4 parcialmente |

La mayoría de los pares no relacionados con `stress_level` presentan correlaciones cercanas a 0, lo que indica que las variables son en general independientes entre sí.

---

## Conclusiones y hallazgos relevantes

### H1 — Estrés y burnout ✅ Confirmada
El nivel de estrés es el predictor más fuerte del burnout. Las medianas por grupo son prácticamente sin solapamiento: **24.4** (Low), **53.2** (Medium) y **81.2** (High). `stress_level` es la variable más discriminante del dataset.

### H2 — Horas de trabajo y estrés ✅ Confirmada
Correlación de Pearson de **0.59** entre horas trabajadas y estrés. Los developers con burnout *High* trabajan en mediana **11.5 hs/día**, casi el doble que los de burnout *Low* (**6.4 hs/día**). Se observa un umbral práctico alrededor de las **10 horas diarias**: por encima de ese valor, el burnout *High* se convierte en el grupo dominante.

### H4 — Ejercicio como factor protector ⚠️ Confirmada parcialmente
Existe una correlación negativa débil entre ejercicio y estrés (**−0.10**). La mediana de ejercicio desciende levemente de *Low* (1.10 hs) a *High* (0.90 hs). El efecto protector existe pero es marginal: el ejercicio no compensa el impacto de una carga laboral excesiva. Un developer que se ejercita puede igualmente tener burnout alto si trabaja muchas horas.

### H5 — Reuniones y estrés ✅ Confirmada
Correlación de Pearson de **0.35** entre reuniones y estrés. Los developers con burnout *High* tienen en mediana **6 reuniones/día** vs **3** en el grupo *Low*. El overhead de comunicación duplicado se asocia al nivel más alto de agotamiento. Tener pocas reuniones (≤ 3) se asocia claramente a burnout *Low*, aunque a partir de 5 reuniones la distinción entre *Medium* y *High* depende de otros factores como las horas trabajadas.

### Hallazgo principal
El burnout en developers responde a una combinación de factores, no a una causa única:

1. **El estrés es el predictor dominante**: `stress_level` separa casi perfectamente las tres categorías.
2. **La carga de trabajo es el principal driver del estrés**: superar las 10 horas diarias se asocia fuertemente con burnout *High*.
3. **Las reuniones amplifican el estrés**: duplicar la cantidad de reuniones correlaciona con pasar de burnout *Low* a *High*.
4. **El ejercicio tiene un efecto protector pero limitado**: reduce levemente el riesgo pero no compensa una carga laboral excesiva.

Las intervenciones más efectivas deberían focalizarse en **limitar las horas de trabajo y optimizar la cantidad de reuniones**, más que en promover únicamente hábitos saludables individuales.

---

## Estructura del repositorio

```
├── README.md
├── TP1_instrucciones.md     # Consignas del trabajo práctico
├── TP1_zarazaga.ipynb       # Notebook principal con el análisis completo
└── data/
    └── developer_burnout_dataset_7000.csv
```
