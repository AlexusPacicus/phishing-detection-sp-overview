## Propósito
Este documento describe el rol del modelo de machine learning dentro del sistema.

El modelo combina y pondera señales previamente filtradas y contextualizadas para generar una estimación de riesgo, actuando como un componente más del pipeline y no como un detector independiente.

El modelo se define como una base, diseñada para validar el funcionamiento del pipeline y servir como referencia estable sobre la que evaluar futuras extensiones del sistema.

## Principios de diseño del modelo

- **Modelo como combinador de señales:** el modelo no aprende directamente a detectar phishing desde URLs crudas ni incorpora lógica heurística; su función se limita a combinar y ponderar señales previamente filtradas y contextualizadas por el sistema.
- **Simplicidad e interpretabilidad:** se prioriza un modelo sencillo e interpretable, adecuado para escenarios con datasets reducidos, que permita analizar el impacto de cada señal y mantener un comportamiento estable.
- **Control explícito de la decisión:** la salida del modelo se interpreta como una estimación de riesgo, permitiendo ajustar umbrales de decisión sin necesidad de modificar ni reentrenar el modelo.
- **Modelo como línea base contractual:** el modelo se define como una línea base estable destinada a validar el funcionamiento del pipeline y servir como referencia para futuras evoluciones del sistema.

## Tipo de modelo utilizado

El sistema utiliza un modelo supervisado de carácter lineal.

El modelo produce una salida continua que representa una estimación de riesgo, sin emitir decisiones binarias directas. Esta elección es coherente con el diseño del pipeline, donde la interpretación final de la salida se desacopla del propio modelo.

## Por qué este modelo y no otro

La elección de un modelo lineal responde a la naturaleza del sistema y a los objetivos del proyecto, no a la búsqueda de complejidad o rendimiento máximo.

El conjunto de señales de entrada es reducido, explicable y diseñado para capturar dimensiones estables del problema. En este contexto, modelos más complejos no aportarían una mejora clara y sí introducirían mayor riesgo de sobreajuste y pérdida de interpretabilidad.

Arquitecturas más sofisticadas, como modelos no lineales o redes neuronales, suelen requerir volúmenes de datos mayores y señales más ricas para generalizar correctamente. Dado el tamaño y el carácter curado del dataset, su uso resultaría desproporcionado y dificultaría el análisis de errores.

Asimismo, enfoques end-to-end basados en URLs crudas o contenido textual entrarían en conflicto con el diseño del sistema, que prioriza la separación explícita entre filtrado heurístico, extracción de señales y decisión final.

En este escenario, un modelo lineal proporciona un equilibrio adecuado entre capacidad de combinación, estabilidad, control y explicabilidad, alineándose con el papel que el modelo debe desempeñar dentro del pipeline.

## Interpretación de la salida

La salida del modelo representa una estimación de riesgo asociada a cada URL, no una decisión binaria definitiva.

La interpretación de esta salida se realiza fuera del propio modelo, mediante umbrales configurables que permiten adaptar el comportamiento del sistema al contexto operativo sin necesidad de modificar ni reentrenar el modelo.

## Limitaciones conocidas

- **Dataset de tamaño reducido:** el modelo se entrena sobre un conjunto de datos curado y limitado, lo que condiciona la complejidad del modelo y el tipo de patrones que puede aprender.

- **Dependencia de la calidad de las señales:** el rendimiento del modelo está directamente ligado a la calidad y estabilidad de las señales de entrada; errores o sesgos en etapas previas se propagan al modelo.

- **Validación fuera de producción:** el modelo no ha sido evaluado en un entorno de producción continuo, por lo que su comportamiento frente a drift real debe considerarse no validado.

- **Alcance limitado a URLs:** el modelo no incorpora información de contenido, contexto temporal ni otros canales como SMS, email completo o señales visuales.
