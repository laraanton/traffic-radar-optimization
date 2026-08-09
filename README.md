# 🚦 Optimización de la ubicación de radares para la prevención de accidentes de tráfico en Madrid

## 📌 Descripción del proyecto

Este proyecto aborda el problema de la **localización óptima de radares de velocidad en Madrid** con el objetivo de identificar y priorizar las zonas con mayor concentración y gravedad de accidentes de tráfico.

Para ello, se combinan técnicas de **análisis de datos, análisis espacial, modelado de redes y optimización**. A partir de los datos históricos de accidentes se identifican puntos negros, se evalúa su peligrosidad y posteriormente se utiliza la red viaria de Madrid para determinar qué tramos podrían ser candidatos adecuados para la instalación de nuevos radares.

El objetivo final es seleccionar un conjunto limitado de ubicaciones que permita **maximizar la cobertura de las zonas de mayor riesgo**, evitando al mismo tiempo colocar radares excesivamente próximos entre sí o a los ya existentes.

---

## 🎯 Objetivos

Los principales objetivos del proyecto son:

* Analizar los accidentes de tráfico registrados en Madrid.
* Estudiar su distribución espacial.
* Detectar y clasificar **puntos negros** según su nivel de peligrosidad.
* Construir una representación de la red viaria de Madrid mediante grafos.
* Asociar la información de peligrosidad de los accidentes a la red de carreteras.
* Incorporar los radares existentes como restricciones espaciales.
* Evaluar los posibles tramos candidatos mediante diferentes factores de riesgo.
* Seleccionar **25 ubicaciones óptimas para nuevos radares** mediante un algoritmo greedy.
* Representar los resultados mediante mapas interactivos.

---

## 📊 Datos utilizados

El proyecto utiliza diferentes fuentes de datos relacionadas con la movilidad y la seguridad vial de Madrid.

### Accidentes de tráfico

El conjunto de datos histórico principal utilizado en el proyecto contiene información de accidentes registrados entre:

**2019 – 2025**

El fichero procesado contiene **317.844 registros** y recoge información relacionada con:

* Fecha y hora del accidente.
* Localización.
* Distrito.
* Tipo de accidente.
* Condiciones meteorológicas.
* Tipo de vehículo.
* Tipo de persona implicada.
* Rango de edad.
* Sexo.
* Gravedad de las lesiones.
* Alcohol.
* Drogas.
* Coordenadas geográficas.

Además, se dispone de un conjunto de datos separado correspondiente a accidentes de **2026**, utilizado como fuente adicional durante el desarrollo.

### Otros datos

También se utilizan datos relacionados con:

* 📡 Radares fijos y móviles.
* 🚗 Intensidad de tráfico.
* 🚦 Cruces semaforizados.
* 🛣️ Red de carreteras de Madrid.

Las principales fuentes de datos son el **Portal de Datos Abiertos del Ayuntamiento de Madrid**, OpenStreetMap y Geofabrik.

Los datasets originales de gran tamaño no se incluyen íntegramente en el repositorio. En `data/README.md` se documentan sus fuentes y cómo obtenerlos.

---

# 🗺️ Metodología

El proyecto se estructura en varias etapas:

```text
Datos de accidentes
        │
        ▼
Limpieza y procesamiento
        │
        ▼
Discretización espacial
        │
        ▼
Detección de puntos negros
        │
        ▼
Clasificación de peligrosidad
        │
        ▼
Construcción de la red viaria
        │
        ▼
Modelado mediante grafos
        │
        ▼
Asignación de riesgo a la red
        │
        ▼
Evaluación de tramos candidatos
        │
        ▼
Optimización mediante algoritmo Greedy
        │
        ▼
25 ubicaciones seleccionadas
```

---

## 📍 1. Discretización espacial

Para analizar la concentración espacial de los accidentes se utiliza una **rejilla regular de aproximadamente 90 × 90 metros**.

Los accidentes se agrupan dentro de estas celdas para detectar zonas con una elevada concentración de siniestros.

Este enfoque se utilizó frente a métodos de clustering como DBSCAN debido al **efecto de chaining**, que podía generar agrupaciones excesivamente conectadas en zonas con una elevada densidad de accidentes.

A partir del procesamiento espacial se identificaron:

* **7.763 puntos negros**
* **140.872 accidentes únicos**

Los puntos negros se clasificaron en cuatro categorías:

| Categoría  | Nº de puntos |
| ---------- | -----------: |
| 🔴 CRÍTICO |          439 |
| 🟠 ALTO    |          798 |
| 🟡 MEDIO   |        2.813 |
| 🟢 BAJO    |        3.713 |

### Criterios de clasificación

* 🔴 **Crítico:** ≥ 50 accidentes o ≥ 1 accidente mortal.
* 🟠 **Alto:** 25–49 accidentes.
* 🟡 **Medio:** 10–24 accidentes.
* 🟢 **Bajo:** 5–9 accidentes.

---

## ⚠️ 2. Índice de peligrosidad

Para cada punto negro se calcula un índice compuesto de peligrosidad basado en diferentes características de los accidentes.

El índice utiliza los siguientes pesos:

```text
Índice de peligrosidad =
    0,4 × accidentes
  + 0,3 × accidentes graves
  + 0,2 × accidentes mortales
  + 0,1 × accidentes relacionados con alcohol
```

De esta forma, el modelo no considera únicamente el número de accidentes, sino también su **gravedad y determinadas circunstancias asociadas**.

---

## 🛣️ 3. Modelado de la red viaria

La red de carreteras de Madrid se representa mediante un **grafo** utilizando NetworkX y OSMnx.

En este grafo:

* Los **nodos** representan intersecciones o puntos de la red.
* Las **aristas** representan los tramos de carretera que conectan dichos nodos.

