Este repositorio ha sido creado como material de presentación para entrevistas técnicas.
Describe el diseño y las decisiones clave de un sistema de detección de URLs de phishing orientado específicamente a usuarios en España.

El repositorio contiene documentación sobre los principales componentes del pipeline (datos, scoring, features y modelo), con el objetivo de explicar el razonamiento detrás de cada decisión, no su implementación.

![Visión general del sistema](assets/diagrams/pipeline.png)

# Problema que aborda

El objetivo de un detector dirigido a usuarios en España es capturar patrones y relaciones específicas de este contexto que no aparecen de forma clara en el phishing global. Las campañas suelen concentrarse en un conjunto reducido de marcas nacionales, mezclan el idioma español con infraestructura genérica y se apoyan en patrones altamente repetitivos.

Además, muchas señales que en datasets globales suelen indicar phishing son habituales en portales legítimos españoles (por ejemplo, rutas como /login, /clientes o /sede). Sin una contextualización por país y dominio, los modelos entrenados con datos globales tienden a penalizar estas URLs legítimas y a aprender patrones de campaña en lugar de señales estructurales estables.

## Enfoque del sistema

El sistema parte de URLs obtenidas de feeds OSINT globales (OpenPhish, TweetFeed, PhishTank, GitHub), que presentan un alto nivel de ruido y carecen de contextualización por país. Para gestionar este problema, las URLs atraviesan primero un sistema de reglas heurísticas cuyo objetivo es reducir ruido, priorizar campañas relevantes y filtrar contenido no orientado a usuarios españoles.

Tras este prefiltrado, se realiza una curación manual final para garantizar la calidad y coherencia del dataset. Sobre este conjunto curado se extraen señales estructurales y de contexto, que capturan propiedades del dominio y del host, el contexto de legitimidad asociado a marcas y país, y patrones de riesgo de infraestructura comúnmente utilizados en campañas de phishing.

El sistema evita depender de contenido web o de señales frágiles ligadas a campañas concretas, priorizando indicadores estables que generalizan mejor y reducen el riesgo de sobreajuste.

Estas señales se utilizan como entrada de un modelo de machine learning cuya función es combinar y ponderar la información disponible de forma consistente, manteniendo una separación clara de responsabilidades dentro del pipeline.

## Decisiones técnicas

### Orientación nacional
- **Decisión:** Diseñar el sistema específicamente para usuarios españoles.
- **Motivo:** Captar señales intrínsecas a España.
- **Evita:** Falsos positivos y mala generalización.

### Calidad sobre cantidad
- **Decisión:** Construir un dataset altamente curado.
- **Motivo:** En los datasets de phishing el ruido es muy alto y degrada el funcionamiento del modelo, aunque el volumen sea mayor.
- **Evita:** Métricas infladas y aprendizaje de relaciones específicas.

### Sistema heurístico previo al ML
- **Decisión:** Introducir un sistema de reglas antes del entrenamiento.
- **Motivo:** Los feeds OSINT contienen ruido y campañas irrelevantes.
- **Evita:** Entrenar el modelo sobre datos contaminados.

### Control explícito de la representatividad por sector
- **Decisión:** Limitar y equilibrar la representación de sectores en el dataset.
- **Motivo:** Banca y logística dominan el phishing en España.
- **Evita:** Modelos que detectan sectores sobrerrepresentados en lugar de patrones de phishing.

### Features estructurales y de contexto
- **Decisión:** Priorizar señales del dominio, host e infraestructura.
- **Motivo:** Son más estables en el tiempo que el contenido web.
- **Evita:** Sobreajuste a campañas concretas.

### Modelo simple y explicable como baseline
- **Decisión:** Usar un modelo lineal interpretable como base.
- **Motivo:** Facilita auditoría, análisis de errores y ajuste de umbral.
- **Evita:** Complejidad innecesaria con datos limitados.

### Priorizar recall frente a precisión
- **Decisión:** Diseñar el sistema para minimizar falsos negativos.
- **Motivo:** Un phishing no detectado tiene mayor impacto que una alerta falsa.
- **Evita:** Optimizar métricas cómodas pero poco operativas.

### Separación estricta de responsabilidades
- **Decisión:** Diferenciar claramente heurísticas, features y modelo.
- **Motivo:** Cada bloque resuelve un problema distinto.
- **Evita:** Solapamientos y decisiones opacas.

## Limitaciones

1. **Tamaño del dataset**
   - El dataset es reducido frente a escenarios de producción.
   - Las conclusiones se centran en calidad y estabilidad.

2. **Sesgo sectorial residual**
   - Banca y logística siguen estando más representadas.
   - Otros sectores cuentan con menor cobertura.

3. **Dependencia de heurísticas iniciales**
   - El sistema parte de reglas para reducir ruido, construido inicialmente sobre el dataset v1.
   - Cambios grandes en el panorama nacional podrían requerir ajustes.

4. **Falta de validación con datos reales**
   - No ha sido probado en producción real continua.
   - Las métricas provienen de datasets curados.

5. **Alcance limitado a URLs**
   - No cubre SMS, email completo ni contenido visual.

## Documentación de soporte

- [Arquitectura del sistema](docs/arquitectura.md)
- [Datos y proceso de curación](docs/data.md)
- [Sistema de scoring heurístico](docs/scoring.md)
- [Extracción de señales y features](docs/features.md)
- [Modelo y criterios de decisión](docs/model.md)

## Próximos pasos

El siguiente paso natural del sistema es reforzar el bloque de heurísticas ampliando la sectorialización, avanzar hacia un dataset más representativo y complementar el enfoque actual con señales semánticas de mayor nivel que permitan estimar de forma más realista el ruido y la detección fuera de la distribución.
