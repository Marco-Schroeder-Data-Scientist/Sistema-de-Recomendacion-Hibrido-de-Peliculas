# Sistema de Recomendacion Hibrido de Peliculas

![Python](https://img.shields.io/badge/Python-3.13%2B-blue)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Streamlit](https://img.shields.io/badge/Streamlit-deployed-brightgreen)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0%2B-blue)

## Demo en vivo
Prueba el recomendador interactivo aqui:
[Abrir app en Streamlit Cloud](https://movie-recommendation-system-hybrid-app-znywwpw5eiat5bjjduciqv.streamlit.app/)

**Autor:** Marco Schroeder Labrin
**Fecha:** Febrero 2026
**Objetivo:** Construir un sistema de recomendacion hibrido que combine filtrado
colaborativo, basado en contenido y popularidad ponderada, utilizando datos reales
de The Movies Dataset (TMDB + MovieLens, Kaggle).

---

## Descripcion

Proyecto end-to-end de recomendacion de peliculas que cubre todo el flujo:
ETL y limpieza de datos reales complejos, EDA completo, feature engineering,
modelado con multiples enfoques y despliegue de una aplicacion interactiva
en Streamlit con 4 modos de recomendacion.

---

## Tecnologias utilizadas

- Python 3.13
- pandas, numpy, scikit-learn
- implicit (BPR — Bayesian Personalized Ranking)
- matplotlib, seaborn
- Streamlit (deploy interactivo)

---

## Estructura del Proyecto

```
├── movies_metadata.csv           # Dataset principal (metadata de peliculas)
├── credits.csv                   # Cast y crew
├── links_small.csv               # Mapeo movieId <-> tmdbId
├── ratings_small.csv             # Calificaciones de usuarios (MovieLens)
├── app.py                        # Aplicacion interactiva en Streamlit
├── recomendacion_contenido.ipynb # Notebook completo (ETL -> EDA -> Modelado)
├── df_content_final.pkl          # DataFrame procesado
├── bpr_model_final.pkl           # Modelo BPR entrenado
├── user_map.pkl                  # Mapas de indices para collaborative filtering
├── item_map.pkl
├── inv_item_map.pkl
├── interactions.pkl
└── README.md
```

---

## Flujo del Proyecto

### 1. ETL y Limpieza
- Carga de ~45k peliculas con tratamiento de nulos (imputacion por mediana/moda)
- Conversion de tipos (fechas, categoricas, enteros nullable)
- Parseo de columnas JSON-like (genres → lista de nombres)
- Eliminacion de duplicados por id y titulo+año

### 2. EDA
- Univariado: distribuciones, deteccion y tratamiento de outliers
- Bivariado: correlaciones Spearman, boxplots por genero, tendencias temporales
- Outliers eliminados: runtime > 400 min

### 3. Preparacion y Feature Engineering
- Filtro conservador: solo peliculas Released, runtime 10–400 min, >= 5 votos
- Score de popularidad ponderado (formula IMDb-like)
- Texto combinado para TF-IDF: overview + generos x3 + tagline + titulo +
  director + top 3 actores

### 4. Modelado

| Modelo | Descripcion |
|---|---|
| Popularidad ponderada | Baseline — formula IMDb-like |
| Content-based | TF-IDF + similitud coseno sobre metadata enriquecida |
| Hibrido content | Similitud coseno x score ponderado |
| Collaborative filtering | implicit BPR (factors=100, iterations=30) |

**Collaborative filtering — detalle:**
- Feedback implicito: rating >= 3.5 → confianza 1.0
- Algoritmo BPR (Bayesian Personalized Ranking) para metricas de ranking top-N

### 5. Despliegue
App interactiva en Streamlit con 4 modos:
- Popularidad ponderada (top peliculas generales)
- Recomendaciones por titulo (content-based)
- Recomendaciones personalizadas por userId (collaborative BPR)
- Modo hibrido (combinado)

---

## Resultados

- Dataset original: ~45k peliculas
- Dataset final filtrado: ~29.8k peliculas (post-limpieza y filtros)
- Subconjunto content-based: ~9.1k peliculas con >= 50 votos
- Cobertura cruce ratings-movieId: ~63% (suficiente para collaborative filtering)

**Validacion colaborativa — ejemplo userId=1:**
Recomendaciones recibidas: Pulp Fiction, Shawshank Redemption, Forrest Gump,
Star Wars, Back to the Future — alta coherencia con historial del usuario
(clasicos 80s-90s, aventura/sci-fi, crimen/thriller).

---

## Proximos Pasos

- Incorporar keywords.csv para enriquecer content-based
- Agregar evaluacion offline formal (NDCG@10, Precision@K)
- Fine-tuning de hiperparametros en BPR
- Visualizacion de posters (poster_path) en la app Streamlit

---

## Como ejecutar localmente

```bash
# 1. Clonar repositorio
git clone https://github.com/Marco-Schroeder-Data-Scientist/movie-recommendation-system-hybrid-streamlit

# 2. Crear entorno virtual
python -m venv .venv

# 3. Activar entorno
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # Linux/macOS

# 4. Instalar dependencias
pip install -r requirements.txt

# 5. Ejecutar app
streamlit run app.py
```

---

## Licencia

MIT License — libre para uso, modificacion y distribucion (atribucion apreciada).
Pull requests y sugerencias son bienvenidos.
