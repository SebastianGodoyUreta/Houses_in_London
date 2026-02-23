# 🏠 Houses in London — KMeans Clustering Analysis
### Unsupervised Learning Exercise / Ejercicio de Aprendizaje No Supervisado

![Python](https://img.shields.io/badge/Python-3.12-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-orange)
![Status](https://img.shields.io/badge/Status-Complete-green)

---

## Overview / Descripción

A two-phase unsupervised learning analysis applied to a London housing dataset, using KMeans clustering to explore whether houses naturally group into meaningful market segments.

Análisis de aprendizaje no supervisado en dos fases aplicado a un dataset de casas en Londres, usando clustering KMeans para explorar si las casas se agrupan naturalmente en segmentos de mercado significativos.

> **Key finding / Hallazgo clave:** The dataset is synthetically generated and lacks real cluster structure. With `Neighborhood` included, KMeans perfectly replicates the neighborhood column (Conceptual Data Leakage). Without it, no meaningful structure emerges (Silhouette: 0.09, ANOVA p=0.17).

---

## Repository Structure / Estructura del Repositorio

```
Houses_in_London/
├── Houses_in_London.ipynb   # Main notebook (bilingual ES/EN)
├── london_houses.csv        # Dataset (1000 rows, 17 columns)
└── README.md
```

---

## Dataset

| | |
|---|---|
| **Source / Fuente** | [Kaggle — Houses in London](https://www.kaggle.com/datasets/oktayrdeki/houses-in-london) |
| **Rows / Filas** | 1,000 |
| **Columns / Columnas** | 17 |
| **Null values / Nulos** | 0 |
| **Target (excluded) / Objetivo (excluido)** | Price (£) |

---

## Methodology / Metodología

### Preprocessing / Preprocesamiento

| Column / Columna | Treatment / Tratamiento |
|---|---|
| `Address` | Drop |
| `Price (£)` | Drop (saved for analysis / guardado para análisis) |
| `Garden`, `Garage` | Binary map → 0/1 |
| `Balcony` | Ordinal → 0, 1, 2 |
| `Building Status` | Ordinal → 0, 1, 2 |
| `Neighborhood`, `Property Type`, `Heating Type`, `Interior Style`, `View`, `Materials` | One-Hot Encoding |

> ⚠️ **Bug fixed / Bug corregido:** Original notebook incorrectly mapped `Balcony` as binary (Yes/No), generating NaN across the entire column. Corrected as ordinal variable.

### Scaling / Escalado
StandardScaler — zero mean (μ=0), unit variance (σ=1)

---

## Results / Resultados

### Phase 1 — With Neighborhood / Fase 1 — Con Neighborhood

| Method / Método | Result / Resultado |
|---|---|
| Elbow Method | No clear elbow / Sin codo claro |
| KneeLocator | `None` |
| Silhouette Score | K=10, score: **0.1402** |
| ANOVA F-statistic | **26.35** (p = 0.000000 ✅) |
| Clusters represent | **Neighborhoods perfectly / Vecindarios perfectamente** |

🚨 KMeans replicated the `Neighborhood` column with 0 houses mixed between neighborhoods — **Conceptual Data Leakage**.

### Phase 2 — Without Neighborhood / Fase 2 — Sin Neighborhood

| Method / Método | Result / Resultado |
|---|---|
| Silhouette Score | K=4, score: **0.0893** |
| ANOVA F-statistic | **1.67** (p = 0.170911 ❌) |
| Clusters represent | **Nothing real / Nada real** |

All physical variables (bedrooms, m², floors, age) are virtually identical across clusters.

### Comparative Summary / Resumen Comparativo

| | Phase 1 | Phase 2 |
|---|---|---|
| Neighborhood | ✅ Included | ❌ Excluded |
| Optimal K | 10 | 4 |
| Silhouette | 0.1402 | 0.0893 |
| ANOVA p-value | 0.000000 ✅ | 0.170911 ❌ |
| Clusters | = Neighborhoods | = Noise |

---

## Key Learnings / Aprendizajes Clave

**1. Preprocessing matters / El preprocesamiento importa**
A single encoding bug (`Balcony`) silently corrupted the entire column.

**2. Conceptual Data Leakage**
One-Hot Encoding `Neighborhood` into 10 columns gave it excessive weight, dominating the model completely.

**3. Synthetic data diagnosis / Diagnóstico de datos sintéticos**
Neighborhood std = 8.25 over a mean of 100 — too uniform for real London market data.

**4. Failure teaches / El fracaso también enseña**
Diagnosing *why* a model fails is a core data science skill.

---

## Technologies / Tecnologías

- Python 3.12
- pandas, numpy
- scikit-learn (KMeans, StandardScaler, silhouette_score)
- seaborn, matplotlib
- scipy (ANOVA)
- kneed (KneeLocator)

---

## How to Run / Cómo Ejecutar

```bash
git clone https://github.com/your-username/Houses_in_London
cd Houses_in_London
pip install pandas numpy scikit-learn seaborn matplotlib scipy kneed
jupyter notebook Houses_in_London.ipynb
```

---

> *"KMeans did not fail — the data had nothing to discover."*  
> *"KMeans no falló — los datos no tenían nada que descubrir."*

---
*Unsupervised Learning Exercise | Sebastian — 2025*
