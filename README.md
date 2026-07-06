# "Come Together": cruzando métricas de streaming, playlists, redes sociales y conciertos en busca de diferencias sistemáticas entre artistas musicales.

Este repositorio contiene el código de extracción, preprocesamiento y modelado desarrollado para la entrega final del **Taller de Tesis I** de la **Maestría en Explotación de Datos y Descubrimiento del Conocimiento (DMEyF)** de la Facultad de Ciencias Exactas y Naturales, Universidad de Buenos Aires (UBA).

**Autora:** Silvana Contreras  
**Fecha:** Julio 2026 

---

## 📌 Resumen del Proyecto

El objetivo central de este trabajo es analizar qué perfiles emergen de la combinación de métricas digitales, streaming, playlists, YouTube y redes sociales, y cómo se distribuye la actividad en vivo documentada dentro de cada perfil. Además, se busca identificar qué variables
del ecosistema digital están más asociadas al nivel de actividad en vivo de un artista y qué dimensión tiene mayor capacidad explicativa.

A partir de un enfoque centrado en el artista y no en las canciones, se construyó un dataset propio con **7.000 artistas únicos de alta visibilidad global**. 

El análisis integra señales distribuidas en múltiples dimensiones conceptuales:
* **Streaming:** Seguidores y oyentes mensuales en plataformas como Spotify, Deezer, Amazon Music, Apple Music y otras.
* **Playlists:** Alcance y presencia en listas editoriales y curatoriales de múltiples plataformas.
* **Redes Sociales:** Audiencia e interacciones en plataformas como TikTok e Instagram.
* **YouTube:** Actividad y visualizaciones de contenido audiovisual.
* **Metadata:** Atributos descriptivos de los artistas (género musical, territorio de origen, posición industrial.
* **Conciertos:** Shows documentados durante el periodo 2024-2025 (`n_shows_24_25`)

---

## 🔒 Política de Acceso a los Datos

El conjunto de datos resultante de esta investigación se denomina `Top Music Artists 2026: A Cross-Platform Dataset of 7,000 Music Artists Across Streaming, Live Events, and Social Media`. 

⚠️ **Nota Importante sobre la Disponibilidad del Dataset:** > Debido a los términos de servicio y las estrictas restricciones de la API cerrada de **Chartmetric** (fuente global cross-platform líder utilizada para la recolección) [cite: 4][cite_start], **los datos crudos individuales no pueden ser publicados de forma abierta ni distribuirse públicamente en este repositorio**.
>
> Para garantizar la total **transparencia y replicabilidad** de la investigación sin vulnerar la propiedad intelectual de la fuente, se provee el ecosistema completo de código en Python:
> 1. Los scripts de extracción y consulta automatizada a los endpoints de metadata y eventos en vivo.
> 2. Los pipelines de preprocesamiento, agregación y transformación de variables.
> 3. El código completo de feature engineering y modelado.

Cualquier usuario que cuente con credenciales de acceso válidas a la API de Chartmetric podrá utilizar las herramientas aquí provistas para reconstruir y actualizar el dataset.

---

## 🛠️ Metodología y Modelos (Python)

El proyecto implementa una estrategia analítica complementaria en dos grandes etapas:

1. **Análisis No Supervisado (Clustering):** Se aplicaron múltiples algoritmos de agrupamiento sobre el espacio digital cross-platform excluyendo deliberadamente la actividad en vivo. 
2. **Modelado Supervisado:** Diseñado para modelar la variable objetivo de conteo `n_shows_24_25` (shows documentados en el bienio 2024-2025).
    * **Modelos Baselines:** Se ajustaron modelos de Regresión Lineal (OLS sobre escala `log1p`), Regresión Binomial Negativa y un **Modelo de Dos Etapas (*Hurdle Model*).
    * **Modelo LightGBM:** para capturar interacciones complejas de alto orden y relaciones no lineales.
  
---

## 📩 Contacto Profesional

* **LinkedIn:** [Silvana Contreras](https://www.linkedin.com/in/silvanacontrerasok/)
* **GitHub:** [@SilvanaBContreras](https://github.com/SilvanaBContreras)
* **Email:** [scontreras@liaa.dc.uba.ar](mailto:scontreras@liaa.dc.uba.ar)
* **Substack:** [POLEA](https://poleaia.substack.com) – Publicación sobre inteligencia artificial, estudios de frontera y ciencia argentina.
