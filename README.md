# ml-no-supervisado-inmuebles
Segmentación de 1 000 inmuebles en arriendo con aprendizaje no supervisado: K-Means (k=2,3,4), PCA, SVD y clustering jerárquico en Google Colab. CADI Introducción a Machine Learning, Universidad de Cundinamarca.

# Segmentación de inmuebles en arriendo con aprendizaje no supervisado

**K-Means · PCA · SVD · Clustering jerárquico**

Universidad de Cundinamarca — Especialización en Inteligencia Artificial
CADI: Introducción a Machine Learning · Período 2026-2
**Estudiante:** Ruby Dayana Cárdenas Gómez
**Docente:** Ing. Wilson Rojas Reales, PhD(c)

Una inmobiliaria con 1 000 inmuebles en arriendo no tiene una clasificación previa de su portafolio. Este proyecto busca grupos naturales con algoritmos no supervisados y recomienda qué enfoque usar. El informe completo (APA 7) está en [`Informe_ML_No_Supervisado_Inmuebles.pdf`](Informe_ML_No_Supervisado_Inmuebles.pdf).

## Notebooks (Google Colab)

| # | Notebook | Contenido | Abrir |
|---|---|---|---|
| 1 | `01 KMeansSegmentacionInmuebles.ipynb` | Auditoría del dataset, script del docente, K-Means con k = 2, 3 y 4, perfil comercial, 11 preguntas | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/RubyDayana/ml-no-supervisado-inmuebles/blob/main/01%20KMeansSegmentacionInmuebles.ipynb) |
| 2 | `02 PCAyKMeansInmuebles.ipynb` | Preprocesamiento, PCA, varianza explicada, cargas, información perdida, K-Means sobre componentes, 7 preguntas | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/RubyDayana/ml-no-supervisado-inmuebles/blob/main/02%20PCAyKMeansInmuebles.ipynb) |
| 3 | `03 SVDyClusteringJerarquico.ipynb` | PCA como SVD, `TruncatedSVD`, aproximación de bajo rango, compresión de imagen, enlaces y dendrograma | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/RubyDayana/ml-no-supervisado-inmuebles/blob/main/03%20SVDyClusteringJerarquico.ipynb) |

Los notebooks traen sus salidas y gráficas ya ejecutadas. Si el archivo de datos no está en `datos/` (por ejemplo, en Colab), lo descargan solos del repositorio.

## Resultados principales

**K-Means con k = 3** (área, habitaciones, estrato y precio estandarizados):

| Segmento | Inmuebles | Área | Arriendo promedio | Recaudo |
|---|---|---|---|---|
| Económico | 419 (41.9 %) | 49.7 m² | 1.47 M COP | 16.9 % |
| Intermedio | 209 (20.9 %) | 85.4 m² | 3.13 M COP | 18.0 % |
| Premium | 372 (37.2 %) | 156.1 m² | 6.35 M COP | 65.0 % |

**Elección de k:** las métricas internas favorecen k = 2 (silueta 0.611), pero k = 3 (silueta 0.482) es el más útil comercialmente; k = 4 (0.427) solo divide el premium en compacto y amplio.

**PCA:** con las 8 columnas preprocesadas, dos componentes explican 62.9 % de la varianza (45.1 % + 17.8 %); se necesitan 3 para el 80 % y 5 para el 90 %. Con las 4 variables informativas, PC1 explica 85.1 %.

**Hallazgo clave:** agrupar con todas las columnas, o con solo dos componentes, separa inmuebles equivalentes por número de baños (una variable sin relación con el precio). Se recomienda K-Means sobre las variables informativas, con PCA como diagnóstico.

**SVD y jerárquico:** PCA coincide con la SVD de los datos centrados (diferencias de ~1e-13); Ward con k = 3 coincide con K-Means (ARI = 0.975).

## Calidad de los datos

| Hallazgo | Decisión |
|---|---|
| La columna `Estrato` aparece dos veces; la segunda (1–4) no se relaciona con nada (correlación 0.025 con el precio) | Se descarta la segunda y se usa la primera (por confirmar con el docente) |
| Encabezado `Antig[uedad` | Se renombran las columnas |
| `Localidad` y `Tipo Inmueble` son equivalentes | Se usa solo `Localidad` |
| El identificador `Inmueble` | Se conserva pero no se usa como variable |

## Estructura

```
ml-no-supervisado-inmuebles/
├── 01 KMeansSegmentacionInmuebles.ipynb
├── 02 PCAyKMeansInmuebles.ipynb
├── 03 SVDyClusteringJerarquico.ipynb
├── Informe_ML_No_Supervisado_Inmuebles.pdf
├── README.md
└── datos/
    └── DATASET_Inmuebles.xlsx      # dataset entregado por el docente
```

## Cómo ejecutarlo

En Colab: abrir un notebook con el botón de arriba y elegir *Entorno de ejecución → Ejecutar todo*.

En local:

```bash
git clone https://github.com/RubyDayana/ml-no-supervisado-inmuebles.git
cd ml-no-supervisado-inmuebles
pip install numpy pandas matplotlib scikit-learn scipy openpyxl jupyter
jupyter notebook
```

Todos los modelos usan `random_state=42` y `n_init=10`, por lo que los resultados son reproducibles.
