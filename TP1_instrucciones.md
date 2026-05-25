# Trabajo Práctico N° 1 — EDA

**Fecha límite:** Martes 26 de mayo de 2026 a las 18:59 hs.
**Entrega:** Repositorio público en GitHub
**Presentación:** Durante la clase del mismo día

---

## Objetivo

Desarrollar un análisis exploratorio de datos (EDA) sobre un dataset elegido. Mediante tratamiento, limpieza y visualización, responder hipótesis definidas y extraer conclusiones relevantes.

---

## Consignas

### 1. Selección del dataset
- Mínimo **50.000 filas** y **10 columnas**
- Diversidad de tipos de datos: numéricos, fechas, categóricos, etc.

> **Dataset elegido:** `developer_burnout_dataset_7000.csv` — 7.000 filas, 12 columnas (11 numéricas + 1 categórica ordinal: `burnout_level`)
> **Autorizado por el docente.** El dataset fue validado y aprobado para su uso en este TP. Además, la temática (burnout en developers) resulta de interés personal y el dataset tiene potencial para trabajos futuros de Machine Learning.

---

### 2. Planteo de hipótesis
Formular entre **4 y 5 preguntas/hipótesis** que guíen el análisis.

---

### 3. Carga y limpieza de datos
- Importar el dataset
- Verificar integridad: valores nulos, duplicados, inconsistencias, tipos incorrectos

> **Nulos conocidos:** 140 filas completamente vacías en todas las columnas (~2%)

---

### 4. Análisis exploratorio de datos (EDA)

**Aspectos a cubrir:**
- Tipos de variables
- Distribuciones
- Detección de valores atípicos
- Relación entre variables (correlaciones)

**Visualizaciones mínimas requeridas:**

| Tipo | Cantidad mínima |
|------|:-----------:|
| Histogramas con KDE | 3 |
| Boxplots | 3 |
| Scatterplots | 2 |
| Visualizaciones adicionales (barras, líneas, torta, etc.) | 3 |

---

### 5. Conclusiones *(consigna clave de aprobación)*
- Responder cada hipótesis planteada en el punto 2
- Elaborar una conclusión final
- Identificar y comunicar hallazgos relevantes

---

## Condiciones de entrega

### Repositorio GitHub (público)

**`README.md`** debe incluir:
- [ ] Objetivo del trabajo
- [ ] Contexto del dataset
- [ ] Diccionario de datos
- [ ] Metodología aplicada
- [ ] Conclusiones y hallazgos relevantes

**Archivo(s) `.ipynb`:**
- [ ] Código correctamente ejecutado (sin errores)
- [ ] Comentarios claros y explicativos

---

## Consideraciones importantes
- Se permite cualquier recurso: material del curso, documentación, internet, IA, etc.
- **Todo el contenido debe poder ser explicado y defendido** por el estudiante, incluyendo decisiones metodológicas.

---

## Checklist de entrega

- [ ] Dataset cargado y limpio
- [ ] 4-5 hipótesis planteadas
- [ ] EDA completo (distribuciones, outliers, correlaciones)
- [ ] 3 histogramas con KDE
- [ ] 3 boxplots
- [ ] 2 scatterplots
- [ ] 3 visualizaciones adicionales
- [ ] Conclusiones con respuesta a cada hipótesis
- [ ] README.md completo
- [ ] Notebook ejecutado sin errores
- [ ] Repositorio GitHub público
