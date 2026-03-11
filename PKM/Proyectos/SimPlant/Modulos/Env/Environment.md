# SimPlant Environments

## Enunciado del Problema

Se debe diseñar una librería que permita modelar procesos industriales como entornos de simulación compatibles con Gymnasium, de forma que el Trainer pueda consumirlos a través de una interfaz uniforme sin conocer los detalles internos de cada proceso. La definición del algoritmo de entrenamiento y la exportación de modelos queda fuera del modelado (es responsabilidad del Trainer).

El ingeniero de proceso, cuando desea incorporar un nuevo proceso industrial al sistema (un reactor CSTR, un intercambiador de calor, una columna de destilación), debe poder registrarlo como un entorno de simulación. Para ello define las variables de estado que el agente observará (temperaturas, presiones, flujos, concentraciones), las variables de acción expresadas como setpoints (no como acciones directas de control — el agente decide *a dónde* ir, no *cómo* llegar; el PID interno se encarga de la ejecución táctica), y una función de recompensa que cuantifica la calidad de cada decisión. Esta separación entre setpoint y acción de control es fundamental: el agente SAC opera como supervisor estratégico que piensa en minutos u horas, mientras que los controladores PID internos operan en milisegundos para alcanzar los objetivos propuestos.

Cada proceso se modela como un entorno independiente que encapsula su dinámica particular: las ecuaciones diferenciales que rigen su comportamiento, las interacciones termodinámicas entre componentes, y las restricciones operativas inherentes al equipo. El entorno debe ser capaz de simular internamente la respuesta del PID ante el setpoint recibido, avanzar la dinámica del proceso, y reportar el nuevo estado resultante junto con una señal de recompensa. El Trainer interactúa con estos entornos exclusivamente a través de la interfaz estándar de Gymnasium (`reset()`, `step()`, `observation_space`, `action_space`), lo que permite entrenar agentes de forma agnóstica al proceso específico.

La función de recompensa es una pieza crítica del diseño porque codifica los objetivos del negocio: eficiencia energética (minimizar consumo para un nivel de producción dado), estabilidad del proceso (penalizar oscilaciones y transitorios bruscos), y cumplimiento de especificaciones de producto (concentración, pureza, temperatura de salida). Estas componentes deben poder ponderarse según las prioridades del proceso particular, y el ingeniero debe poder definir funciones de recompensa personalizadas o componer las existentes.

Los entornos deben evolucionar en fidelidad sin romper a sus consumidores. En una primera fase, un CSTR puede modelarse con ODEs simplificadas a través de PC-Gym. En una segunda fase, se incorpora NeqSim para obtener propiedades termodinámicas rigurosas (equilibrios de fase, propiedades de mezcla reales). En una tercera fase, el proceso se modela completo en IDAES-PSE con flowsheets de múltiples equipos conectados. Estas transiciones deben ser transparentes para el Trainer: el entorno sigue exponiendo la misma interfaz Gymnasium, con los mismos espacios de observación y acción; lo que cambia es la fidelidad interna de la simulación.

El sistema debe permitir también ejecutar entornos de forma standalone, sin necesidad del Trainer, para que el ingeniero pueda depurar el comportamiento del simulador, visualizar la dinámica del proceso, y verificar que el entorno responde razonablemente ante distintos setpoints antes de invertir tiempo en entrenamiento.

---

## Interfaces

### SimulationEnvironment

Define cómo cualquier consumidor (Trainer u otro) interactúa con un entorno de simulación. Esta es la interfaz pública central de la librería, compatible con `gymnasium.Env`.

```
SimulationEnvironment
├── observation_space    → Descripción del espacio de estados (dimensiones, rangos)
├── action_space         → Descripción del espacio de acciones/setpoints (dimensiones, rangos)
├── reset()              → Reinicia el entorno, retorna estado inicial
├── step(action)         → Recibe setpoint, simula respuesta PID + dinámica, retorna (nuevo_estado, recompensa, terminado, truncado, info)
├── render()             → Visualización opcional del estado actual
└── close()              → Libera recursos del simulador
```

### RewardFunction

Define cómo se calcula y compone la señal de recompensa.

```
RewardFunction
├── calculate(state, action, next_state, info)  → Calcula recompensa escalar para un paso
├── get_components()                             → Retorna nombres y pesos de cada componente
└── describe()                                   → Retorna descripción legible de la función
```

