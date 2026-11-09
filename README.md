# Smart Retail Site Selection: Aprendizaje No Supervisado en Melbourne

## 🎯 Problema de Negocio
Elegir la ubicación de un nuevo local comercial basándose únicamente en el flujo anual de peatones suele llevar a entrar en calles saturadas de competidores o con tráfico en horarios inadecuados para el modelo de negocio. 

En este proyecto se analizan **456.829 registros horarios de sensores peatonales** y **19.730 establecimientos del censo comercial (CLUE)** de Melbourne para identificar océanos azules inmobiliarios y recomendar ubicaciones exactas entre los **3.011 locales vacíos (`Vacant Space`)** de la ciudad.

## 🛠️ Metodología y Pipeline Técnico
1. **Ingeniería de Variables Espaciales y Temporales:** Filtrado de calidad de sensores ($\ge 90\%$ de cobertura anual), agregación en 4 franjas horarias, ratio fin de semana vs. laborable y cruce geoespacial euclídeo en un radio de 200 m.
2. **Tendencia de Agrupamiento y Reducción Dimensional:** Validación de estructura no aleatoria mediante el **Estadístico de Hopkins ($H = 0,7490$)** y proyección con **PCA** (reteniendo el **71,0%** de la varianza en 2 componentes).
3. **Modelado de Clustering Comparado:**
   * **K-Means++ ($K=3$) y Jerárquico Aglomerativo (Ward):** Segmentación de la ciudad en 3 arquetipos (*Arterias Prime*, *Ocio/Gastronomía* y *Tránsito Laborable*).
   * **DBSCAN ($\varepsilon = 2,4$):** Detección de anomalías espaciales (`-1`) correspondientes a nodos con cifras récord de tráfico u oferta inmobiliaria.
4. **Evaluación de Robustez:** Comparación con criterios internos (*Silhouette* = 0,423 en DBSCAN) y simulación de estrés frente a ruido gaussiano con criterios externos (*ARI* = 0,897 en K-Means++).
5. **Decisión de Negocio, Sensibilidad y Validación Espacial:** Construcción de un *Score de Oportunidad Comercial*, validación de robustez mediante simulación Monte Carlo (**5.000 combinaciones aleatorias con distribución de Dirichlet**) y análisis de solapamiento de radios con `Folium` (identificando duplicidades espaciales entre sensores a <100 m).

## 📊 Principales Resultados
* Identificación de **2 zonas comerciales independientes** óptimas (*Spencer St-Collins St* y el eje *Collins St-Swanston St*) que maximizan la ratio de peatones por competidor directo.
* Extracción de direcciones postales exactas de locales vacíos a menos de 31 metros de los nodos ganadores.

## 💻 Tech Stack
* **Lenguaje:** Python 3
* **Librerías:** `scikit-learn`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `folium`

## 📂 Estructura del Repositorio
* `melbourne_retail_clustering.ipynb`: Cuaderno Jupyter con todo el código documentado, gráficas y tablas ejecutadas.
* `Informe_Tecnico_Melbourne.pdf`: Memoria técnica completa con la justificación matemática y de negocio.
* `mapa_top3_melbourne.html`: Mapa interactivo con los radios de influencia de 200 m y locales recomendados.
