# 🚦 Optimización de la ubicación de radares para la prevención de accidentes de tráfico en Madrid

<p align="center">
  <b>Análisis espacial · Detección de puntos negros · Grafos · Optimización</b>
</p>

---

## 📌 Sobre el proyecto

Este proyecto analiza los **accidentes de tráfico registrados en Madrid** con el objetivo de identificar las zonas de mayor riesgo y proponer **25 ubicaciones óptimas para la instalación de nuevos radares**.

Para ello, se combinan técnicas de **análisis y procesamiento de datos, análisis espacial, modelado de redes y optimización**.

El proyecto parte de los accidentes históricos registrados entre **2019 y 2025**, identifica espacialmente los puntos negros y calcula un índice de peligrosidad. Posteriormente, esta información se integra en la red viaria de Madrid para evaluar diferentes tramos de carretera y seleccionar las ubicaciones más adecuadas para nuevos radares.

> **Objetivo:** maximizar la cobertura de las zonas de mayor peligrosidad evitando una concentración excesiva de radares.

---

# 🎯 Objetivos

* Analizar los accidentes de tráfico registrados en Madrid.
* Detectar concentraciones espaciales de accidentes.
* Identificar y clasificar **puntos negros** según su peligrosidad.
* Construir un modelo de la red viaria de Madrid mediante grafos.
* Integrar la información de accidentes con la red de carreteras.
* Incorporar información sobre tráfico y radares existentes.
* Evaluar los tramos candidatos mediante diferentes factores.
* Seleccionar **25 ubicaciones para nuevos radares** mediante un algoritmo greedy.
* Crear mapas interactivos para visualizar los resultados.

---

# 📊 Datos

El proyecto utiliza diferentes fuentes de información relacionadas con la seguridad vial y la movilidad en Madrid.

### 🚗 Accidentes de tráfico

El dataset histórico principal utilizado contiene accidentes registrados entre:

**2019 – 2025**

El conjunto procesado contiene **317.844 registros**.

Entre las variables disponibles se encuentran:

* Fecha y hora
* Localización
* Distrito
* Tipo de accidente
* Tipo de vehículo
* Personas implicadas
* Gravedad
* Alcohol
* Drogas
* Coordenadas geográficas

También se utiliza un dataset independiente de accidentes correspondiente a **2026**.

### 🛣️ Información adicional

Se incorporan además datos sobre:

* Intensidad de tráfico.
* Radares existentes.
* Cruces semaforizados.
* Red viaria de Madrid.

Las fuentes principales son el **Portal de Datos Abiertos del Ayuntamiento de Madrid**, OpenStreetMap y Geofabrik.

---

# 🔬 Metodología

## 1️⃣ Detección y clasificación de puntos negros

El análisis comienza con el procesamiento de los datos de accidentes de tráfico de Madrid. Tras la limpieza y eliminación de duplicados, los accidentes se agregan espacialmente mediante una **rejilla regular de aproximadamente 90 × 90 metros**. Esta discretización permite identificar zonas donde existe una concentración elevada de accidentes.

Se optó por este enfoque frente a métodos de clustering como DBSCAN debido al **efecto de chaining** observado en zonas con una elevada densidad de accidentes, donde diferentes agrupaciones podían terminar conectándose entre sí.

A partir de esta agregación espacial se identifican **7.763 puntos negros**, que posteriormente se clasifican en cuatro niveles de peligrosidad: **CRÍTICO, ALTO, MEDIO y BAJO**. La clasificación tiene en cuenta tanto el número de accidentes como su gravedad.

Para caracterizar cada punto negro se calcula además un **índice de peligrosidad** que combina diferentes variables relacionadas con los accidentes. El índice asigna un mayor peso a la frecuencia de accidentes y a su gravedad, incorporando también los accidentes relacionados con el consumo de alcohol.

## 2️⃣ Integración con la red viaria

Una vez identificadas las zonas de mayor peligrosidad, se incorpora la **red de carreteras de Madrid** para trasladar el análisis espacial de los accidentes al contexto de las vías donde estos se producen.

