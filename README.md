# Pipeline ETL — Exportaciones del NEA

Pipeline ETL que descarga datos de exportaciones por provincia y país de destino desde la API de Series de Tiempo de datos.gob.ar (INDEC), los transforma en un dataset analítico y los guarda en disco con controles de calidad y trazabilidad.

## ¿Qué hace el pipeline?

El pipeline tiene tres etapas desacopladas en módulos independientes:

1. **Extract** (`src/extract.py`): Descarga los datos crudos desde la API pública y los almacena en `data/raw/`.
2. **Transform** (`src/transform.py`): Limpia, normaliza tipos y deriva variables. Convierte el esquema de formato ancho a formato largo (tidy), clasifica regiones geoeconómicas, décadas, calcula participaciones porcentuales, variaciones interanuales, rankings anuales y realiza el join con los rubros productivos.
3. **Load** (`src/load.py`): Ejecuta validaciones de calidad de datos (*quality checks*), genera el CSV final, la ficha técnica en JSON y asienta el historial de ejecución en el log.

El orquestador (`src/main.py`) coordina la ejecución secuencial de punta a punta.

---

## ¿Cómo instalarlo y ejecutarlo?

### 1. Clonar el repositorio
``` bash
git clone https://github.com/wilson-j-insaurralde/tp-final-etl-nea-wilson-insaurralde.git
cd tp-final-etl 
```


### 2. Crear y activar el entorno virtual

# En Windows:
``` bash
python -m venv .venv
.venv\Scripts\activate
```

# En macOS/Linux:
``` bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Instalar dependencias
``` bash
pip install -r requirements.txt
```

### 4. Ejecutar el pipeline
``` bash
python src/main.py
```

### 5. Correr la suite de tests
``` bash
python tests/test_transform.py
```

## ¿De dónde salen los datos?

Los datos se obtienen de la API de Series de Tiempo del portal nacional de datos abiertos (datos.gob.ar), provistos por el INDEC:

Dataset 357.1: Exportaciones por provincia y país de destino.

Dataset 350.1: Exportaciones por provincia y rubro.

Provincias: Chaco, Corrientes, Formosa y Misiones.

Período: 1993 – 2024 (32 años).

Unidad: Millones de dólares FOB.

No requiere credenciales ni registro previo: es una API pública y abierta.

## Salidas del pipeline

data/processed/exportaciones_nea.csv: Dataset analítico final con 13 columnas y 1.408 filas cumpliendo el contrato de datos.

data/processed/resumen.json: Ficha técnica de la corrida (metadatos, estadísticas descriptivas básicas del valor FOB y resultado de los controles de calidad).

logs/pipeline.log: Registro append-only que asienta cada ejecución exitosa del pipeline.

## Un hallazgo en los datos

Al explorar el dataset generado, se destaca el cambio estructural en la relevancia de China como socio comercial de la región.

En 1993, China representaba un destino marginal para el NEA: en Chaco apenas alcanzaba los 0.31 millones de dólares (0.2% del total provincial). Hacia 2010 se posicionó como el principal destino de la provincia con 129.58 millones, y en 2024 cerró en 110.93 millones consolidados, concentrados fuertemente en productos primarios.

La misma tendencia se replica en el resto de la región: Misiones pasó de registrar 0.0 millones en 1993 a 53.26 millones en 2024, mientras que Corrientes ascendió de 0.0 a 27.43 millones en el mismo período.

Asimismo, resalta un registro atípico en Corrientes durante 2020 con destino Brasil, donde se alcanzaron 399.14 millones de dólares frente a los 25.29 millones del año previo (un salto del 1478%), constituyendo el valor máximo histórico de todo el dataset regional.

## Estructura del proyecto
```
tp-final-etl/
├── data/
│   ├── raw/                 # Datos crudos obtenidos de la API
│   └── processed/           # Dataset analítico (CSV) y ficha técnica (JSON)
├── logs/                    # Historial de ejecuciones del pipeline (pipeline.log)
├── src/
│   ├── config.py            # Rutas y constantes de configuración
│   ├── extract.py           # Ingesta desde API pública
│   ├── transform.py         # Limpieza, transformaciones y cruce de datos
│   ├── load.py              # Quality checks y persistencia
│   └── main.py              # Orquestador del flujo
├── tests/
│   └── test_transform.py     # Tests unitarios de transformaciones
├── requirements.txt
└── README.md
```
Autor
Wilson J. Insaurralde