# SimPlant Gateway

## Enunciado del Problema

Se debe diseñar un sistema que permita conectar el Runtime con los sistemas de control de planta (DCS, PLC, SCADA), abstrayendo los diferentes protocolos industriales detrás de una interfaz uniforme, para lo cual se describe la operatoria que debiera realizar el sistema. La lógica de control y decisión de setpoints queda fuera del modelado (es responsabilidad del Runtime).

El Gateway actúa como adaptador entre el mundo de SimPlant y el mundo de los sistemas de control industrial. Debe implementar una interfaz estándar que el Runtime consume, independientemente del protocolo subyacente. Esta interfaz define operaciones para conectar, desconectar, leer estado de planta, escribir setpoints, y verificar salud de la conexión.

Cuando el Gateway se inicia, recibe una configuración que especifica el protocolo a utilizar (OPC-UA, Modbus TCP, Modbus RTU, MQTT), los parámetros de conexión (dirección IP, puerto, credenciales si aplica), y un mapeo de tags que relaciona los nombres internos de SimPlant con los identificadores específicos del sistema de control (por ejemplo, node IDs en OPC-UA o direcciones de registros en Modbus). El Gateway intenta establecer conexión con el sistema de control y reporta éxito o fallo al Runtime.

Para la lectura de estado, el Runtime invoca una operación que retorna todas las mediciones configuradas en un único llamado. El Gateway debe leer cada tag del sistema de control, convertir el valor al tipo de dato esperado (típicamente punto flotante), y obtener el indicador de calidad que el protocolo provea (en OPC-UA esto es explícito; en Modbus debe inferirse de timeouts o excepciones). Si alguna lectura falla, el Gateway debe reportar la falla específica sin abortar la lectura de los demás tags.

Para la escritura de setpoints, el Runtime invoca una operación pasando los setpoints validados. El Gateway debe escribir cada valor en el tag correspondiente del sistema de control y reportar el resultado de cada escritura. Es crítico que el Gateway nunca modifique los valores recibidos; su única responsabilidad es la traducción de protocolo, no la validación de valores (eso es responsabilidad del Motor de Restricciones en el Runtime).

El Gateway debe manejar desconexiones y reconexiones de forma transparente. Si la conexión con el sistema de control se pierde, el Gateway debe intentar reconectar automáticamente con una estrategia de backoff exponencial, mientras reporta al Runtime que la conexión no está disponible. Durante la desconexión, el Gateway puede almacenar temporalmente los últimos valores leídos en un buffer, pero debe marcarlos claramente como datos históricos (no frescos) para que el Runtime tome decisiones informadas.

Se deben implementar adaptadores para al menos los siguientes protocolos: OPC-UA (el estándar moderno para comunicación industrial, soportado por la mayoría de DCS y SCADA modernos), Modbus TCP (protocolo legacy muy común en PLCs), y un adaptador de Simulación que no conecta con ningún sistema real sino que genera datos sintéticos para pruebas y desarrollo. Cada adaptador implementa la misma interfaz, permitiendo al Runtime operar sin conocer qué protocolo se está usando debajo.

---

# Documentos relacionados

## Módulo Gateway
Este es el documento principal del módulo Gateway.

## Arquitectura SimPlant - Mapa de Módulos

### 🔌 Interfaces directas
- [[Runtime]] - Módulo que consume esta interfaz Gateway para controlar la planta
  - Recibe comandos de setpoints validados
  - Retorna estado de planta con calidades

### 🧠 Modelos y Entrenamiento
- [[Trainer]] - Módulo de entrenamiento de agentes RL
  - Exporta modelos ONNX usados por Runtime
  - Puede usar [[Environment|SimulationAdapter]] para pruebas

### 🌍 Entornos de Simulación
- [[Environment]] - Interfaz de entornos Gymnasium
  - [[Gateway/Conceptual|SimulationAdapter]] implementa interfaz similar para testing

## Diseños detallados de Gateway
- [[Conceptual]] - Diagrama de clases y flujos conceptuales
- [[Tecnico]] - Stack tecnológico y diagramas de secuencia