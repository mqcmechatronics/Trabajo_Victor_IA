# 1. Introducción
La Fórmula 1 es la máxima categoría del automovilismo mundial y constituye una importante fuente de información histórica relacionada con pilotos, escuderías, circuitos, carreras y resultados deportivos. Desde el inicio del campeonato en 1950, se han generado registros detallados que permiten analizar el desempeño de los diferentes participantes y estudiar la evolución de la competición a lo largo del tiempo.

La gran cantidad de datos disponibles convierte a la Fórmula 1 en un caso de estudio adecuado para la aplicación de técnicas de ciencia de datos e inteligencia artificial. El análisis de esta información permite identificar patrones, tendencias y relaciones entre variables que influyen en los resultados de las competencias, facilitando la generación de conocimiento a partir de datos históricos.
El presente proyecto tiene los siguientes objetivos:

## 1.1. Objetivo General
- Desarrollar un sistema de análisis y predicción basado en datos históricos de la Fórmula 1.
## 1.2. Objetivos específicos
- Integrar y preparar datos históricos de la Fórmula 1 a partir de archivos CSV y una base de datos relacional en SQLite.
- Visualizar y analizar la evolución histórica del campeonato mediante técnicas de análisis y representación de datos.
- Aplicar modelos de aprendizaje automático para la identificación de patrones y la generación de predicciones.
- Desarrollar una solución inteligente para la consulta, análisis y presentación de información relacionada con la Fórmula 1.

## 1.3. Alcance

El proyecto usa la versión 24 del dataset de Kaggle, que cubre las temporadas 1950 a 2024. Los 14 CSV originales se
incluyen en el repositorio (`data/raw/`) para que cualquiera obtenga exactamente los mismos resultados. Todo se ejecuta
en una computadora personal: la base de datos es un archivo SQLite, MLflow y Ollama corren como servicios locales y el
LLM es un modelo pequeño (`llama3.2:3b`, 2 GB) que no necesita una GPU dedicada.

Asimismo, el proyecto contempla la aplicación de técnicas de aprendizaje automático para el análisis de patrones y la generación de predicciones, así como la implementación de un asistente conversacional basado en un modelo de lenguaje local para la consulta de información mediante lenguaje natural. Toda la solución se ejecuta en un entorno local y reproducible, permitiendo su funcionamiento en una computadora personal sin requerir infraestructura especializada.

## 1.4. Organización del trabajo

El trabajo se divide en cinco notebooks de Jupyter Lab, que se ejecutan en orden:

| Notebook | Contenido |
|---|---|
| `01_construccion_bd` | Revisión del dataset, división entre base de datos y CSV, construcción y validación de la base de datos |
| `02_analisis_visualizaciones` | Lectura de ambas fuentes, análisis de columnas, limpieza, integración, visualizaciones y dataset final |
| `03_clustering` | Perfil de cada piloto y agrupamiento con K-Means, comparado con otros algoritmos |
| `04_clasificacion` | Clasificador de podio con variables pre-carrera y partición temporal |
| `05_llm` | Asistente con herramientas SQL y texto-a-SQL, evaluado con 34 preguntas |

El código que comparten los notebooks, la API y la aplicación está en `src/f1/`. Los pasos para ejecutar el proyecto
están en el [README del repositorio](https://github.com/vchirinosb/f1-championship#ejecución).

La sección [2](2-Marco-teorico) describe las tecnologías usadas, la [3](3-Descripcion-del-dataset) el dataset, la
[4](4-Pasos-realizados) los pasos realizados y las visualizaciones, la [5](5-Conclusiones) las conclusiones y la
[6](6-Bibliografia) la bibliografía.
