
## Estructura del proyecto

```
├── config.py              Configuración: IDs de series, rutas, mapeos.
│                          El código dice CÓMO; esto dice CON QUÉ.
├── src/
│   ├── extract.py         [RESUELTO]  Descarga de la API -> data/raw/
│   ├── transform.py       [TU TRABAJO] TODOs 1 a 8
│   ├── load.py            [PARCIAL]    TODOs 9 a 12
│   └── main.py            [RESUELTO]  Orquesta E -> T -> L
├── tests/
│   └── test_transform.py  17 tests que definen qué se espera de vos
│                          (+ 2 en blanco para que escribas vos)
├── data/
│   ├── raw/               Datos crudos (no se versionan)
│   └── processed/         Salidas finales (no se versionan)
└── logs/                  Historial de corridas
```



