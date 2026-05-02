# IVCU-MADRID
Índice de Vulnerabilidad Climática Urbana por barrios de Madrid
# IVCU Madrid 🌡️
## Índice de Vulnerabilidad Climática Urbana por barrios — Madrid 2024

[![Datos abiertos](https://img.shields.io/badge/Datos-datos.madrid.es-blue)](https://datos.madrid.es)
[![Python](https://img.shields.io/badge/Python-3.12-green)](https://www.python.org/)
[![Licencia](https://img.shields.io/badge/Licencia-MIT-yellow)](LICENSE)

Proyecto presentado a los **II Premios a la Reutilización de Datos Abiertos del Ayuntamiento de Madrid (2026)**, categoría *Servicios web, aplicaciones y visualizaciones — Premio Estudiante*.

**Autor:** Luis Javier Ramos Medina

---

## ¿Qué es el IVCU?

El **Índice de Vulnerabilidad Climática Urbana (IVCU)** mide el riesgo climático de los 131 barrios del municipio de Madrid combinando cinco dimensiones:

| Dimensión | Variable | Peso AHP |
|---|---|---|
| Exposición al calor | Densidad hab/ha × temperatura IDW | 25 % |
| Déficit de verde urbano | Árboles/ha en parques (invertido) | 25 % |
| Vulnerabilidad social | % población mayor de 65 años | 20 % |
| Privación económica | Renta neta anual (invertido) | 20 % |
| Contaminación del aire | NO₂ verano µg/m³ | 10 % |

Los pesos siguen el marco conceptual del **IPCC AR6 (2022)** para la evaluación de vulnerabilidad climática urbana, aplicando el método AHP (*Analytic Hierarchy Process*) como técnica de elicitación de preferencias.

---

## Resultados principales

El script genera los siguientes ficheros de salida, que se descargan automáticamente al navegador al finalizar la ejecución en Colab:

| Fichero | Descripción |
|---|---|
| `ivcu_madrid_interactivo.html` | ⭐ Mapa interactivo — abrir en navegador |
| `data/processed/ivcu_barrios.csv` | CSV con IVCU, perfiles IA y coordenadas IDW |
| `data/processed/ivcu_barrios.gpkg` | GeoPackage EPSG:25830 — compatible con ArcGIS Pro y QGIS |
| `data/processed/perfiles_clustering.csv` | Resumen K-Means por perfil de vulnerabilidad |
| `data/processed/pca_clustering.html` | Visualización PCA interactiva |

---

## Stack tecnológico

- **Python 3.12** — pandas, GeoPandas, NumPy, Folium, scikit-learn
- **Interpolación IDW** implementada desde cero con NumPy
- **K-Means + PCA** (scikit-learn) para perfiles de vulnerabilidad
- **Folium** para visualización interactiva (HTML autocontenido, sin servidor)
- **GeoPackage EPSG:25830** compatible con ArcGIS Pro y QGIS
- Ejecutable en **Google Colab** sin instalaciones locales

---

## ⚠️ Instrucciones de uso — LEER ANTES DE EJECUTAR

El portal **datos.madrid.es bloquea las descargas automáticas** desde servidores externos (devuelve HTTP 403). Por ello, **los 9 datasets deben descargarse manualmente desde el navegador** y subirse a Google Colab antes de ejecutar el script.

### Paso 1 — Descargar los 9 datasets

Descarga cada fichero haciendo clic en el enlace de descarga correspondiente:

| Fichero | Dataset | Enlace de descarga |
|---|---|---|
| `calidad_aire.csv` | Calidad del aire diario (NO₂, magnitud 8) | [Descargar](https://datos.madrid.es/dataset/201410-0-calidad-aire-diario/resource/201410-2-calidad-aire-diario-csv/download/201410-2-calidad-aire-diario-csv.csv) |
| `meteo.csv` | Meteorológicos diarios (temperatura, magnitud 83) | [Descargar](https://datos.madrid.es/dataset/300351-0-meteorologicos-diarios/resource/300351-1-meteorologicos-diarios-csv/download/300351-1-meteorologicos-diarios-csv.csv) |
| `padron.csv` | Padrón municipal histórico | [Descargar](https://datos.madrid.es/dataset/200076-0-padron/resource/200076-2-padron-csv/download/200076-2-padron-csv.csv) |
| `socioeconomico.csv` | Indicadores socioeconómicos por distrito | [Descargar](https://datos.madrid.es/dataset/300087-0-indicadores-distritos/resource/300087-0-indicadores-distritos-csv/download/300087-0-indicadores-distritos-csv.csv) |
| `arbolado.csv` | Arbolado en parques histórico | [Descargar](https://datos.madrid.es/dataset/300264-0-arbolado-parques-historico/resource/300264-104-arbolado-parques-historico/download/300264-104-arbolado-parques-historico.csv) |
| `vulnerabilidad.csv` | Ranking de vulnerabilidad de barrios | [Descargar](https://datos.madrid.es/dataset/300301-0-ranking-vulnerabilidad/resource/300301-0-ranking-vulnerabilidad-csv/download/300301-0-ranking-vulnerabilidad-csv.csv) |
| `estaciones_calidad_aire.csv` | Estaciones de control de calidad del aire | [Descargar](https://datos.madrid.es/dataset/212629-0-estaciones-control-aire/resource/212629-0-estaciones-control-aire-csv/download/212629-0-estaciones-control-aire-csv.csv) |
| `estaciones_meteo.csv` | Estaciones meteorológicas | [Descargar](https://datos.madrid.es/dataset/300360-0-meteorologicos-estaciones/resource/300360-1-meteorologicos-estaciones-csv/download/300360-1-meteorologicos-estaciones-csv.csv) |
| `barrios.zip` | Shapefile de barrios de Madrid | [Descargar](https://datos.madrid.es/dataset/300496-0-barrios-madrid/resource/300496-3-barrios-madrid/download/300496-3-barrios-madrid.zip) |

> Todos los datasets proceden exclusivamente del portal [datos.madrid.es](https://datos.madrid.es) y cubren el período 2019–2024.

### Paso 2 — Abrir el script en Google Colab

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/11YJRjnDuNJlVPons1AZ2b42XT2IlgTF8)

O bien sube el fichero `IVCU_IA_FINAL_v6.py` manualmente a Colab.

### Paso 3 — Subir los datasets a Colab

Al comienzo del script hay una función auxiliar para ello. Ejecuta esta celda **una sola vez** antes de `main()`:

```python
subir_datos_colab()
```

Se abrirá un diálogo de selección de ficheros. Selecciona los 9 ficheros descargados en el Paso 1 a la vez. El script los renombrará y colocará automáticamente en `data/raw/`.

### Paso 4 — Ejecutar

Una vez subidos los datos, ejecuta todo el script:

```
Runtime → Run all
```

Al finalizar, los ficheros de salida se descargarán automáticamente al navegador.

---

## Metodología

### Construcción del IVCU

Cada indicador se normaliza al rango 0–10 mediante *min-max scaling*. Los indicadores donde un valor mayor implica menor vulnerabilidad (árboles/ha, renta neta) se invierten antes de ponderar. El IVCU final es la suma ponderada de los cinco indicadores normalizados. Los 131 barrios se clasifican en cinco niveles por quintiles de la distribución observada:

| Nivel | Quintil |
|---|---|
| 🔵 Muy bajo | ≤ P20 |
| 🔷 Bajo | P20 – P40 |
| 🟡 Moderado | P40 – P60 |
| 🟠 Alto | P60 – P80 |
| 🔴 Crítico | > P80 |

### Interpolación espacial IDW

Los datos de calidad del aire y temperatura proceden de estaciones de medición puntuales distribuidas irregularmente por el municipio. Para asignar un valor representativo a cada barrio se aplica interpolación por **Distancia Inversa Ponderada (IDW, potencia p=2)** desde los centroides de las estaciones hacia los centroides geométricos de cada barrio, calculados sobre el shapefile oficial en proyección ETRS89. Este enfoque es metodológicamente más riguroso que usar una media global para todos los barrios.

### Clustering IA — Perfiles de vulnerabilidad (K-Means + PCA)

Sobre los cinco indicadores normalizados y estandarizados (Z-score) se entrena un modelo **K-Means con k=5**, seleccionado por su idoneidad interpretativa. Los resultados se proyectan en dos dimensiones mediante **PCA**, que explica conjuntamente más del 60 % de la varianza. Los cinco perfiles resultantes son:

| Perfil | Barrios | IVCU medio | Característica diferenciadora |
|---|---|---|---|
| 🔴 Alta privación económica y contaminación | 26 | 6,42 | Renta baja + NO₂ elevado |
| 🟠 Envejecimiento y déficit de verde | 45 | 6,18 | Alto % mayores 65 + escaso arbolado |
| 🟣 Renta baja y escasez de verde | 28 | 5,19 | Privación moderada + déficit extremo de verde |
| 🟢 Vulnerabilidad contenida | 29 | 4,58 | Indicadores por debajo de la media |
| 🔵 Verde abundante | 3 | 4,06 | Zonas forestales (El Pardo, Casa de Campo) |

> **Nota metodológica:** los índices de silueta son moderados (~0,22) para todos los valores de k (testados de k=2 a k=6), lo que indica que Madrid presenta una vulnerabilidad climática relativamente homogénea entre barrios. Este hallazgo justifica la pertinencia de intervenciones a escala fina de barrio frente a enfoques distritales.

---

## Licencia

Este proyecto se distribuye bajo licencia **MIT**. Los datos utilizados proceden del portal [datos.madrid.es](https://datos.madrid.es) y están sujetos a la licencia de reutilización del Ayuntamiento de Madrid.
