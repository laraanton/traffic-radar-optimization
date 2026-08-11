# 🚦 Optimización de la ubicación de radares para la prevención de accidentes de tráfico en Madrid

<p align="center">
  <b>Análisis espacial · Detección de puntos negros · Grafos · Optimización</b>
</p>

---

## 📌 Sobre el proyecto

Este proyecto analiza los **accidentes de tráfico registrados en Madrid** con el objetivo de identificar las zonas de mayor riesgo y proponer **25 ubicaciones óptimas para nuevos radares**.

Se combinan técnicas de **análisis de datos, análisis espacial, modelado de redes y optimización**.

El análisis utiliza accidentes registrados entre **2019 y 2025**, identifica puntos negros y calcula su peligrosidad. Posteriormente, esta información se integra en la red viaria para seleccionar las ubicaciones más adecuadas para nuevos radares.

> **Objetivo:** maximizar la cobertura de las zonas de mayor riesgo evitando una concentración excesiva de radares.

---

# 🎯 Objetivos

- Analizar los accidentes de tráfico registrados en Madrid.
- Detectar concentraciones espaciales de accidentes.
- Identificar y clasificar **puntos negros** según su peligrosidad.
- Modelar la red viaria mediante grafos.
- Integrar los accidentes con la red de carreteras.
- Incorporar información sobre tráfico y radares existentes.
- Evaluar posibles ubicaciones mediante diferentes factores.
- Seleccionar **25 ubicaciones para nuevos radares** mediante un algoritmo greedy.
- Crear mapas interactivos para visualizar los resultados.

---

# 📊 Datos

El proyecto utiliza diferentes fuentes relacionadas con la seguridad vial y la movilidad en Madrid.

### 🚗 Accidentes de tráfico

El dataset principal contiene accidentes registrados entre **2019 y 2025**, con un total de **317.844 registros**.

Entre las variables utilizadas se encuentran:

- Fecha y hora
- Localización y coordenadas
- Distrito
- Tipo de accidente
- Tipo de vehículo
- Personas implicadas
- Gravedad
- Alcohol y drogas

También se utiliza un dataset independiente de accidentes correspondiente a **2026**.

### 🛣️ Información adicional

Se incorporan datos sobre:

- Intensidad de tráfico.
- Radares existentes.
- Cruces semaforizados.
- Red viaria de Madrid.

Las principales fuentes son el **Portal de Datos Abiertos del Ayuntamiento de Madrid**, OpenStreetMap y Geofabrik.

---

# 🔬 Metodología

El proyecto se divide en cuatro etapas:

### 1️⃣ Detección de puntos negros

Los accidentes se agrupan mediante una **rejilla de 90 × 90 metros** para detectar zonas con alta concentración de accidentes.

Se identifican **7.763 puntos negros**, clasificados en cuatro niveles de peligrosidad:

**CRÍTICO · ALTO · MEDIO · BAJO**

La clasificación considera el número y la gravedad de los accidentes.

### 2️⃣ Integración con la red viaria

La red de carreteras de Madrid se modela como un **grafo utilizando OSMnx y NetworkX**.

Los puntos negros se asignan a los nodos de la red cuando se encuentran a menos de **50 metros**, incorporando así la información de peligrosidad a las vías.

### 3️⃣ Evaluación de ubicaciones

Se generan candidatos para nuevos radares y cada uno recibe una puntuación basada en:

- Peligrosidad.
- Tipo de vía.
- Velocidad.
- Intensidad de tráfico.
- Conectividad.
- Categoría del nodo.

### 4️⃣ Optimización

Se aplica un **algoritmo greedy de cobertura máxima ponderada** para seleccionar las mejores ubicaciones.

Se utiliza un **radio de exclusión de 500 metros** para evitar que los radares queden demasiado próximos.

El proceso continúa hasta seleccionar las **25 ubicaciones finales**.

---

# 🗺️ Resultados interactivos

Los resultados se representan mediante **mapas interactivos desarrollados con Folium y Leaflet**.

### 🔴 Mapa de puntos negros

Permite explorar la distribución espacial de los puntos negros de Madrid y consultar información sobre su peligrosidad.

👉 **[🗺️ Abrir mapa interactivo de puntos negros](maps/black_spots.html)**

---

# 📓 Notebooks

El análisis está dividido en tres notebooks:

### 01 — Análisis de puntos negros

Procesamiento de accidentes, análisis espacial, creación de la rejilla y clasificación de los puntos negros.

👉 [`01_analisis_puntos_negros.ipynb`](notebooks/01_analisis_puntos_negros.ipynb)

### 02 — Modelado de la red viaria

Construcción del grafo de la red viaria y asignación de los puntos negros.

👉 [`02_modelado_red_viaria.ipynb`](notebooks/02_modelado_red_viaria.ipynb)

### 03 — Optimización de radares

Evaluación de candidatos y aplicación del algoritmo greedy para seleccionar las 25 ubicaciones.

👉 [`03_optimizacion_radares.ipynb`](notebooks/03_optimizacion_radares.ipynb)

---

# 📈 Principales resultados

| Resultado | Valor |
|---|---:|
| Accidentes históricos procesados | **317.844** |
| Periodo principal | **2019–2025** |
| Accidentes utilizados en el análisis espacial | **140.872** |
| Puntos negros detectados | **7.763** |
| Puntos críticos | **439** |
| Ubicaciones de radares propuestas | **25** |
| Radio de exclusión | **500 m** |

---

# 🧰 Tecnologías

### Lenguaje
- Python

### Análisis de datos
- Pandas
- NumPy
- Scikit-learn

### Análisis espacial
- GeoPandas
- Folium
- QGIS

### Redes y grafos
- NetworkX
- OSMnx

### Algoritmos
- BallTree
- Distancia Haversine
- Índices espaciales
- Algoritmo Greedy

### Herramientas
- Jupyter Notebook
- Visual Studio Code
- Git
- GitHub

---

# 📁 Estructura del repositorio

```text
madrid-traffic-radar-optimization/
│
├── README.md
│
├── notebooks/
│   ├── 01_analisis_puntos_negros.ipynb
│   ├── 02_modelado_red_viaria.ipynb
│   └── 03_optimizacion_radares.ipynb
│
├── maps/
│   └── black_spots.html
│
├── data/
│   ├── README.md
│   └── puntos_negros_nodos.csv
│
└── docs/
    └── memoria.pdf

---

---

# 📄 Memoria del proyecto

La memoria contiene la explicación completa de la metodología, el procesamiento de los datos, el modelado de la red, el algoritmo de optimización y los resultados obtenidos.

👉 **[📄 Consultar memoria completa](docs/grupo_D_Antón_Lara.pdf)**

---

# 🔎 Limitaciones y posibles mejoras

Algunas posibles líneas de trabajo futuro son:

* Incorporar información de tráfico más detallada.
* Realizar un análisis de sensibilidad de los pesos del índice de peligrosidad.
* Estudiar diferentes radios de exclusión.
* Incorporar condiciones meteorológicas y características de la vía.
* Introducir medidas de exposición al tráfico.
* Comparar el algoritmo greedy con otros métodos de optimización.
* Incorporar medidas de centralidad de grafos.
* Actualizar el modelo con nuevos datos de accidentes.

---

# 👥 Autores

**Lara Antón Güemes**
**Hugo García Gutierrez**
**Juan Manuel de Miguel**
**Nuria García Arnaíz**
**Daniela Pino Criado**
**Yonathan Bautista Pilar**

**Universidad de León**
Escuela de Ingenierías Industrial, Informática y Aeroespacial

**Asignatura:** Matemática Finita II
**Curso académico:** 2025–2026

