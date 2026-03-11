# SimPlant: Sistema Operativo de Inteligencia Industrial

## Qué es

SimPlant es una **arquitectura de control bicameral** que actúa como el sistema nervioso central para la termodinámica industrial. Transforma el caos de los datos de una planta en una Ontología estructurada (un gemelo digital semántico), sobre la cual despliega agentes de Aprendizaje por Refuerzo (RL) para calcular setpoints óptimos en tiempo real.

No manipula equipos físicamente de forma directa ni ciega. Funciona bajo un paradigma estricto: ingiere la entropía de los sensores, limpia la señal, calcula los objetivos matemáticamente óptimos, los somete a un **motor de restricciones determinista inquebrantable**, y finalmente los envía a los controladores PID existentes.

## Principios Claves

Derivados del Axioma Cero (el software modela la realidad para resolver problemas humanos), SimPlant se rige por tres leyes fundacionales:

1. **Bicameralidad Cognitiva (Safety-First):** La inteligencia propone, la física dispone. El hemisferio estocástico (RL) tiene libertad para explorar la máxima eficiencia económica; el hemisferio determinista (Constraint Engine) bloquea físicamente cualquier comando que viole las leyes de la termodinámica o los límites del equipo. La IA nunca tiene control directo.
    
2. **Agnosticismo de Infraestructura:** SimPlant no exige hardware propietario. Se asienta sobre la topología heredada de la planta (OPC-UA, Modbus, MQTT), extrayendo la telemetría existente para construir su modelo del mundo.
    
3. **Evolución Anclada a la Realidad:** El mapa debe coincidir con el territorio. Mediante el módulo de _Twin Calibration_, el sistema ajusta continuamente sus modelos de simulación para reflejar el desgaste físico real de los metales, cañerías y bombas.
    

## El Problema Fundamental

Los procesos industriales sufren de dos enfermedades estructurales que destruyen los márgenes de ganancia:

1. **La Entropía de Datos (Garbage In):** Las plantas producen millones de señales por segundo. Estos datos viven en silos, provienen de sensores ruidosos, llegan de forma asíncrona o están derechamente rotos. Un modelo matemático que opere sobre esta ilusión eléctrica tomará decisiones catastróficas.
    
2. **La Fricción Cognitiva Humana:** Un operador humano es incapaz de calcular la interacción simultánea de 50 variables termodinámicas.
    

**El Ciclo Vicioso del Fracaso Operativo:**

Cuando los datos son ruidosos o los lazos de control básicos (PIDs) fallan, el operador humano pierde la confianza. Para evitar un accidente, pasa el sistema a **modo manual** y adopta márgenes de seguridad excesivamente conservadores. Esto garantiza la supervivencia a corto plazo, pero destruye sistemáticamente la eficiencia energética y la calidad del producto.

## Cómo lo Solucionamos

SimPlant rompe este ciclo transformando la operación de un estado _reactivo-humano_ a uno _proactivo-matemático_, dividiendo la solución en cuatro capas de responsabilidad localizada:

1. **Acondicionamiento de la Realidad (Signal Conditioning):** Antes de que la IA "piense", un pipeline puramente matemático ingiere los datos asíncronos, filtra el ruido eléctrico, detecta anomalías (ej. un sensor congelado) y entrega un vector de estado coherente y temporalmente alineado.
    
2. **Optimización Cognitiva (Agente RL):** Un modelo de Inteligencia Artificial que comprende la física de la planta calcula de forma continua los setpoints exactos para minimizar el consumo energético y maximizar el rendimiento.
    
3. **El Foso Defensivo (Constraint Engine):** Un motor de reglas de física pura intercepta la decisión de la IA. Evalúa límites absolutos y velocidades de cambio máximas (Rate of Change). Si la IA alucina, el motor corrige el valor al límite seguro más cercano de forma determinista.
    
4. **Observabilidad y Confianza Humana:** A través de la UI del Operador, SimPlant no solo dicta qué hacer; traduce su matemática a explicabilidad operativa. Proyecta el impacto económico y físico del setpoint sugerido antes de aplicarlo, permitiendo al operador aprobar la acción (_Advisory Mode_) hasta que exista la confianza para el lazo cerrado automático (_Closed-Loop_).
    

**La Transformación Estructural:**

|**Dimensión**|**Realidad Actual (Sin SimPlant)**|**La Solución SimPlant**|
|---|---|---|
|**Control de Setpoints**|Ajuste manual reactivo basado en la intuición|Cálculo matemático óptimo y continuo|
|**Garantía de Seguridad**|Depende de la vigilia y el criterio humano|Motor de restricciones de código rígido|
|**Gestión del Desgaste**|Re-sintonización manual de PIDs (meses de trabajo)|Calibración automática del gemelo digital|
