# Sistema de scoring heurístico

Este documento describe el sistema de scoring heurístico, aplicado **antes de la extracción de señales**, orientado a la reducción de ruido y priorización de campañas relevantes para usuarios españoles antes de su incorporación al pipeline de machine learning.

## 1. Propósito del sistema de scoring

El sistema de scoring heurístico actúa como una **capa de control previa** cuyo objetivo principal es prefiltrar la entrada de datos al sistema. Su función no es clasificar URLs, sino reducir ruido, colapsar campañas repetidas y priorizar información alineada con el contexto objetivo.

## 2. Por qué existe el scoring

Los feeds OSINT globales proporcionan grandes volúmenes de URLs, pero presentan un alto nivel de ruido, campañas repetidas y una baja representatividad de datos dirigidos a usuarios españoles.

Utilizar estos datos de forma directa, sin un proceso previo de filtrado y priorización, introduce aleatoriedad en el entrenamiento y favorece el aprendizaje de patrones ligados a campañas concretas. Esto degrada la estabilidad del sistema y dificulta su adaptación en el tiempo.

El scoring heurístico permite reducir esta variabilidad, controlar la calidad del dato de entrada y hacer viable la escalabilidad del sistema sin comprometer su coherencia.

## 3. Rol del scoring en la arquitectura

Dentro de la arquitectura del sistema, el scoring heurístico cumple un rol claramente delimitado:

- Reduce ruido presente en los feeds OSINT.
- Prioriza campañas y URLs alineadas con el contexto español.
- Elimina duplicados y campañas repetidas para evitar sesgos por sobre-representación.

El sistema de scoring **no clasifica URLs**, **no sustituye al modelo de machine learning** y **no emite decisiones finales**, actuando exclusivamente como una capa de prefiltrado y control.

## 4. Riesgos y limitaciones

- **Especialización sectorial:** el sistema de scoring ha sido afinado principalmente sobre campañas de banca y logística (especialmente Correos), lo que puede limitar su generalización a otros sectores con menor representación.

- **Acoplamiento al dataset inicial:** el diseño del scoring y la selección de señales se ha construido iterativamente a partir del análisis de falsos positivos y falsos negativos del primer dataset curado. Este proceso introduce relaciones implícitas que pueden sesgar el comportamiento del sistema hacia patrones ya observados.

- **Cobertura limitada de señales complejas:** el scoring prioriza señales estructurales y de contexto, por lo que no captura patrones semánticos de alto nivel ni variaciones sutiles de contenido.
