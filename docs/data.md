# Datos
Este documento describe el enfoque usdo en el tratamiento de datos.
El dataset se construye priorizando calidad y representatividad frente a volumen, evitando redundancias y manteniendo coherencia con el panorama real del phishing dirigido a usuarios en España.

## Tipo de datos
- URLs etiquetadas como phishing o legítimas.
- Enfoque específico en URLs dirigidas a usuarios en España.
- Presencia controlada de sectores y entidades relevantes en el contexto nacional.

## Origen y curación
- Fuentes públicas de phishing filtradas y revisadas.
- Enfoque híbrido: scraping controlado y selección manual de URLs legítimas de dominios oficiales.
- Curación basada en reglas heurísticas y validación final para eliminar campañas repetidas y casos no representativos.

## Principios de diseño
- **Calidad sobre volumen**: se prioriza la coherencia y estabilidad del dataset frente a su tamaño.
- **Control por entidad y sector**: se evita la sobre-representación de una misma entidad o sector.
- **Separación datos / features**: la selección y curación de datos se realiza antes de cualquier extracción de features para prevenir leakage.

## Riesgos y limitaciones
- Tamaño limitado del dataset.
- Sesgo sectorial inevitable, reflejo del panorama real.
- Dependencia de reglas previas en la selección de datos.