### ProcessModel

Define la interfaz del modelo de proceso subyacente (lo que PC-Gym, IDAES o ODEs custom implementan).

```
ProcessModel
├── initialize(config)          → Configura el modelo con parámetros del proceso
├── advance(setpoints, dt)      → Avanza la simulación un paso temporal con los setpoints dados
├── get_state()                 → Retorna el estado actual del proceso (vector de mediciones)
├── reset_to_initial()          → Reinicia el proceso a condiciones iniciales
└── get_process_info()          → Retorna metadatos del proceso (nombre, variables, unidades)
```

### EnvironmentRegistry

Define cómo se registran y descubren entornos disponibles.

```
EnvironmentRegistry
├── register(env_id, env_factory, metadata)  → Registra un entorno con su factory y descripción
├── make(env_id, config)                     → Crea instancia de un entorno registrado
├── list_available()                         → Retorna lista de entornos registrados con metadata
└── get_metadata(env_id)                     → Retorna metadata de un entorno específico (espacios, descripción)
```

---

## Tipos de Datos

```
EnvironmentConfig
├── process_model_type     → Tipo de modelo de proceso (pcgym, idaes, custom)
├── process_params         → Parámetros específicos del proceso (constantes cinéticas, geometría, etc.)
├── observation_variables  → Lista de variables observables con rangos
├── action_variables       → Lista de setpoints controlables con rangos
├── reward_config          → Configuración de la función de recompensa
├── episode_length         → Duración máxima de un episodio en pasos
└── dt                     → Paso temporal de simulación (segundos)

ObservationVariable
├── name                   → Nombre de la variable (ej: "reactor_temperature")
├── unit                   → Unidad de medida (ej: "°C")
├── low                    → Límite inferior del rango observable
└── high                   → Límite superior del rango observable

ActionVariable
├── name                   → Nombre del setpoint (ej: "temperature_setpoint")
├── unit                   → Unidad de medida (ej: "°C")
├── low                    → Setpoint mínimo permitido
└── high                   → Setpoint máximo permitido

RewardConfig
├── components             → Lista de RewardComponent
└── aggregation            → Método de agregación ("weighted_sum", "product", "min")

RewardComponent
├── name                   → Nombre del componente (ej: "energy_efficiency")
├── weight                 → Peso relativo del componente
├── type                   → Tipo de cálculo ("minimize", "maximize", "target", "penalty")
├── target_value           → Valor objetivo (para type="target")
└── parameters             → Parámetros adicionales específicos del componente

EnvironmentMetadata
├── env_id                 → Identificador único (ej: "cstr-basic-v1")
├── description            → Descripción del proceso modelado
├── observation_space_info → Descripción de variables observables
├── action_space_info      → Descripción de setpoints controlables
├── fidelity_level         → Nivel de fidelidad ("basic", "thermodynamic", "rigorous")
└── process_model_type     → Backend de simulación utilizado
```

---

## Historias de Usuario

### US-E01: Registrar un nuevo entorno de proceso

**Como** ingeniero de proceso  
**Quiero** registrar un nuevo entorno de simulación definiendo sus variables de estado, setpoints controlables y función de recompensa  
**Para** que el Trainer pueda entrenar un agente específico para mi proceso

**Criterios de aceptación:**
- [ ] Puedo especificar las variables de observación con sus nombres, unidades y rangos
- [ ] Puedo especificar los setpoints controlables con sus nombres, unidades y rangos
- [ ] Puedo asociar un modelo de proceso (PC-Gym, IDAES, custom)
- [ ] El entorno se registra con un identificador único y metadata descriptiva
- [ ] El sistema valida que el entorno implementa la interfaz `SimulationEnvironment`

---

### US-E02: Configurar función de recompensa para un proceso

**Como** ingeniero de proceso  
**Quiero** definir una función de recompensa compuesta por múltiples componentes ponderados  
**Para** que el agente optimice según los objetivos específicos de mi proceso

**Criterios de aceptación:**
- [ ] Puedo agregar componentes de recompensa predefinidos (eficiencia energética, estabilidad, calidad de producto)
- [ ] Puedo asignar pesos relativos a cada componente
- [ ] Puedo definir funciones de recompensa personalizadas que cumplan la interfaz `RewardFunction`
- [ ] El sistema valida que los pesos sean consistentes
- [ ] Puedo describir la función de recompensa en forma legible para auditoría