La red se representa mediante un grafo utilizando **OSMnx y NetworkX**, donde los nodos representan puntos de la red viaria y las aristas representan los diferentes tramos de carretera.

Los puntos negros se relacionan espacialmente con los nodos de esta red. Cuando un nodo se encuentra a una distancia inferior a **50 metros** de un punto negro, se le asigna la información de peligrosidad correspondiente. En caso de existir varios puntos cercanos, se considera el de mayor peligrosidad.

Este proceso permite pasar de un análisis basado únicamente en coordenadas geográficas a una representación de la peligrosidad directamente asociada a la **red viaria de Madrid**.

## 3️⃣ Evaluación de candidatos para nuevos radares

A partir de la red viaria enriquecida con la información de peligrosidad se identifican diferentes tramos como posibles ubicaciones para nuevos radares.

Cada candidato recibe una puntuación que combina diferentes características relevantes para la seguridad vial, entre ellas el **nivel de peligrosidad, el tipo de vía, la velocidad, la intensidad de tráfico, la conectividad de la red y la categoría del nodo**.

De esta forma, la selección no depende únicamente del número de accidentes de una zona, sino que considera conjuntamente diferentes factores relacionados con el riesgo y las características de la vía.

## 4️⃣ Optimización mediante algoritmo Greedy

Finalmente, se aplica un **algoritmo greedy de cobertura máxima ponderada** para seleccionar las ubicaciones de los nuevos radares.

En cada iteración se selecciona el candidato con mayor puntuación entre los disponibles. Una vez seleccionado, se aplica una restricción espacial para evitar que las nuevas ubicaciones queden excesivamente próximas entre sí o a los radares existentes.

En el proyecto se utiliza un **radio de exclusión de 500 metros** y el proceso continúa hasta obtener las **25 ubicaciones finales**.

El algoritmo greedy permite obtener una solución de forma eficiente para un problema de optimización espacial de gran tamaño. No obstante, al tratarse de un método heurístico, la solución obtenida no garantiza necesariamente el óptimo global.

## 5️⃣ Visualización de los resultados

Los resultados de las diferentes etapas se representan mediante **mapas interactivos desarrollados con Folium y Leaflet**. Estos permiten explorar la distribución de los puntos negros, consultar sus principales características y visualizar posteriormente las ubicaciones seleccionadas por el modelo de optimización.

De esta manera, el proyecto combina el análisis cuantitativo de los accidentes con una representación espacial que facilita la interpretación de los resultados.