Posteriormente, la información de los puntos negros se relaciona espacialmente con la red viaria.

Los nodos situados a una distancia inferior a **50 metros** de un punto negro reciben su categoría de peligrosidad. Cuando existen varios puntos negros cercanos, se asigna la categoría correspondiente al punto de mayor peligrosidad.

---

## 📡 4. Optimización de la ubicación de radares

Una vez identificadas las zonas de mayor riesgo, se generan candidatos sobre la red viaria.

Cada candidato recibe una puntuación teniendo en cuenta diferentes factores:

* Nivel de peligrosidad.
* Tipo de vía.
* Velocidad.
* Intensidad de tráfico.
* Conectividad.
* Categoría del nodo.

Posteriormente se aplica un **algoritmo greedy** para seleccionar progresivamente las ubicaciones con mayor puntuación.

Para evitar que las ubicaciones seleccionadas queden excesivamente concentradas, se aplica una restricción espacial de **500 metros** respecto a otras ubicaciones seleccionadas y a los radares existentes.

El resultado final son:

### 📍 25 ubicaciones candidatas para nuevos radares

El algoritmo greedy proporciona una solución eficiente para el problema, aunque no garantiza necesariamente el óptimo global.

---

# 🗺️ Mapas interactivos

Uno de los principales resultados del proyecto son los mapas interactivos desarrollados con **Folium y Leaflet**.

## 🔴 Puntos negros de Madrid

Este mapa permite explorar espacialmente los puntos negros detectados.

Cada punto contiene información sobre:

* Categoría.
* Número de accidentes.
* Accidentes graves.
* Accidentes mortales.
* Accidentes relacionados con alcohol.
* Número de sensores IMD en un radio de 500 m.
* Índice de peligrosidad.
* Coordenadas.

El tamaño de los círculos es proporcional al índice de peligrosidad.

👉 **[Abrir mapa interactivo de puntos negros](maps/black_spots.html)**

El mapa utiliza las categorías:

```text
🔴 CRÍTICO
🟠 ALTO
🟡 MEDIO
🟢 BAJO
```

---

## 📡 Ubicaciones optimizadas de radares

Otro de los resultados principales es el mapa con las ubicaciones seleccionadas por el algoritmo de optimización.

👉 **[Abrir mapa de ubicaciones de radares](maps/radar_locations.html)**

Este mapa permite comparar las ubicaciones propuestas con la distribución de los radares existentes y con las zonas de mayor riesgo.

---

# 📈 Resultados principales

El proyecto permite obtener una visión espacial de la distribución de los accidentes de tráfico y utilizarla posteriormente para apoyar la toma de decisiones sobre la ubicación de radares.

Entre los principales resultados se encuentran:

* **317.844 registros** en el dataset histórico procesado.
* **140.872 accidentes únicos** utilizados en el análisis espacial.
* **7.763 puntos negros** identificados.
* **439 puntos negros críticos**.
* Clasificación de los puntos negros en cuatro niveles de peligrosidad.
* Construcción de la red viaria de Madrid como un grafo.
* Integración de información sobre tráfico y radares existentes.
* Selección de **25 ubicaciones candidatas para nuevos radares**.
* Desarrollo de mapas interactivos para explorar los resultados.

---

# 🧰 Tecnologías utilizadas

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

### Grafos y redes

* NetworkX
* OSMnx

### Algoritmos espaciales

* BallTree
* Distancia Haversine
* Índices espaciales

### Visualización

* Matplotlib
* Folium
* Leaflet

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
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   ├── 01_data_preparation.ipynb
│   ├── 02_black_spots_analysis.ipynb
│   └── 03_radar_optimization.ipynb
│
├── src/
│   ├── data_processing.py
│   ├── spatial_analysis.py
│   ├── graph.py
│   └── radar_optimization.py
│
├── data/
│   ├── README.md
│   └── processed/
│       └── puntos_negros_nodos.csv
│
├── maps/
│   ├── black_spots.html
│   └── radar_locations.html
│
├── results/
│   ├── figures/
│   └── radar_locations.csv
│
└── docs/
    └── project_report.pdf
```

---

# 🔎 Limitaciones y posibles mejoras

El proyecto puede ampliarse en diferentes direcciones:

* Incorporar medidas de tráfico más precisas.
* Realizar un análisis de sensibilidad de los pesos utilizados en el índice de peligrosidad.
* Analizar diferentes distancias mínimas entre radares.
* Incorporar nuevas variables relacionadas con las condiciones de la vía.
* Incorporar información meteorológica más detallada.
* Utilizar medidas de exposición al tráfico.
* Comparar el algoritmo greedy con otros métodos de optimización.
* Incorporar métricas de centralidad de grafos.
* Actualizar periódicamente el modelo con nuevos datos de accidentes.

---

# 🤖 Uso de Inteligencia Artificial

Durante el desarrollo del proyecto se utilizaron herramientas de Inteligencia Artificial como apoyo complementario para:

* Revisión y mejora de la redacción.
* Configuración de documentos.
* Generación y apoyo en determinadas partes del código.
* Desarrollo de visualizaciones.
* Exploración bibliográfica.
* Revisión de consistencia del código.

El código generado o asistido mediante estas herramientas fue posteriormente integrado y validado dentro del proyecto.

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

## 📄 Documentación

La documentación completa del proyecto, incluyendo la metodología, formulación del problema, algoritmos y resultados, se encuentra disponible en:

👉 **[Memoria del proyecto](docs/project_report.pdf)**

---

## ⚖️ Licencia y datos

Este proyecto ha sido desarrollado con fines académicos.

Los datos utilizados proceden de fuentes públicas. Antes de redistribuir los datasets originales, deben consultarse sus correspondientes condiciones de uso y licencia.