---

### US-E03: Validar cumplimiento con interfaz Gymnasium

**Como** ingeniero de proceso  
**Quiero** que el sistema valide que mi entorno cumple con la interfaz estándar de Gymnasium  
**Para** asegurar compatibilidad con el Trainer antes de entrenar

**Criterios de aceptación:**
- [ ] El sistema verifica que `observation_space` y `action_space` son instancias válidas de `gymnasium.spaces`
- [ ] El sistema verifica que `reset()` retorna `(observation, info)` con la forma correcta
- [ ] El sistema verifica que `step(action)` retorna `(observation, reward, terminated, truncated, info)` con tipos correctos
- [ ] El sistema reporta errores específicos si alguna verificación falla
- [ ] La validación puede ejecutarse antes del entrenamiento

---

### US-E04: Ejecutar entorno standalone para depuración

**Como** ingeniero de proceso  
**Quiero** ejecutar un entorno interactivamente sin necesidad del Trainer  
**Para** depurar la dinámica del proceso y verificar que el simulador se comporta razonablemente

**Criterios de aceptación:**
- [ ] Puedo crear una instancia del entorno y ejecutar `step()` manualmente con setpoints elegidos
- [ ] Puedo visualizar la evolución del estado del proceso a lo largo de los pasos
- [ ] Puedo graficar las variables de estado, los setpoints aplicados y la recompensa obtenida
- [ ] La ejecución standalone no requiere dependencias del Trainer (Ray, RLlib)

---

### US-E05: Evolucionar fidelidad de un entorno sin romper compatibilidad

**Como** ingeniero de proceso  
**Quiero** reemplazar el modelo de proceso subyacente (de ODEs simples a PC-Gym, de PC-Gym a IDAES+NeqSim) sin cambiar la interfaz del entorno  
**Para** mejorar la fidelidad de la simulación progresivamente sin afectar al Trainer

**Criterios de aceptación:**
- [ ] Puedo intercambiar el `ProcessModel` subyacente sin cambiar `observation_space` ni `action_space`
- [ ] El entorno sigue cumpliendo la validación de interfaz Gymnasium después del cambio
- [ ] Los agentes previamente entrenados pueden evaluarse en el entorno actualizado
- [ ] El registro del entorno refleja el nivel de fidelidad actual

---

### US-E06: Definir restricciones operativas dentro del entorno

**Como** ingeniero de proceso  
**Quiero** definir los rangos operativos válidos del proceso dentro del entorno  
**Para** que la simulación penalice o termine episodios cuando se violan condiciones de operación segura

**Criterios de aceptación:**
- [ ] Puedo definir límites operativos para cada variable de estado (ej: temperatura máxima del reactor)
- [ ] Puedo configurar si la violación de un límite termina el episodio o aplica una penalización
- [ ] Las restricciones se integran con la función de recompensa (penalización por acercarse a límites)
- [ ] Las restricciones son independientes de las del Runtime (estas son para entrenamiento, las del Runtime son para producción)

---

## Diseño de Algoritmos

### Algoritmo: SimulationEnvironment.step()

