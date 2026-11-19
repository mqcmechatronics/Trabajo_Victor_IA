# Campeonato Mundial de Fórmula 1: análisis, modelos y asistente

## Descripción

El presente proyecto desarrolla una solución de análisis y predicción basada en datos históricos del Campeonato Mundial de Fórmula 1. Se integran técnicas de ciencia de datos, aprendizaje automático y modelos de lenguaje para el procesamiento, análisis y consulta de información histórica del campeonato.

La solución incorpora una base de datos SQLite, herramientas de visualización, modelos de machine learning registrados en MLflow, un asistente conversacional basado en Ollama, una API desarrollada en Flask y una interfaz de usuario implementada en Streamlit.

La documentación completa del proyecto se encuentra disponible en la [Wiki del repositorio](https://github.com/vchirinosb/f1-championship/wiki).

## Curso
Herramientas para Inteligencia Artificial - Maestría en Inteligencia Artificial Aplicada UCSM - UTPL

## Dataset

Se utiliza el conjunto de datos Formula 1 World Championship (1950-2024), disponible en Kaggle. [Formula 1 World Championship (1950 -
2024)](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020) de Kaggle, **versión 24**.
El proyecto integra información histórica de pilotos, carreras, circuitos, escuderías y resultados deportivos, consolidando los datos en una base de datos SQLite para su posterior análisis.

## Estructura del repositorio

```
data/raw/            14 CSV originales (dataset inicial)
data/f1.db           base de datos SQLite
data/final/          dataset final
data/eval/           preguntas de evaluación del asistente LLM
sql/                 schema.sql (tablas), views.sql (vistas) y queries.sql (consultas de ejemplo)
scripts/             build_database.py (construye la base de datos)
src/f1/              código compartido: lectura de datos, variables, gráficos, MLflow y asistente LLM
notebooks/           notebooks 01 a 05
api/                 API Flask
app/                 aplicación Streamlit
tests/               pruebas con pytest
images/              gráficos exportados como PNG (notebooks y wiki)
docs/wiki/           páginas del informe que se publican en la Wiki
```

## Estructura de los notebooks

Cada notebook empieza con un encabezado (maestría, curso, repositorio, plataformas y librerías) y termina con
conclusiones. Deben ejecutarse en orden: cada uno usa lo que genera el anterior.

## Notebooks desarrollados

01. Construcción de la base de datos. `01_construccion_bd.ipynb`
02. Análisis exploratorio y visualizaciones. `02_analisis_visualizaciones.ipynb` (notebook principal)
03. Clustering de pilotos. `03_clustering.ipynb`
04. Clasificación de podios. `04_clasificacion.ipynb`
05. Asistente basado en LLM. `05_llm.ipynb`

## Ejecución

Todos los comandos se ejecutan desde la raíz del repositorio. Los servicios usan estos puertos:

| Servicio | Puerto | Dirección |
|---|---|---|
| MLflow | 9090 | http://127.0.0.1:9090 |
| Ollama | 11434 | http://127.0.0.1:11434 |
| API Flask | 5001 | http://127.0.0.1:5001 |
| Streamlit | 8501 | http://localhost:8501 |
| Jupyter Lab | 8888 | http://localhost:8888 |

### 1. Instalar las dependencias

Se necesita Python 3.13 y [Ollama](https://ollama.com). El entorno virtual de este proyecto está en la carpeta que
contiene al repositorio, por eso los comandos usan `../bin/`; si se crea en otro lugar, hay que cambiar esa ruta.

```bash
python3.13 -m venv ..
```

```bash
../bin/pip install -r requirements.txt
```

Playwright necesita su navegador para exportar los gráficos de Bokeh como PNG:

```bash
../bin/playwright install chromium
```

### 2. Construir la base de datos

Crea `data/f1.db` a partir de los CSV de `data/raw/` (el notebook 01 también lo hace):

```bash
../bin/python scripts/build_database.py
```

### 3. Iniciar MLflow

Los notebooks 03 a 05 y la API usan un servidor local de MLflow. En una terminal aparte:

```bash
../bin/mlflow server --backend-store-uri sqlite:///mlflow.db --host 127.0.0.1 --port 9090
```

Las ejecuciones se guardan en `mlflow.db` y los artefactos en `mlartifacts/`; ambos están en `.gitignore`. Para usar
otro servidor, definir la variable `MLFLOW_TRACKING_URI`. El módulo `src/f1/tracking.py` concentra el uso de MLflow.

### 4. Iniciar Ollama

El asistente usa el modelo local `llama3.2:3b`. Descargarlo una vez y dejar Ollama en ejecución en otra terminal:

```bash
ollama pull llama3.2:3b
```

```bash
ollama serve
```

Para usar otro servidor de Ollama, definir la variable `OLLAMA_HOST`.

### 5. Ejecutar los notebooks en orden

Abrir Jupyter Lab y ejecutar `01` a `05`, en ese orden:

```bash
../bin/jupyter lab notebooks
```

O ejecutarlos todos desde la terminal, guardando las salidas en cada notebook:

```bash
../bin/jupyter nbconvert --to notebook --execute --inplace notebooks/0*.ipynb
```

El notebook 02 genera el dataset final y los gráficos de `images/`; los notebooks 03 y 04 registran los modelos
`f1-driver-clustering` y `f1-podium-classifier` en MLflow con el alias `champion`, que es la versión que carga la API.
El notebook 05 evalúa el asistente con las preguntas de `data/eval/preguntas_llm.csv`.

### 6. Iniciar la API Flask

Con MLflow en ejecución (para cargar los modelos `champion`), en otra terminal:

```bash
../bin/python api/app.py
```

También se puede iniciar con `../bin/flask --app api.app:create_app run --port 5001`. Si MLflow no está disponible al
iniciar, las predicciones responden 503 y el resto de los endpoints sigue funcionando. La API es la única capa que
consulta la base de datos, MLflow y Ollama.

| Método | Ruta | Devuelve |
|---|---|---|
| GET | `/health` | Estado de la BD, MLflow y Ollama, versiones de los modelos y conteos del dataset |
| GET | `/races?year=2023` | Carreras de una temporada (sin `year`, todas) |
| GET | `/races/<race_id>/results` | Resultados de una carrera |
| GET | `/winner?circuit=monza&year=2019` | Ganador por circuito o nombre de carrera y año |
| GET | `/drivers/<driver_ref>/cluster` | Grupo del clustering, perfil del piloto y promedios del grupo |
| POST | `/predict/podium` | Probabilidad de podio con `{"race_id", "driver_id"}` o con las variables del modelo |
| POST | `/ask` | Respuesta del asistente LLM a `{"question": "..."}` |

```bash
curl "http://127.0.0.1:5001/winner?circuit=monza&year=2019"
```

### 7. Iniciar la aplicación Streamlit

Con la API en ejecución, en otra terminal:

```bash
../bin/streamlit run app/streamlit_app.py
```

La aplicación tiene cuatro páginas: **Inicio** (cifras del dataset), **Explorar** (resultados por temporada y
carrera), **Predicciones** (probabilidad de podio y tipo de piloto) y **Asistente** (chat con el LLM). La barra
lateral muestra el estado de la base de datos, MLflow y Ollama; si un servicio no está disponible, la página que lo
usa muestra un aviso. Para usar otra URL de la API, definir `F1_API_URL`.

### 8. Ejecutar las pruebas

Las pruebas de `tests/` usan la base de datos y Ollama; las de MLflow usan una base temporal:

```bash
../bin/python -m pytest tests
```