```

---

## 1️⃣ Detección de puntos negros

Para estudiar la distribución espacial de los accidentes se utiliza una **rejilla de aproximadamente 90 × 90 metros**.

Los accidentes se agregan espacialmente para identificar zonas con una elevada concentración de siniestros.

Este enfoque se utiliza frente a DBSCAN debido al **efecto de chaining** observado en zonas con una elevada densidad de accidentes.

El análisis espacial permite identificar:

### **7.763 puntos negros**

clasificados en cuatro categorías:

| Categoría      | Nº de puntos |
| -------------- | -----------: |
| 🔴 **CRÍTICO** |          439 |
| 🟠 **ALTO**    |          798 |
| 🟡 **MEDIO**   |        2.813 |
| 🟢 **BAJO**    |        3.713 |

La clasificación considera principalmente la concentración y gravedad de los accidentes.

---

## 2️⃣ Índice de peligrosidad

Cada punto negro recibe un **índice de peligrosidad** que combina diferentes características de los accidentes.

Los pesos utilizados son:

```text
40 %  Accidentes
30 %  Accidentes graves
20 %  Accidentes mortales
10 %  Accidentes relacionados con alcohol
```

De esta manera, el modelo no considera únicamente cuántos accidentes se producen, sino también **su gravedad y determinadas circunstancias asociadas**.

---

# 🛣️ 3️⃣ Modelado de la red viaria

La red de carreteras de Madrid se representa mediante un **grafo** utilizando `OSMnx` y `NetworkX`.

* Los **nodos** representan puntos de la red viaria.
* Las **aristas** representan los tramos de carretera.

Los puntos negros se relacionan espacialmente con los nodos de la red.

Los nodos situados a menos de **50 metros** de un punto negro reciben la información correspondiente a dicho punto.

De esta forma, la información de peligrosidad obtenida a partir de los accidentes puede incorporarse directamente a la red viaria.

---

# 📡 4️⃣ Optimización de radares

Una vez construida la red viaria y asignada la información de riesgo, se evalúan diferentes tramos como posibles ubicaciones para nuevos radares.

Cada candidato recibe una puntuación basada en diferentes factores:

* Peligrosidad del punto negro.
* Tipo de vía.
* Velocidad.
* Intensidad de tráfico.
* Conectividad.
* Categoría del nodo.

A continuación se aplica un **algoritmo greedy de cobertura máxima ponderada**.

En cada iteración se selecciona el candidato con mayor puntuación y se aplica una restricción espacial para evitar que las ubicaciones seleccionadas queden demasiado próximas.

Se utiliza un radio de exclusión de:

### **500 metros**

El proceso continúa hasta seleccionar:

# 📍 25 ubicaciones

El algoritmo greedy proporciona una solución eficiente para un problema de optimización espacial de gran tamaño, aunque no garantiza necesariamente el óptimo global.

---

# 🗺️ Resultados interactivos

Una de las principales salidas del proyecto son los **mapas interactivos**, desarrollados con Folium y Leaflet.

### 🔴 Mapa de puntos negros

Permite explorar la distribución espacial de los puntos negros de Madrid.

Cada punto proporciona información sobre:

* Categoría.
* Número de accidentes.
* Accidentes graves.
* Accidentes mortales.
* Accidentes relacionados con alcohol.
* Sensores IMD cercanos.
* Índice de peligrosidad.
* Coordenadas.

👉 **[🗺️ Abrir mapa interactivo de puntos negros](maps/index.html)**

---

# 📓 Notebooks

El análisis completo está dividido en tres notebooks:

### 01 — Análisis de puntos negros

Procesamiento de los accidentes, análisis espacial, creación de la rejilla, cálculo del índice de peligrosidad y clasificación de los puntos negros.

👉 [`01_analisis_puntos_negros.ipynb`](notebooks/01_analisis_puntos_negros.ipynb)

### 02 — Modelado de la red viaria

Construcción y análisis del grafo de la red viaria de Madrid y asignación de los puntos negros a la red.

👉 [`02_modelado_red_viaria.ipynb`](notebooks/02_modelado_red_viaria.ipynb)

### 03 — Optimización de radares

Evaluación de candidatos y aplicación del algoritmo greedy para seleccionar las 25 ubicaciones propuestas.

👉 [`03_optimizacion_radares.ipynb`](notebooks/03_optimizacion_radares.ipynb)

---

# 📈 Principales resultados

| Resultado                                     |         Valor |
| --------------------------------------------- | ------------: |
| Accidentes históricos procesados              |   **317.844** |
| Periodo principal                             | **2019–2025** |
| Accidentes utilizados en el análisis espacial |   **140.872** |
| Puntos negros detectados                      |     **7.763** |
| Puntos críticos                               |       **439** |
| Ubicaciones de radares propuestas             |        **25** |
| Radio de exclusión                            |     **500 m** |

---

# 🧰 Tecnologías

### Lenguaje

* Python

### Análisis de datos

* Pandas
* NumPy
* Scikit-learn

### Análisis espacial

* GeoPandas
* Folium
* QGIS

### Redes y grafos

* NetworkX
* OSMnx

### Algoritmos

* BallTree
* Distancia Haversine
* Índices espaciales
* Algoritmo Greedy

### Herramientas

* Jupyter Notebook
* Visual Studio Code
* Git
* GitHub

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
│   └── index.html
│
├── data/
│   ├── README.md
│   └── processed/
│
└── docs/
    └── memoria.pdf
```

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

---

## 📚 Documentación

Para consultar la metodología completa y el desarrollo del proyecto:

👉 **[📄 Consultar memoria completa](docs/grupo_D_Antón_Lara.pdf)**