```
PRECONDICIONES:
- el entorno fue inicializado con reset() previamente
- action es un vector de setpoints dentro del action_space definido
- el ProcessModel está en un estado válido

POSTCONDICIONES:
- retorna (observation, reward, terminated, truncated, info) con tipos correctos
- observation pertenece al observation_space
- reward es un escalar flotante
- el estado interno del ProcessModel avanzó un paso temporal dt

PSEUDOCÓDIGO:
función step(action):
    // 1. Interpretar la acción como setpoints
    setpoints = interpretar_setpoints(action)
    // action[0] = setpoint_temperatura, action[1] = setpoint_flujo, etc.
    
    // 2. Simular respuesta del PID hacia los setpoints
    // El PID es interno al entorno — el agente NO controla el PID
    para cada setpoint en setpoints:
        pid_controller = obtener_pid_para(setpoint.variable)
        error = setpoint.valor - estado_actual[setpoint.variable]
        señal_control = pid_controller.calcular(error)
        // señal_control es la acción física: % apertura válvula, potencia, etc.
        aplicar_señal_control(señal_control)
    
    // 3. Avanzar la dinámica del proceso un paso temporal
    process_model.advance(señales_control, dt)
    
    // 4. Obtener nuevo estado
    nuevo_estado = process_model.get_state()
    observation = construir_observacion(nuevo_estado)
    
    // 5. Calcular recompensa compuesta
    reward = reward_function.calculate(
        state = estado_anterior,
        action = setpoints,
        next_state = nuevo_estado,
        info = {"señales_control": señales_control, "pid_errors": errores_pid}
    )
    
    // 6. Verificar condiciones de terminación
    terminated = verificar_terminacion(nuevo_estado)
    // terminated = True si el proceso alcanzó estado terminal
    // (ej: runaway térmico, concentración fuera de límites críticos)
    
    truncated = verificar_truncamiento()
    // truncated = True si se alcanzó el máximo de pasos del episodio
    
    // 7. Construir info adicional
    info = {
        "setpoints_aplicados": setpoints,
        "señales_control_pid": señales_control,
        "reward_components": reward_function.get_last_components(),
        "constraint_violations": violaciones_detectadas
    }
    
    estado_anterior = nuevo_estado
    paso_actual += 1
    
    retornar (observation, reward, terminated, truncated, info)

CASOS DE PRUEBA:
- Setpoint dentro de rango → paso ejecuta normalmente, PID converge
- Setpoint en límite del action_space → aceptado, PID opera en borde
- Proceso alcanza estado terminal → terminated=True, episodio termina
- Máximo de pasos alcanzado → truncated=True, episodio se corta
- Reward components → info contiene desglose de cada componente
- Estado resultante → observation pertenece al observation_space
```

---

### Algoritmo: reward_function.calculate()

```
PRECONDICIONES:
- state y next_state son vectores de estado válidos
- action es un vector de setpoints válido
- reward_config tiene al menos un componente definido

POSTCONDICIONES:
- retorna un escalar flotante que representa la calidad de la transición
- _last_components almacena el desglose por componente

PSEUDOCÓDIGO:
función calculate(state, action, next_state, info):
    componentes = diccionario vacío
    
    para cada componente en reward_config.components:
        según componente.type:
            caso "minimize":
                // Penalizar valores altos de una variable
                // Ej: minimizar consumo energético
                variable = extraer variable de next_state según componente.name
                valor_componente = -variable / componente.parameters.normalization_factor
            
            caso "maximize":
                // Premiar valores altos de una variable
                // Ej: maximizar conversión del reactor
                variable = extraer variable de next_state según componente.name
                valor_componente = variable / componente.parameters.normalization_factor
            
            caso "target":
                // Premiar cercanía a un valor objetivo
                // Ej: mantener temperatura en 350K
                variable = extraer variable de next_state según componente.name
                error = |variable - componente.target_value|
                valor_componente = -error / componente.parameters.tolerance
            
            caso "penalty":
                // Penalizar violaciones de restricciones
                // Ej: penalizar oscilaciones bruscas
                cambio = |next_state - state|
                si cambio > componente.parameters.threshold:
                    valor_componente = -componente.parameters.penalty_factor * (cambio - threshold)
                sino:
                    valor_componente = 0
        
        componentes[componente.name] = valor_componente * componente.weight
    
    // Agregar componentes según método configurado
    según reward_config.aggregation:
        caso "weighted_sum":
            reward_total = sumar todos los valores en componentes
        caso "product":
            reward_total = multiplicar todos los valores en componentes
        caso "min":
            reward_total = mínimo de los valores en componentes
    
    _last_components = componentes
    retornar reward_total

CASOS DE PRUEBA:
- Componente "minimize" con valor bajo → recompensa alta (cercana a 0)
- Componente "maximize" con valor alto → recompensa alta (positiva)
- Componente "target" en el objetivo → recompensa máxima (0 de penalización)
- Componente "target" lejos del objetivo → recompensa negativa
- Componente "penalty" sin violación → contribución 0
- Componente "penalty" con violación → contribución negativa proporcional
- Weighted sum de múltiples componentes → suma ponderada correcta
- Todos los componentes → get_last_components() retorna desglose
```

---

## Notas de Implementación

