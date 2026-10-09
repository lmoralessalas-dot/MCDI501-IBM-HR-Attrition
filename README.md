# MCDI501 · Rotación de empleados (IBM HR Employee Attrition)

Proyecto del curso **Estadística Computacional para la Toma de Decisiones (MCDI501)**, Magíster en Ciencia de Datos e Inteligencia Artificial, Universidad Andrés Bello.

**Equipo:** Hernán Saavedra González · Lenin Morales Salas · Rodrigo Gómez Indellicati
**Docente:** Jean Paul Maidana

## Pregunta de investigación

> ¿Se asocian las horas extra, el ingreso mensual, la satisfacción laboral y la antigüedad en la empresa con la salida de empleados?

La variable objetivo es `Attrition` (`Yes` = el empleado dejó la empresa). En esta etapa el análisis es descriptivo e inferencial; todavía no se entrenan modelos predictivos.

## Resultados principales (Formativa 1)

| Variable | Prueba | Resultado | Tamaño del efecto |
|---|---|---|---|
| Horas extra | χ² de independencia | 30,5 % de salida con horas extra vs. 10,4 % sin ellas, *p* < 0,001 | V = 0,25 · riesgo relativo 2,93 |
| Ingreso mensual | *t* de Welch | mediana 3.150,5 (salió) vs. 5.228 (permaneció), *p* < 0,001 | *d* = −0,47 |
| Antigüedad | *t* de Welch | mediana 3 vs. 6 años, *p* < 0,001 | *d* = −0,37 |
| Satisfacción laboral | χ² de independencia | salida de 22,8 % (nivel 1) a 11,3 % (nivel 4), *p* < 0,001 | V = 0,11 |

Proporción de salida: 16,12 % (IC 95 % de Wilson: 14,33 % – 18,09 %). Son asociaciones bivariadas en datos sintéticos: no prueban causalidad.

## Estructura

```
├── datos/
│   ├── raw/                          CSV original de Kaggle (no se modifica)
│   └── processed/                    generado por el cuaderno
│       ├── ibm_hr_mcar.csv           copia con 10 % de faltantes MCAR en ingreso y distancia
│       └── mcar_valores_reservados.csv   valores retirados (para evaluar imputaciones)
├── Formativa1/
│   ├── F1_IBM_HR_Analisis.ipynb      análisis completo, en el orden del informe
│   ├── figuras/                      figuras del informe (PNG)
│   └── resultados/                   tablas del informe (CSV) y diccionario de variables
├── docs/
│   ├── Informe_Formativa1_IBM_HR.pdf
│   └── Informe_Formativa1_IBM_HR.docx
├── requirements.txt
└── README.md
```

## Cómo ejecutar

```bash
git clone https://github.com/lmoralessalas-dot/MCDI501-IBM-HR-Attrition.git
cd MCDI501-IBM-HR-Attrition
python -m venv .venv
# Windows: .venv\Scripts\activate    ·    macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Abrir `Formativa1/F1_IBM_HR_Analisis.ipynb` y ejecutar **Kernel → Restart Kernel and Run All Cells**. El cuaderno detecta la raíz del repositorio, lee `datos/raw/`, fija la semilla 42 y regenera `datos/processed/`, `Formativa1/figuras/` y `Formativa1/resultados/`. Las verificaciones (`assert`) detienen la ejecución si el archivo de datos no coincide con el esperado.

## Datos

**IBM HR Analytics Employee Attrition & Performance**: 1.470 registros sintéticos y 35 columnas, sin faltantes ni duplicados. Fuente: [Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).

Decisiones de preparación:
- Se excluyen `EmployeeNumber` (identificador) y `EmployeeCount`, `Over18`, `StandardHours` (constantes).
- Se generan faltantes MCAR (10 %, semilla 42) en `MonthlyIncome` y `DistanceFromHome`, como pide el apunte de datasets del curso. No se imputan en esta etapa.
- `BusinessTravel` se trata como ordinal (Non-Travel < Travel_Rarely < Travel_Frequently).

## Flujo de trabajo

Cada cambio se hace en una rama y entra a `main` mediante *pull request* revisado por otro integrante:

```bash
git checkout -b feature/<tema>
git add <archivos> && git commit -m "Descripción breve del cambio"
git push -u origin feature/<tema>
```
