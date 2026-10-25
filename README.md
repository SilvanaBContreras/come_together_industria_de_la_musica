# "Come Together": cruzando métricas de streaming, playlists, redes sociales y conciertos en busca de diferencias sistemáticas entre artistas musicales

Este repositorio contiene el código de extracción, preprocesamiento y modelado desarrollado para el Trabajo Final Integrador de la Carrera de Especialización en Explotación de Datos y Descubrimiento del Conocimiento, primer año de la Maestría en Explotación de Datos y Descubrimiento del Conocimiento, Facultad de Ciencias Exactas y Naturales, Universidad de Buenos Aires (UBA).

**Autora:** Silvana Contreras
**Fecha:** octubre de 2026

## 📌 Resumen del proyecto

El trabajo analiza qué perfiles de artistas emergen de la combinación de métricas digitales (*streaming*, *playlists*, YouTube y redes sociales), cómo se distribuye la actividad de conciertos documentados dentro de cada perfil y qué variables y dimensiones se asocian más con esa actividad.

Con el artista como unidad de análisis, se construyó un dataset propio de 7.000 artistas únicos de alta visibilidad global. Las variables se organizan en tres dimensiones de la industria y sus subdimensiones operativas:

| Dimensión | Subdimensión | Ejemplos |
|---|---|---|
| Fonogramas | *Streaming* | Oyentes y seguidores en Spotify, reproducciones en Pandora, Deezer, Shazam |
| | *Playlists* | Presencia y alcance en playlists de Spotify, Apple Music, Amazon Music, Deezer y YouTube |
| | YouTube | Suscriptores y visualizaciones del canal |
| Medios | Redes sociales | Seguidores e interacción en Instagram, TikTok y Twitter/X |
| Medios y fonogramas | *Ratios* entre plataformas | Seguidores de Instagram en relación con oyentes de Spotify |
| Conciertos | Actividad en vivo | Shows documentados en 2024-2025 (`n_shows_24_25`, variable objetivo) |
| Perfil del artista | Metadata | Género musical, territorio, formato banda o solista, sello discográfico |

**Hallazgo principal:** la actividad en vivo no se organiza alrededor de una métrica única. Medido con SHAP agrupado, las redes sociales aportan a las predicciones tanto como el conjunto de las métricas de *streaming*, *playlists* y YouTube, en línea con la hipótesis de un ecosistema multiplataforma.

## 🔒 Política de acceso a los datos

El dataset resultante se denomina *Top Music Artists 2026: A Cross-Platform Dataset of 7,000 Music Artists Across Streaming, Live Events, and Social Media*. 
El listado inicial de artistas proviene del dataset público Make Music Equal (CC BY-SA 4.0). Las salidas de las notebooks se depuraron para no exponer valores individuales por artista.

> ⚠️ **Disponibilidad del dataset.** Los datos provienen de la API de Chartmetric, fuente privada líder en analítica de la industria musical. Por sus términos de servicio, los datos individuales no pueden publicarse ni distribuirse en este repositorio.

Lo que sí se pone a disposición es el código completo del procedimiento:

- extracción automatizada desde los *endpoints* de metadata y eventos en vivo,
- preprocesamiento, agregación y transformación de variables,
- construcción de *features*, análisis y modelado.

Quien cuente con credenciales válidas de la API de Chartmetric puede usar este código para reconstruir y actualizar el dataset.

## 🗂️ Estructura del repositorio

Las notebooks se numeran según el orden de ejecución (`CT`: *Come Together*):

| Etapa | Notebook | Contenido |
|---|---|---|
| 1. Construcción del dataset | `1A_CT_seleccion_artistas.ipynb` | Selección de los 7.000 artistas a partir del dataset público *Make Music Equal* (CC BY-SA 4.0) |
| | `1B_CT_extraccion_api.ipynb` | Extracción de datos por API, por cohortes de artistas |
| | `1C_CT_dataset_cohorte1.ipynb`, `1D_CT_dataset_cohorte2.ipynb` | Armado del dataset a partir de las respuestas de la API y agregación de eventos en vivo |
| | `1E_CT_dataset_final.ipynb` | Construcción de la variable objetivo, tratamiento de faltantes y dataset final |
| 2. Análisis exploratorio | `2A_CT_EDA.ipynb` | Variables por dimensión, distribuciones, correlaciones y redundancia |
| | `2B_CT_categoricas_multivariado.ipynb` | Análisis multivariado de variables categóricas |
| 3. No supervisado | `3A_CT_clustering_jerarquico.ipynb` | Exploración inicial: clustering jerárquico |
| | `3B_CT_clustering_perfiles.ipynb` | K-Medoids, GMM y *Spectral Clustering*; solución final con k = 3 |
| 4. Supervisado | `4A_CT_modelos_estadisticos.ipynb` | OLS sobre `log1p`, binomial negativa y modelo de dos etapas |
| | `4B_CT_lgbm_regression.ipynb` | LightGBM principal, importancias por *gain*, SHAP y lectura agrupada por dimensión |
| | `4C_CT_lgbm_poisson.ipynb`, `4D_CT_lgbm_tweedie.ipynb` | LightGBM con objetivos Poisson y Tweedie (análisis de sensibilidad) |

Archivos de datos esperados en la raíz (no incluidos): mme_artist_info_DATASET.csv (Make Music Equal), dataset_7000_53.csv (dataset completo) y dataset_4913_53.csv (artistas con dato en la variable objetivo)
Carpetas y archivos auxiliares:

- `graficos/`: figuras generadas por las notebooks.
- `modelitos/<experimento>/{dev,full}`: modelos LightGBM entrenados (ensambles de 100 semillas), por objetivo (`lgbm-regression`, `lgbm-poisson`, `lgbm-tweedie`).
- `*_optuna.db`: estudios de optimización bayesiana de hiperparámetros.

## 🛠️ Metodología

- **Análisis no supervisado.** Clustering sobre el espacio digital, excluyendo deliberadamente la actividad en vivo. La solución final, *Spectral Clustering* con k = 3, identifica tres perfiles (Headliners, Performers y Streamers), cuya actividad en vivo se analiza *a posteriori*.
- **Modelado supervisado** de la variable de conteo `n_shows_24_25`:
  - **Modelos estadísticos:** OLS sobre `log1p`, binomial negativa y un modelo de dos etapas (ocurrencia con regresión logística e intensidad con binomial negativa).
  - **LightGBM:** captura relaciones no lineales e interacciones. Se optimizó con Optuna y se entrenó en ensambles multisemilla; las importancias se interpretan por *gain* y se contrastan con SHAP agrupado por dimensión.

## ⚙️ Entorno

Python 3.13 con pandas 3.0, NumPy 2.4, SciPy 1.17, scikit-learn 1.8, statsmodels 0.14, LightGBM 4.6, Optuna 4.9, umap-learn 0.5, matplotlib 3.10 y seaborn 0.13. Los valores SHAP se calculan con LightGBM (pred_contrib), sin la librería shap.

Las versiones exactas están en `requirements.txt`:

```
pip install -r requirements.txt
```

Los modelos entrenados (`modelitos/`) no se incluyen en el repositorio; se regeneran al ejecutar las notebooks de boosting.

## 📩 Contacto

- LinkedIn: [Silvana Contreras](https://www.linkedin.com/in/silvanacontrerasok/)
- GitHub: [@SilvanaBContreras](https://github.com/SilvanaBContreras)
- Email: scontreras@liaa.dc.uba.ar
- Substack: [POLEA](https://poleaia.substack.com), blog sobre inteligencia artificial, estudios de frontera, impacto y ciencia argentina
