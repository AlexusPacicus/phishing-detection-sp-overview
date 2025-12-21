## Propósito
La extracción de señales transforma la URL en una representación estructurada, capturando propiedades estables del dominio, la infraestructura y el contexto.

La extracción de señales constituye una capa intermedia del sistema, situada entre el filtrado heurístico y el modelo de machine learning.

## Principios de diseño

- **Exclusión deliberada de señales semánticas:** en esta versión del sistema se priorizan señales estructurales y de contexto frente a señales semánticas de contenido. Esta decisión busca maximizar la estabilidad temporal y reducir el sobreajuste, dejando la incorporación de señales semánticas como una extensión natural en versiones futuras sin alterar la arquitectura del sistema.

- **Whitelist controlada de dominios oficiales:** se utiliza una lista curada de dominios oficiales españoles como referencia de legitimidad, con el objetivo de reducir falsos positivos y mejorar la contextualización de señales estructurales.

- **Cobertura completa de la estructura de la URL:** el diseño de las señales considera los distintos componentes de la URL (dominio, subdominios, ruta y parámetros), evitando depender de una única parte.

- **Diseño por dimensiones no solapadas:** las señales se definen para capturar dimensiones distintas del problema, evitando que múltiples señales describan el mismo fenómeno.

- **Estabilidad temporal como criterio de diseño:** el sistema prioriza señales estructurales con potencial de generalización en el tiempo, asumiendo que su estabilidad real debe ser validada mediante observación y reevaluación continuas.

- **Señales diseñadas para ser revisables:** ninguna señal se considera definitiva; su validez se evalúa a partir del comportamiento observado del sistema.

## Familias de señales
Las señales se agrupan en familias que ataquen diferentes materializaciones del problema, evitando solapamientos y facilitando el análisis y evolución del sistema. 

### Señales estructurales de dominio y host

Capturan propiedades estructurales del dominio y sus subdominios, reflejando patrones de complejidad y jerarquía habituales en campañas de phishing.

- Describen construcciones anómalas o innecesariamente complejas en la estructura del dominio.
- Son costosas de evadir sin perder capacidad de suplantación, ya que simplificar la estructura suele reducir el realismo del engaño.
- No dependen de contenido ni de tokens semánticos; su interpretación se contextualiza mediante una whitelist controlada de dominios oficiales para evitar falsos positivos.

### Señales de infraestructura

Describen características asociadas al **entorno técnico de alojamiento y distribución** de la URL, aportando contexto sobre el coste operativo y las decisiones de despliegue del atacante.

- Reflejan el uso de infraestructura genérica, compartida o de bajo coste, frecuente en campañas de phishing.
- Tienden a ser más estables en el tiempo que el contenido o los tokens de la URL, ya que cambiar de infraestructura implica un coste operativo real.
- Complementan a las señales estructurales del dominio, aportando información independiente sobre el entorno en el que se aloja el recurso.

### Señales de contexto de legitimidad

Introducen referencias externas conocidas para anclar el sistema a dominios legítimos del contexto español, reduciendo falsos positivos estructurales frecuentes en datasets globales.

- La whitelist se construye a partir de dominios oficiales de las principales entidades del panorama nacional, seleccionados de forma controlada como ancla de legitimidad. Su diseño es extensible, permitiendo incorporar nuevas entidades relevantes sin alterar el funcionamiento del sistema.
- Aunque la whitelist desempeña un papel relevante, su alcance se mantiene deliberadamente limitado para evitar introducir un sesgo estructural que lleve al modelo a interpretar cualquier dominio fuera de ella como phishing.
- Separan de forma explícita la legitimidad conocida del resto de señales genéricas de riesgo, evitando que ambas dimensiones se mezclen.

Estas señales no introducen lógica de decisión, sino que aportan contexto para la combinación de señales en el modelo.

### Señales de abuso de marca y contexto país

Capturan la introducción deliberada de referencias a marcas, entidades o servicios relevantes del contexto español fuera de sus dominios oficiales.
- Detectan el uso de marcas nacionales en estructuras de URL no asociadas a dominios legítimos.
- Representan un intento explícito de aumentar credibilidad y tasa de éxito del engaño.
- Se interpretan siempre en combinación con señales de legitimidad, evitando penalizar referencias legítimas dentro de dominios oficiales.

Estas señales distinguen entre presencia de marca y legitimidad del dominio, evitando asumir que la mera aparición de una marca implica comportamiento malicioso.

## Alcance de la extracción de señales

La extracción de señales no pretende capturar intención del atacante, contenido visual ni semántica del mensaje. Su objetivo es describir propiedades estructurales y contextuales de la URL como artefacto técnico, dejando la decisión final al modelo de machine learning.
