# Movilidad urbana y productividad económica en ciudades de Latinoamérica

Análisis exploratorio en Python que relaciona la **congestión vehicular** (TomTom Traffic Index) con indicadores **económicos** (OECD Cities) de las principales ciudades latinoamericanas en 2024, con el fin de identificar en qué ciudades conviene priorizar la inversión en infraestructura de transporte.

## Contenido del repositorio

| Archivo | Descripción |
|---|---|
| `Tom_Tom_Traffic_Movility.ipynb` | Notebook con todo el análisis (limpieza, unión, visualización y conclusiones) |
| `ladb_mobility_economy_2024_clean.csv` | Dataset final (resultado del notebook), una fila por ciudad |

## Datos de entrada

- **`tomtom_traffic.csv`**: mediciones de tráfico de TomTom (~1 millón de registros). Incluye país, ciudad, fecha/hora UTC, retraso por congestión (`JamsDelay`), índice de tráfico, longitud y número de congestiones, y tiempos de viaje por cada 10 km.
- **`oecd_city_economy.csv`**: indicadores económicos por ciudad y año (2023 y 2024): PIB per cápita, desempleo, PM2.5 y población.

> Los archivos se leen desde `/datasets/`. Ajusta la ruta si ejecutas el notebook en otro entorno.

## Metodología

1. **Carga y exploración** de ambos datasets.
2. **Limpieza y preparación**
   - Renombrado de columnas a formato `snake_case`.
   - Conversión de fechas a `datetime` (UTC).
   - Conversión de columnas numéricas de `eco` que venían como texto (separadores de miles/decimales, símbolo `%`) y cálculo de la población en unidades absolutas.
3. **Extracción del año y filtrado a 2024.**
4. **Agregación del tráfico** por ciudad, país y año (promedio de las métricas de congestión).
5. **Unión (`inner join`)** de tráfico y economía por `city` y `year`.
6. **Visualización**: boxplot de `jams_delay`, histograma de PIB per cápita y gráfico de barras comparando congestión y PIB por ciudad.
7. **Exportación** del dataset final a CSV.

## Variables del dataset final

`city`, `country`, `year`, `jams_delay`, `traffic_index_live`, `jams_lenght_in_km`, `jams_count`, `mins_delay`, `travel_time_live_per_10kms_mins`, `travel_time_historic_per_10kms_mins`, `city_gdp_capita`, `unemployment_pct`, `pm2.5_(μg/m³)`, `population`.

## Hallazgos principales

- Ciudad de México es la ciudad con mayor retraso promedio por congestión (`jams_delay`) en el conjunto de datos de tráfico de 2024.
- Se observa una relación entre actividad económica (PIB per cápita) y congestión: la congestión parece ser un síntoma del dinamismo económico y de una mayor demanda de movilidad.
- Existen ciudades con PIB alto y poca congestión, que podrían explicarse por mayor uso de transporte público, trabajo remoto o menor parque vehicular (hipótesis por validar).

## Recomendaciones

- Priorizar inversión en **Ciudad de México, Bogotá, Lima, Santiago y São Paulo**.
- Enfocarla en renovar y ampliar el **transporte público**, no solo en nuevas vialidades.
- Fomentar **trabajo híbrido o remoto** donde sea viable.
- Estudiar el volumen de **viajes pendulares** desde zonas metropolitanas y periféricas hacia los centros urbanos (relevante en CDMX) para evaluar la descentralización de la actividad económica.

## Limitaciones

- La relación PIB–tráfico se interpreta de forma visual; **no se calculó un coeficiente de correlación** ni se probó causalidad.
- Muestra reducida (15 ciudades, un solo año), por lo que las conclusiones son exploratorias.
- Las medias de tráfico se calculan sobre todos los registros de 2024 sin distinguir horas pico, días laborales ni estacionalidad.

## Requisitos

- Python 3.9+
- `pandas`, `matplotlib`, `seaborn`, `jupyter`

```bash
pip install pandas matplotlib seaborn jupyter
jupyter notebook Tom_Tom_Traffic_Movility.ipynb
```

## Fuentes

- [TomTom Traffic Index](https://www.tomtom.com/traffic-index/)
- [OECD Cities](https://www.oecd.org/cfe/regionaldevelopment/)
