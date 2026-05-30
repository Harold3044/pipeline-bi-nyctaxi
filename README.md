# Pipeline BI — NYC Yellow Taxi Trips
## Bases de Datos Avanzada 2026-I | Universidad Popular del Cesar

## Descripción general
Pipeline completo de Business Intelligence construido sobre Databricks, 
procesando más de 81 millones de registros del dataset NYC Yellow Taxi Trips 
(año 2019) siguiendo la Arquitectura Medallion (Silver → Gold) con 
visualización en Grafana Cloud.

---

## Tecnologías utilizadas

| Tecnología | Versión | Rol |
|---|---|---|
| Databricks | Trial 14 días | Plataforma central |
| Apache Spark (PySpark) | 3.x | Procesamiento distribuido |
| Delta Lake | Incluido en Databricks | Almacenamiento transaccional |
| Unity Catalog | Incluido en Databricks | Gobierno de datos |
| Grafana Cloud | Free tier | Visualización y dashboards |
| GitHub | — | Control de versiones |

---

## Estructura del repositorio

pipeline-bi-nyctaxi/
│
├── notebooks/
│   ├── 01_silver_transformacion.py   # Limpieza y filtrado de datos crudos
│   └── 02_gold_star_schema.py        # Star Schema y tablas agregadas
│
└── README.md

---

## Dataset

- **Nombre:** NYC Yellow Taxi Trips
- **Fuente:** `/databricks-datasets/nyctaxi/tripdata/yellow/`
- **Formato original:** CSV comprimido (.csv.gz)
- **Período:** Enero 2019 — Diciembre 2019
- **Registros procesados:** 81,749,077

---

## Arquitectura del pipeline
Fuente CSV.GZ (dataset Databricks)
│
▼
CAPA SILVER
proyecto_bi.silver.yellow_trips
[Limpieza con PySpark + Delta Lake]
81,749,077 registros
│
▼
CAPA GOLD
proyecto_bi.gold.*
[Star Schema + Tablas agregadas]
│
▼
SQL WAREHOUSE
Databricks
│
▼
GRAFANA CLOUD
3 Dashboards de BI

---

## Instrucciones de ejecución

### Requisitos previos
- Cuenta activa en Databricks (trial o paga)
- Cluster con Spark 3.x activo
- Unity Catalog habilitado

### Paso 1 — Crear catálogo y esquemas
Ejecutar en un notebook SQL:
```sql
CREATE CATALOG IF NOT EXISTS proyecto_bi;
CREATE SCHEMA IF NOT EXISTS proyecto_bi.silver;
CREATE SCHEMA IF NOT EXISTS proyecto_bi.gold;
```

### Paso 2 — Ejecutar capa Silver
Abrir y ejecutar el notebook:

notebooks/Proceso de Carga de Datos a Silver.ipynb

Tiempo estimado: 10-15 minutos

### Paso 3 — Ejecutar capa Gold
Abrir y ejecutar el notebook:

notebooks/Dimensiones.ipynb

notebooks/Tabla de Hechos.ipynb

Tiempo estimado: 5-10 minutos

### Paso 4 — Verificar resultados
```sql
SELECT COUNT(*) FROM proyecto_bi.silver.yellow_trips;  -- 81,749,077
SELECT COUNT(*) FROM proyecto_bi.gold.fact_viajes;     -- 81,749,077
SELECT COUNT(*) FROM proyecto_bi.gold.agg_viajes_por_dia;   -- ~365
SELECT COUNT(*) FROM proyecto_bi.gold.agg_viajes_por_hora;  -- 24
SELECT COUNT(*) FROM proyecto_bi.gold.agg_viajes_por_pago;  -- 5
SELECT COUNT(*) FROM proyecto_bi.gold.agg_viajes_por_zona;  -- 20
```

---

## Modelo dimensional (Star Schema)

dim_tiempo (9,082 registros)

dim_vendor (3)

fact_viajes (81.7M)

dim_pago (5)

### Tablas agregadas para dashboards

| Tabla | Registros | Uso |
|---|---|---|
| agg_viajes_por_dia | ~365 | Tendencia diaria |
| agg_viajes_por_hora | 24 | Horas pico |
| agg_viajes_por_pago | 5 | Métodos de pago |
| agg_viajes_por_zona | 20 | Top zonas |

---

## Dashboards en Grafana Cloud

- **Dashboard 1 — Ejecutivo:** KPIs generales, ingresos totales y tendencia mensual
- **Dashboard 2 — Operacional:** Demanda por hora, top zonas y métodos de pago
- **Dashboard 3 — Calidad de datos:** Anomalías, nulos y valores atípicos

---

## Autores
- Harold Solano
- Diego Caro

**Universidad Popular del Cesar — 2026**