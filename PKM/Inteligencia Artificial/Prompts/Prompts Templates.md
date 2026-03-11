
Revise este plan detenidamente antes de realizar cualquier cambio en el código. Para cada problema o recomendación, explique las desventajas concretas, deme una recomendación con criterio y solicite mi opinión antes de tomar una decisión.

Mis preferencias de ingeniería (úselas para guiar sus recomendaciones):


DRY es importante: marque la repetición con firmeza.
Un código bien probado es innegociable; prefiero demasiadas pruebas que muy pocas.
Quiero código con un diseño lo suficientemente preciso, no insuficiente (frágil, chapucero) ni excesivo (abstracción prematura, complejidad innecesaria).
Prefiero manejar más casos extremos, no menos; consideración > velocidad.
Prefiero lo explícito a lo ingenioso.
1. Revisión de la arquitectura
Evaluar:
Diseño general del sistema y límites de los componentes.
Problemas con el grafo de dependencias y el acoplamiento. Patrones de flujo de datos y posibles cuellos de botella.
Características de escalado y puntos únicos de fallo.
Arquitectura de seguridad (autenticación, acceso a datos, límites de la API).
2. Revisión de la calidad del código
Evaluar:
Organización del código y estructura del módulo.
Infracciones de DRY: ser agresivo en este aspecto.
Patrones de gestión de errores y casos extremos omitidos (indíquelos explícitamente).
Puntos críticos de deuda técnica.
Áreas con ingeniería excesiva o insuficiente en comparación con mis preferencias.
3. Revisión de pruebas
Evaluar:
Brechas en la cobertura de pruebas (unidad, integración, extremo a extremo).
Calidad de las pruebas y solidez de las aserciones.
Cobertura de casos extremos omitida: ser exhaustivo.
Modos de fallo y rutas de error no probados.
4. Revisión del rendimiento
Consultas N+1 y patrones de acceso a la base de datos.
Evaluar:
Problemas de uso de memoria.
Oportunidades de almacenamiento en caché.
Ruta de código lenta o de alta complejidad. Para cada problema que encuentres
Para cada problema específico (error, mal funcionamiento, problema de diseño o riesgo):
Describe el problema de forma concreta, con referencias a archivos y líneas.
Presenta de 2 a 3 opciones, incluyendo "no hacer nada" cuando sea razonable.
Para cada opción, especifica: esfuerzo de implementación, riesgo, impacto en otro código y carga de mantenimiento.
Dame la opción que recomiendas y por qué, en relación con mis preferencias anteriores.
Luego, pregúntame explícitamente si estoy de acuerdo o si quiero tomar una dirección diferente antes de continuar.
Flujo de trabajo e interacción
No supongas mis prioridades en cuanto a plazos o escala.
Después de cada sección, haz una pausa y solicita mi opinión antes de continuar.
ANTES DE EMPEZAR:
Pregúntame si prefiero una de estas dos opciones:
1/ CAMBIO GRANDE: Analiza esto de forma interactiva, sección por sección (Arquitectura → Pruebas de calidad del código → Rendimiento) con un máximo de 4 problemas principales en cada una.
2/ CAMBIO PEQUEÑO: Analiza interactivamente UNA pregunta por sección de revisión. PARA CADA ETAPA DE LA REVISIÓN: presente la explicación, los pros y los contras de las preguntas de cada etapa, así como su recomendación y el porqué. Luego, utilice AskUserQuestion. También indique el NÚMERO de problemas y luego indique las opciones en LETRAS. Al usar AskUserQuestion, asegúrese de que cada opción indique claramente el NÚMERO del problema y la LETRA de la opción para evitar confusiones. La opción recomendada siempre debe ser la primera.