- **Interfaz**: `gymnasium>=0.29` para compatibilidad con Gymnasium API v1.0
- **Simulación fase 1**: `pcgym` para entornos de proceso con ODEs (CSTR, intercambiadores)
- **Simulación fase 2**: `pcgym` + `neqsim` para propiedades termodinámicas rigurosas
- **Simulación fase 3**: `idaes-pse` + `pcgym` + `neqsim` para flowsheets completos con múltiples equipos
- **Dependencias base**: gymnasium, numpy, pydantic
- **Dependencias de simulación** (opcionales por fase): pcgym, neqsim, idaes-pse, pyomo
- **Visualización standalone**: matplotlib para gráficos de debug, sin dependencias de Trainer
- **Estructura del paquete**:

```
simplant-environments/
├── src/
│   └── simplant_environments/
│       ├── __init__.py
│       ├── registry.py             # EnvironmentRegistry
│       ├── interfaces/
│       │   ├── __init__.py
│       │   ├── environment.py      # SimulationEnvironment (ABC)
│       │   ├── reward.py           # RewardFunction (ABC)
│       │   ├── process_model.py    # ProcessModel (ABC)
│       │   └── types.py            # EnvironmentConfig, ObservationVariable, etc.
│       ├── rewards/
│       │   ├── __init__.py
│       │   ├── composite.py        # CompositeReward (weighted_sum, product, min)
│       │   ├── energy.py           # EnergyEfficiencyReward
│       │   ├── stability.py        # ProcessStabilityReward
│       │   └── quality.py          # ProductQualityReward
│       ├── processes/
│       │   ├── __init__.py
│       │   ├── cstr.py             # CSTREnvironment
│       │   ├── heat_exchanger.py   # HeatExchangerEnvironment
│       │   └── distillation.py     # DistillationEnvironment
│       ├── models/
│       │   ├── __init__.py
│       │   ├── pcgym_model.py      # Wrapper de PC-Gym
│       │   ├── idaes_model.py      # Wrapper de IDAES-PSE
│       │   └── ode_model.py        # ODEs custom (scipy.integrate)
│       ├── validation/
│       │   ├── __init__.py
│       │   └── gym_checker.py      # Validador de compliance Gymnasium
│       └── debug/
│           ├── __init__.py
│           └── visualizer.py       # Visualización standalone de entornos
├── tests/
│   ├── unit/
│   │   ├── test_rewards.py
│   │   ├── test_registry.py
│   │   └── test_gym_compliance.py
│   └── integration/
│       ├── test_cstr_environment.py
│       └── test_fidelity_swap.py
├── examples/
│   ├── cstr_basic.py               # Ejemplo de CSTR con PC-Gym
│   └── custom_reward.py            # Ejemplo de función de recompensa custom
├── pyproject.toml
└── README.md
```

- **Dependencias**:

```toml
[project]
dependencies = [
    "gymnasium>=0.29",
    "numpy>=1.24",
    "pydantic>=2.0",
]

[project.optional-dependencies]
pcgym = ["pcgym>=0.1"]
neqsim = ["neqsim>=2.5"]
idaes = ["idaes-pse>=2.4", "pyomo>=6.7"]
viz = ["matplotlib>=3.8"]
all = ["pcgym>=0.1", "neqsim>=2.5", "idaes-pse>=2.4", "pyomo>=6.7", "matplotlib>=3.8"]
```

---

# Documentos relacionados

## Módulo Environments
Este es el documento principal del módulo Environments.

## Arquitectura SimPlant - Mapa de Módulos

### 🧠 Pipeline de ML
- [[Trainer]] - Módulo de entrenamiento de agentes RL
  - Consume entornos de este módulo a través de la interfaz `SimulationEnvironment`
  - Los algoritmos (PPO, SAC, TD3) interactúan con los entornos vía `reset()` y `step()`

### 🔌 Despliegue y Runtime
- [[Runtime]] - Módulo de ejecución en producción
  - Los modelos entrenados en estos entornos se exportan a ONNX y se ejecutan en Runtime
  - Las restricciones de entrenamiento (este módulo) son conceptualmente distintas a las de producción (Runtime)

### 🌐 Comunicación
- [[Gateway]] - Interfaz de comunicación con sistemas de control
  - [[Gateway/Conceptual|SimulationAdapter]] genera datos sintéticos similares a lo que estos entornos simulan

### 📄 Arquitectura de referencia
- [[Papers/ARQUITECTURA-SAC-PID|Arquitectura SAC-PID]] - Explica la jerarquía de control: SAC produce setpoints, PID ejecuta
  - Fundamental para entender por qué `action_space` representa setpoints, no acciones de control directas
