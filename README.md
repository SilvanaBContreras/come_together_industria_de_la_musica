# "Come Together": cruzando métricas de streaming, playlists, redes sociales y conciertos en busca de diferencias sistemáticas entre artistas de la industria de la música

[cite_start]Este repositorio contiene el código de extracción, preprocesamiento y modelado desarrollado para la entrega final del **Taller de Tesis I** de la **Maestría en Explotación de Datos y Descubrimiento del Conocimiento (DMEyF)** de la Facultad de Ciencias Exactas y Naturales, Universidad de Buenos Aires (UBA).

**Autora:** Silvana Contreras  
[cite_start]**Fecha:** Julio 2026 

---

## 📌 Resumen del Proyecto

[cite_start]El objetivo central de este trabajo es analizar cómo se relacionan la huella digital multiplataforma de un artista musical y su actividad en vivo documentada. [cite_start]A partir de un enfoque centrado en el artista y no en las canciones, se construyó un dataset propio con **7.000 artistas únicos de alta visibilidad global**. 

El análisis integra señales distribuidas en múltiples dimensiones conceptuales:
* [cite_start]**Streaming:** Seguidores y oyentes mensuales en plataformas como Spotify[cite: 3].
* [cite_start]**Playlists:** Alcance y presencia en listas editoriales y curatoriales[cite: 3].
* [cite_start]**Redes Sociales:** Audiencia e interacciones en plataformas clave como TikTok e Instagram[cite: 3].
* [cite_start]**YouTube:** Actividad y visualizaciones de contenido audiovisual[cite: 3].
* [cite_start]**Metadata:** Atributos descriptivos de los artistas (género, territorio de origen, posición industrial)[cite: 3].
* [cite_start]**Conciertos:** Shows documentados durante el periodo 2024-2025 (`n_shows_24_25`)[cite: 3, 4].

---

## 🔒 Política de Acceso a los Datos

[cite_start]El conjunto de datos resultante de esta investigación se denomina `Top Music Artists 2026: A Cross-Platform Dataset of 7,000 Music Artists Across Streaming, Live Events, and Social Media`. 

> [cite_start]⚠️ **Nota Importante sobre la Disponibilidad del Dataset:** > Debido a los términos de servicio y las estrictas restricciones de la API cerrada de **Chartmetric** (fuente global cross-platform líder utilizada para la recolección) [cite: 4][cite_start], **los datos crudos individuales no pueden ser publicados de forma abierta ni distribuirse públicamente en este repositorio**.
>
> Para garantizar la total **transparencia y replicabilidad** de la investigación sin vulnerar la propiedad intelectual de la fuente, se provee el ecosistema completo de código en Python:
> [cite_start]1. Los scripts de extracción y consulta automatizada a los endpoints de metadata y eventos en vivo.
> [cite_start]2. Los pipelines de preprocesamiento, agregación y transformación de variables.
> [cite_start]3. El código completo de feature engineering y modelado.

[cite_start]Cualquier usuario que cuente con credenciales de acceso válidas a la API de Chartmetric podrá utilizar las herramientas aquí provistas para reconstruir y actualizar el dataset.

---

## 🛠️ Metodología y Modelos (Python)

El proyecto implementa una estrategia analítica complementaria en dos grandes etapas utilizando librerías estándar del ecosistema de ciencia de datos (`pandas`, `scikit-learn`, `lightgbm`, `UMAP`):

1. [cite_start]**Análisis No Supervisado (Spectral Clustering):** Se aplicó clustering sobre el espacio digital cross-platform excluyendo deliberadamente la actividad en vivo. [cite_start]Se identificó un continuo de integración multiplataforma estructurado en tres perfiles analíticos que muestran una clara gradación *a posteriori* en su cantidad mediana de shows ejecutados: **Superestrellas**, **Constelación** y **Screen Natives**.
2. [cite_start]**Modelado Supervisado (LightGBM):** Modelos basados en árboles de decisión optimizados mediante optimización bayesiana para explicar las diferencias sistemáticas en el volumen de conciertos (`n_shows_24_25`)[cite: 1, 4]. [cite_start]Como análisis de sensibilidad, se evaluaron especificaciones de regresión en escala logarítmica (`log1p`), así como funciones de pérdida de tipo Poisson y Tweedie para el manejo de la asimetría y el exceso de ceros en la variable objetivo.

### 📊 Resultados Destacados
* [cite_start]El modelo principal **LightGBM** superó significativamente a los baselines basales, logrando una **reducción del RMSE del 20,2%** y una **reducción del MAE del 26,6%** en el conjunto de *holdout*.
* [cite_start]Los análisis de importancia de variables (*feature importance* por ganancia media) confirmaron la hipótesis de **ecosistema multiplataforma**, evidenciando que el peso explicativo se reparte de forma distribuida entre redes sociales, playlists, streaming y metadata, sin la existencia de un predictor hegemónico.

---

## 📁 Estructura del Repositorio

Siguiendo las buenas prácticas para proyectos de Machine Learning, la estructura del código se organiza de la siguiente manera:

```text
├── data_extraction/         # Scripts para interactuar con la API de Chartmetric (Metadata y Events)
│   └── extract_artists.py
├── preprocessing/           # Limpieza, agregación a nivel artista e imputación de fuentes secundarias
│   ├── build_target.py      # Construcción de la variable n_shows_24_25 e integraciones auxiliares
│   └── transform_features.py # Aplicación de transformaciones logarítmicas y cálculo de ratios cruzados
├── models/                  # Pipelines de entrenamiento y optimización de hiperparámetros
│   ├── unsupervised.py      # Configuración de Spectral Clustering y proyecciones UMAP
│   └── supervised_lgb.py    # Optimización Bayesiana y entrenamiento del ensamble LightGBM
├── notebooks/               # Jupyter Notebooks para Análisis Exploratorio de Datos (EDA) y gráficos
├── requirements.txt         # Dependencias necesarias para la ejecución del proyecto
└── README.md                # Presentación del repositorio
