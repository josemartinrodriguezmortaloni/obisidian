**SimPlant Lab es el entorno de simulación donde los modelos matemáticos se incuban, entrenan y validan antes de interactuar con la realidad física.** Es un espacio virtual de riesgo cero diseñado para el Ingeniero de Control y Machine Learning. Aquí, mediante Aprendizaje por Refuerzo (RL), el sistema ejecuta millones de iteraciones aceleradas contra un gemelo digital del proceso industrial (basado en primeros principios termodinámicos o datos históricos). El agente de IA explora diferentes estrategias operativas y ajusta sus políticas basándose en funciones de recompensa que penalizan la ineficiencia y premian el rendimiento. El único propósito del Lab es producir y exportar un modelo matemático determinista validado (un archivo `.onnx`) que luego será desplegado en el sistema de control de la planta real.

# SimPlant ¿Qué es?

SimPlant es una **arquitectura de control bicameral** que actúa como el sistema nervioso central para la termodinámica industrial. Transforma el caos de los datos de una planta en una Ontología estructurada (un gemelo digital semántico), sobre la cual despliega agentes de Aprendizaje por Refuerzo (RL) para calcular setpoints óptimos en tiempo real.

No manipula equipos físicamente de forma directa ni ciega. Funciona bajo un paradigma estricto: ingiere la entropía de los sensores, limpia la señal, calcula los objetivos matemáticamente óptimos, los somete a un **motor de restricciones determinista inquebrantable**, y finalmente los envía a los controladores PID existentes.

# Objetivos

Se debe diseñar la interfaz para un sistema que permita entrenar agentes de Aprendizaje por Refuerzo para control de procesos industriales, para lo cual se describe la operatoria que debiera realizar el sistema. La forma de ejecutar el entrenamiento en infraestructura distribuida queda fuera del modelado.

El ingeniero de control, cuando desea entrenar un nuevo modelo para una planta específica, debe primero configurar un entorno de simulación que represente el proceso a controlar. El sistema permite registrar múltiples entornos de simulación, cada uno definido por sus variables de estado (las mediciones que el agente observa, como temperaturas, presiones, flujos), sus variables de acción (los setpoints que el agente puede modificar), y una función de recompensa que cuantifica qué tan buena es una decisión (típicamente basada en eficiencia energética, estabilidad del proceso y cumplimiento de especificaciones de producto). El entorno debe implementar una interfaz estándar tipo Gymnasium que permita al sistema de entrenamiento interactuar con él de forma uniforme.

Una vez configurado el entorno, el ingeniero selecciona un algoritmo de entrenamiento entre los disponibles (PPO, SAC, TD3). Cada algoritmo tiene hiperparámetros configurables como tasa de aprendizaje, tamaño de batch, factor de descuento, entre otros. El sistema permite ejecutar búsquedas automáticas de hiperparámetros para encontrar la mejor configuración. Durante el entrenamiento, el sistema registra métricas de progreso (recompensa promedio, pérdida del modelo, entropía de la política) que el ingeniero puede visualizar para monitorear la convergencia.

El entrenamiento puede ejecutarse por un número fijo de iteraciones o hasta que se alcance un criterio de convergencia definido por el ingeniero. Al finalizar, el sistema genera checkpoints intermedios que permiten retomar el entrenamiento si se interrumpe. El ingeniero puede evaluar el modelo entrenado ejecutándolo en el entorno de simulación sin aprendizaje, obteniendo métricas de rendimiento como recompensa total, tiempo de estabilización, y violaciones de restricciones.

Cuando el ingeniero está satisfecho con el rendimiento del modelo, solicita exportarlo a formato ONNX para su uso en producción. El sistema genera un archivo portable que contiene la política entrenada, junto con metadatos que describen las variables de entrada esperadas y las salidas producidas. Es importante que el archivo exportado sea independiente del framework de entrenamiento (PyTorch, TensorFlow) para que pueda ejecutarse en el Runtime sin dependencias pesadas. El sistema mantiene un registro versionado de todos los modelos exportados, permitiendo al ingeniero comparar versiones y hacer rollback si un modelo nuevo no funciona como se esperaba en planta.

## Requisitos
1. La librería debe poder configurar un entorno de simulación pudiéndolo importar de SimPlant Environment.
2. SimPlant Trainer debe ser una librería que entrenar la parte cognitiva de un sistema operativo de realidad industrial qué es SimPlant en conjunto
3. La librería debe permitir la selección de modelos pre-definidos (PPO, SAC, TD3, etc).
4. La librería debe permitir configurar dichos modelos.
5. La librería debe registrar métricas de progreso para evaluar el entrenamiento del modelo
6. La librería debe permitir poder continuar el entrenamiento del modelo desde el punto donde se interrumpió el entrenamiento.
7. La librería debe permitir configurar epochs o el número de iteraciones del entrenamiento del modelo.
8. La librería debe permitir exportar el modelo a ONNX.
9. La librería debe tener un sistema de versionado de todos los modelos exportados.