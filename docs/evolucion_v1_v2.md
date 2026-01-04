# Propósito
Este documento describe los cambios técnicos introducidos entre las versiones v1 y v2 del sistema.

Su objetivo es resumir, de forma comparativa, las diferencias en dataset, scoring heurístico, conjunto de features y resultados.

## Limitaciones detectadas en v1

Durante el desarrollo de la v1 se identificaron varias limitaciones que impedían
utilizarla como baseline estable:

- Dataset pequeño y de carácter exploratorio.
- Scoring heurístico sin rol claramente definido en el pipeline.
- Conjunto de features no contractual, con riesgo de dependencia del dataset.
- Evaluación suficiente para aprendizaje, pero no para generalización.

Estas limitaciones motivaron el rediseño introducido en la v2.


## Dataset

| Campo | V1 | V2 |
|---|---:|---:|
| Nº total de URLs | 200 | 482 |
| Balance de clases | 100 / 100 | 241 / 241 |
| Curación | Manual + heurísticas básicas | Scoring v2 + curación manual |
| Sectores dominantes | banca, SaaS | banca, logística |

Nota: el sesgo hacia banca y logística se acepta de forma consciente para priorizar realismo del ataque frente a cobertura artificial de sectores.

**Fuentes de datos:**

- V1: feeds OSINT públicos, curación manual exploratorio y primer conjunto de reglas de priorización.
- V2: feeds OSINT públicos con filtrado, scoring avanzado y revisión manual de casos límite.

## Scoring heurístico
Esta sección resume la evolución del sistema de scoring desde la v1 a la v2.

| Campo | V1 | V2 |
|---|---|---|
| Rol del scoring | Apoyo exploratorio | Pilar del sistema |
| Nivel de formalización | Reglas básicas | Reglas gobernadas e iteradas |
| Uso principal | Filtrado manual inicial | Curación del dataset |
| Relación con el dataset | Auxiliar | Parte del proceso de curación |
| Estado | Experimental | Estable (iteraciones internas) |

- Sistema heurístico orientado a priorización, no a etiquetado automático.
- Combina señales lingüísticas, de marca e infraestructura.
- Incorpora penalizaciones explícitas para reducir falsos positivos.
- Ajustado mediante iteraciones internas tras curación manual.
- Integrado en la construcción del dataset v2.

## Features

| Aspecto | V1 | V2 |
|---|---|---|
| Enfoque | No contractual | Contractual |
| Origen | Manual / exploratorio | Derivadas de la URL |
| Dependencia del dataset | Alta | Baja |
| Redundancia | Alta | Reducida |
| Reproducibilidad | Parcial | Completa |
| Interpretabilidad | Alta | Alta |

- V2 fija un vector de features contractual, estable y reproducible.
- Se reduce la dependencia del dataset y la redundancia para evitar leakage.


| Versión               | Dataset          | Validación                                                    | Modelo              | Precision (phishing) | Recall (phishing) | F1       | ROC-AUC  | Observaciones |
| --------------------- | ---------------- | ------------------------------------------------------------- | ------------------- | -------------------- | ----------------- | -------- | -------- | ------------ |
| **v1 (exploratoria)** | 200 URLs (50/50) | Stratified CV (k=5..10)                                       | Logistic Regression | 0.92                 | 0.90              | 0.91     | 0.95     | Métricas elevadas obtenidas sobre un dataset pequeño y con features no contractuales. Útiles para exploración, pero poco robustas fuera de distribución. |
| **v2 (baseline)**     | 482 URLs (50/50) | Hold-out 80/20 + GroupShuffleSplit (anti-leakage por entidad) | Logistic Regression | **0.89**             | **0.946**         | **0.92** | **0.99** | Baseline contractual, reproducible y auditable. Buen equilibrio precision–recall bajo un esquema de validación exigente orientado a entidades no vistas. |
