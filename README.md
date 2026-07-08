# Documentación del Proyecto: Modelos de Machine Learning (Cáncer de mama)

## Descripción
El presente proyecto tiene como objeto de estudio el análisis de un conjunto de datos médicos sobre el cáncer de mama mediante técnicas de aprendizaje no supervisado. El propósito principal es identificar patrones y grupos con características similares para lograr caracterizar a la población que presenta tumores benignos (B) y malignos (M), sin depender de la variable objetivo durante el entrenamiento del modelo.

## Tecnologías y Librerías Utilizadas
* **Python 3**: Lenguaje base para la ejecución del cuaderno.
* **Pandas y NumPy**: Utilizados para la ingesta, manipulación estructural y análisis descriptivo de los datos.
* **Matplotlib (`pyplot`), Seaborn y Plotly Express**: Implementados para la visualización de datos, incluyendo la gráfica de la "regla del codo", diagramas de dispersión de componentes principales e histogramas interactivos.
* **Scikit-Learn**: Empleado para la implementación del algoritmo de agrupamiento (`KMeans`) y para la reducción de dimensionalidad matemática (`PCA`).

## Fuente de Datos
Los datos se cargan desde el archivo local `breast-cancer.csv`. El conjunto de datos consta de 569 pacientes (entradas) y 32 columnas que contienen información clínica y medidas de los tumores (como `radius_mean`, `texture_mean`, `perimeter_mean`, entre otras). A través de la exploración inicial con `.info()`, se confirmó que no existen datos nulos en el dataset, por lo que no fue necesario aplicar técnicas de imputación o rellenado de valores. Originalmente, la variable objetivo diagnóstica indica que el resultado más común en esta muestra es el tumor benigno con 357 incidencias.

## Estructura del Análisis

### 1. Exploración Inicial y Eliminación de Datos Irrelevantes
* Se realizó una limpieza inicial eliminando la columna `id` para reducir el ruido en los datos.
* Se eliminó la variable objetivo `diagnosis` para evitar sesgar al modelo, cumpliendo con el rigor del aprendizaje no supervisado. Tras este paso, el dataset quedó conformado por 30 variables de tipo decimal continuo (`float64`).

### 2. Determinación del Número Óptimo de Clústeres
* Se iteró el algoritmo K-Means en un rango de 1 a 9 clústeres para calcular la inercia del modelo en cada punto.
* Los resultados se graficaron utilizando la "Regla del codo", lo cual permitió definir visualmente y de forma objetiva que el número óptimo de agrupaciones a utilizar era $k=2$.

### 3. Implementación de K-Means y Reducción de Dimensionalidad (PCA)
* Se entrenó el modelo K-Means con $k=2$ y se extrajeron las predicciones hacia una nueva columna llamada `group`.
* Para poder visualizar las 30 dimensiones clínicas en un plano 2D, se aplicó el Análisis de Componentes Principales (PCA), reduciendo los datos a dos variables principales (`PCA1` y `PCA2`).
* El gráfico de dispersión resultante evidenció que el "Grupo 0" posee datos sumamente concentrados y con alta correlación entre sí, mientras que el "Grupo 1" exhibe una mayor dispersión.

### 4. Distribución de las Predicciones y Matriz de Correlación
* Se generó un histograma con Plotly Express para observar la distribución final dictada por el algoritmo: el Grupo 0 agrupó a 438 pacientes, mientras que el Grupo 1 agrupó a 131 pacientes.
* Finalmente, se descartaron las columnas generadas por el modelo (`group`, `PCA1`, `PCA2`) para graficar un mapa de calor (Heatmap) limpio que expone la matriz de correlación directa entre las 30 variables clínicas originales.

## Conclusiones Principales
1. **Agrupación Natural:** El algoritmo no supervisado fue capaz de separar con éxito a la población en dos grandes clústeres basándose únicamente en las medidas paramétricas del tejido, sin conocer el diagnóstico previo del paciente.
2. **Caracterización de los Clústeres:** La distribución reveló que el **Grupo 0**, caracterizado por su alta concentración de datos y agrupando a la inmensa mayoría de la población (438 registros), corresponde al diagnóstico de tumor benigno. En contraste, el **Grupo 1**, con mayor dispersión de variables y menor volumen de pacientes (131 registros), concentra los casos de tumores malignos.
