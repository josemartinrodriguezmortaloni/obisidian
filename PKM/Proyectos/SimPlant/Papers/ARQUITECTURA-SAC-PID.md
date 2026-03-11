# Arquitectura SAC-PID en SimPlant
## Jerarquia de Control: Quien decide que

> **Documento de referencia**: Explicacion de como interactuan SAC y PID en SimPlant

---

## Concepto Central

**El PID NO recibe las "acciones" del SAC directamente.**

El PID recibe **SETPOINTS** (objetivos), no acciones de control.

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   SAC observa estado ──▶ Calcula SETPOINT optimo               │
│                                │                                │
│                                ▼                                │
│                         SP = 72.5°C                             │
│                                │                                │
│                                ▼                                │
│   PID recibe SP ──▶ Calcula ACCION DE CONTROL ──▶ Actuador     │
│                                │                                │
│                                ▼                                │
│                         u = 65% valvula                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## La Diferencia Clave

| Concepto | Que es | Ejemplo |
|----------|--------|---------|
| **Setpoint (SP)** | El OBJETIVO, donde queres llegar | "Quiero 72.5°C" |
| **Accion de control (u)** | COMO llegas ahi | "Abri la valvula al 65%" |

- **SAC calcula el setpoint** → "¿A que temperatura deberia apuntar para minimizar energia?"
- **PID calcula la accion** → "¿Cuanto abro la valvula para llegar a esa temperatura?"

---

## Por que esta Separacion

| Razon | Explicacion |
|-------|-------------|
| **Seguridad** | El PID ya existe, esta probado, tiene limites fisicos configurados |
| **Velocidad** | PID opera en milisegundos, SAC puede pensar mas lento |
| **No reinventas la rueda** | Los PIDs ya saben controlar procesos |

---

## Ejemplo Concreto: Camara Frigorifica

```
Estado actual:
  - Temperatura: 5°C
  - Temperatura externa: 30°C
  - Compresor: ON
  - Hora: 14:00 (pico de calor)

SAC piensa:
  "Si bajo el setpoint a -2°C ahora, el compresor trabaja mas
   pero la noche viene fria... mejor pongo SP = 1°C y dejo
   que el frio nocturno ayude."

SAC output (setpoint): 1°C  ◄── Esto recibe el PID

PID recibe SP=1°C, ve que esta en 5°C:
  error = 1 - 5 = -4°C
  accion = Kp*(-4) + Ki*∫error + Kd*d(error)/dt
  accion = 85% potencia compresor  ◄── Esto va al equipo fisico
```

---

## En Codigo

```python
# SAC decide el setpoint optimo
optimal_setpoint = sac_agent.predict(process_state)  # → 1.0°C

# Se envia al PID via OPC-UA
await setpoint_node.write_value(optimal_setpoint)

# El PID (en el PLC) hace su trabajo internamente:
# error = setpoint - temperatura_actual
# control_output = Kp*error + Ki*integral + Kd*derivative
# → Manda senal al compresor
```

---

## Resumen

> El SAC es el **cerebro estrategico** (que objetivo perseguir).
> El PID es el **ejecutor tactico** (como llegar al objetivo).

---

# En RL, "Accion" es lo que VOS Definis

La "accion" del agente depende de **como disenas el problema**.

En SimPlant, definimos:

```
Accion del SAC = El setpoint que elige
```

**NO es**:

```
Accion del SAC = Senal al actuador (valvula, bomba, etc.)
```

---

## Dos Arquitecturas Posibles

### Opcion A: SAC controla directo (NO es SimPlant)

```
SAC ──▶ Accion = % apertura valvula ──▶ Valvula
        (SAC reemplaza al PID)
```

❌ Peligroso, reinventas la rueda, perdes safety del PID

### Opcion B: SAC como supervisor (SimPlant) ✓

```
SAC ──▶ Accion = setpoint ──▶ PID ──▶ % valvula ──▶ Valvula
        (SAC trabaja CON el PID)
```

✅ Seguro, usas infraestructura existente

---

## Implementacion en Gymnasium

```python
import gymnasium as gym
from gymnasium import spaces
import numpy as np

class SimPlantEnv(gym.Env):
    def __init__(self):
        # El ESTADO es lo que observa el agente
        self.observation_space = spaces.Box(
            low=np.array([0, 0, 0]),      # temp, flow, pressure
            high=np.array([100, 50, 10]),
            dtype=np.float32
        )
        
        # La ACCION es el setpoint que elige (NO el control directo)
        self.action_space = spaces.Box(
            low=np.array([20.0]),    # setpoint minimo: 20°C
            high=np.array([80.0]),   # setpoint maximo: 80°C
            dtype=np.float32
        )
    
    def step(self, action):
        # action = el setpoint que eligio SAC
        setpoint = action[0]  # Por ejemplo: 45.5°C
        
        # Internamente, el PID lleva el proceso al setpoint
        # (esto lo simula el environment o lo hace el PLC real)
        self._pid_controls_to_setpoint(setpoint)
        
        # Calculamos reward (ahorro energetico, estabilidad, etc.)
        reward = self._calculate_reward()
        
        return new_state, reward, done, truncated, info
```

---

## Respuesta Directa

> "¿El SAC genera acciones de como llegamos al setpoint?"

**NO.** El SAC genera el setpoint. El PID se encarga de COMO llegar.

| Quien | Que decide | Ejemplo |
|-------|-----------|---------|
| **SAC** | "¿A donde vamos?" | SP = 45°C |
| **PID** | "¿Como llegamos?" | Valvula al 67% |

---

## Jerarquia de Control

```
┌─────────────────────────────────────────┐
│  NIVEL SUPERIOR (SAC)                   │
│  - Estrategia                           │
│  - Optimizacion                         │
│  - Piensa en minutos/horas              │
├─────────────────────────────────────────┤
│  NIVEL INFERIOR (PID)                   │
│  - Ejecucion                            │
│  - Tactica                              │
│  - Opera en milisegundos                │
└─────────────────────────────────────────┘
```

---

## Diagrama Completo de SimPlant

```
┌─────────────────────────────────────────────────────────────────┐
│                    SIMPLANT - FLUJO DE DATOS                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌─────────────┐                                               │
│   │   PROCESO   │ ◄─────────────────────────────────────┐       │
│   │  (Planta)   │                                       │       │
│   └──────┬──────┘                                       │       │
│          │                                              │       │
│          │ Mediciones (T, P, F, etc.)                   │       │
│          ▼                                              │       │
│   ┌─────────────┐                                       │       │
│   │   OPC-UA    │ Lee sensores                          │       │
│   │  (Lectura)  │                                       │       │
│   └──────┬──────┘                                       │       │
│          │                                              │       │
│          │ Estado del proceso                           │       │
│          ▼                                              │       │
│   ┌─────────────┐                                       │       │
│   │     SAC     │                                       │       │
│   │  (Agente)   │ Calcula setpoint optimo               │       │
│   └──────┬──────┘                                       │       │
│          │                                              │       │
│          │ Setpoint (SP)                                │       │
│          ▼                                              │       │
│   ┌─────────────┐                                       │       │
│   │ RESTRICCION │ Valida seguridad                      │       │
│   │   ENGINE    │ (nunca viola limites)                 │       │
│   └──────┬──────┘                                       │       │
│          │                                              │       │
│          │ Setpoint validado                            │       │
│          ▼                                              │       │
│   ┌─────────────┐                                       │       │
│   │   OPC-UA    │ Escribe setpoint                      │       │
│   │ (Escritura) │                                       │       │
│   └──────┬──────┘                                       │       │
│          │                                              │       │
│          │ SP al controlador                            │       │
│          ▼                                              │       │
│   ┌─────────────┐                                       │       │
│   │     PID     │ Calcula accion de control             │       │
│   │ (En el PLC) │ u = Kp*e + Ki*∫e + Kd*de/dt          │       │
│   └──────┬──────┘                                       │       │
│          │                                              │       │
│          │ Senal de control (u)                         │       │
│          ▼                                              │       │
│   ┌─────────────┐                                       │       │
│   │  ACTUADOR   │ Valvula, bomba, compresor             │       │
│   │             │────────────────────────────────────────┘       │
│   └─────────────┘                                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

# Stack de Librerias para SimPlant

## Combinacion Recomendada: PC-Gym + IDAES-PSE + NeqSim

```
┌─────────────────────────────────────────────────────────────────┐
│                  STACK PROPUESTO (Potente)                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌─────────────┐                                               │
│   │   PC-Gym    │  Wrapper Gymnasium, manejo de RL              │
│   │             │  Estados, acciones, rewards, episodios        │
│   └──────┬──────┘                                               │
│          │                                                      │
│          ▼                                                      │
│   ┌─────────────┐                                               │
│   │  IDAES-PSE  │  Modelos de equipos, flowsheets               │
│   │             │  CSTR, HX, Separadores, Reactores             │
│   └──────┬──────┘                                               │
│          │                                                      │
│          ▼                                                      │
│   ┌─────────────┐                                               │
│   │   NeqSim    │  Termodinamica rigurosa                       │
│   │             │  Propiedades, equilibrios, flash              │
│   └─────────────┘                                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Que Aporta Cada Libreria

| Libreria | Aporta | Sin ella tendrias que... |
|----------|--------|--------------------------|
| **PC-Gym** | Interfaz Gymnasium, rewards, constraints, benchmarks | Escribir todo el wrapper de RL |
| **IDAES-PSE** | Modelos de equipos validados, balances, dinamica | Derivar ecuaciones de cada equipo |
| **NeqSim** | Propiedades termodinamicas reales | Usar correlaciones simplificadas |

---

## Paper de Referencia: RL + IDAES

> **"A coupled reinforcement learning and IDAES process modeling framework for automated conceptual design of energy and chemical systems"**
> - Wang et al., 2023 (Pacific Northwest National Lab)
> - Energy Advances, RSC Publishing
> - DOI: 10.1039/D3YA00310H

**Links**:
- Paper HTML: https://pubs.rsc.org/en/content/articlelanding/2023/ya/d3ya00310h
- Paper PDF: https://pubs.rsc.org/en/content/articlepdf/2023/ya/d3ya00310h

Este paper valida la arquitectura de combinar RL con IDAES para diseno de procesos.

---

## Arquitectura de Integracion

```python
# Ejemplo conceptual de integracion

import gymnasium as gym
from idaes.core import FlowsheetBlock
from idaes.models.unit_models import CSTR, Heater, Mixer
from pyomo.environ import ConcreteModel, SolverFactory
from neqsim.thermo import fluid

class IDEASEnv(gym.Env):
    """
    Environment que usa IDAES para el proceso
    y NeqSim para propiedades termodinamicas
    """
    
    def __init__(self):
        super().__init__()
        
        # Definir espacios (estilo PC-Gym)
        self.observation_space = gym.spaces.Box(
            low=np.array([0, 250, 0]),    # [concentracion, temp, presion]
            high=np.array([1, 400, 10]),
            dtype=np.float32
        )
        self.action_space = gym.spaces.Box(
            low=np.array([280]),   # Setpoint temp minimo
            high=np.array([350]),  # Setpoint temp maximo
            dtype=np.float32
        )
        
        # Crear modelo IDAES
        self._build_idaes_model()
        
        # Inicializar NeqSim para propiedades
        self._init_neqsim()
    
    def _build_idaes_model(self):
        """Construir flowsheet con IDAES"""
        self.model = ConcreteModel()
        self.model.fs = FlowsheetBlock(dynamic=True, time_set=[0, 1])
        
        # Agregar unidades (IDAES tiene modelos listos!)
        self.model.fs.heater = Heater(property_package=self.props)
        self.model.fs.reactor = CSTR(
            property_package=self.props,
            reaction_package=self.rxn_props
        )
        
        # Conectar unidades
        self.model.fs.stream = Arc(
            source=self.model.fs.heater.outlet,
            destination=self.model.fs.reactor.inlet
        )
        
        self.solver = SolverFactory('ipopt')
    
    def _init_neqsim(self):
        """Configurar NeqSim para propiedades"""
        self.neq_fluid = fluid('srk')
        self.neq_fluid.addComponent('ethylene-oxide', 0.3)
        self.neq_fluid.addComponent('water', 0.7)
    
    def step(self, action):
        """
        Ejecutar un paso de simulacion
        action = setpoint de temperatura
        """
        setpoint_temp = action[0]
        
        # Actualizar setpoint en IDAES
        self.model.fs.heater.outlet.temperature.fix(setpoint_temp)
        
        # Resolver el modelo (IDAES + Pyomo)
        result = self.solver.solve(self.model, tee=False)
        
        # Obtener estado resultante
        new_state = self._get_state()
        
        # Calcular reward (eficiencia energetica, conversion, etc)
        reward = self._calculate_reward()
        
        return new_state, reward, done, truncated, info
    
    def _get_state(self):
        """Extraer estado del modelo IDAES"""
        return np.array([
            self.model.fs.reactor.outlet.conc_mol['product'].value,
            self.model.fs.reactor.outlet.temperature.value,
            self.model.fs.reactor.outlet.pressure.value
        ])
```

---

## Ventajas de esta Combinacion

| Aspecto | Beneficio |
|---------|-----------|
| **Velocidad de desarrollo** | IDAES tiene CSTR, HX, Flash, Columnas listos |
| **Validacion** | Modelos de IDAES son de DOE, validados industrialmente |
| **Realismo** | NeqSim da propiedades termodinamicas reales |
| **RL-ready** | PC-Gym ya tiene la estructura de Gymnasium |
| **Optimizacion** | IDAES esta sobre Pyomo → optimizadores industriales |

---

## Complejidad vs Beneficio

```
                    Complejidad
                         │
    IDAES+NeqSim+PC-Gym  ●─────────────────────► Maximo realismo
                         │                        Validacion industrial
                         │                        Equipos complejos
                         │
    PC-Gym + NeqSim      ●─────────────────► Buen balance
                         │                   Termodinamica real
                         │                   Dinamica simplificada
                         │
    PC-Gym solo          ●─────────────────► Rapido de implementar
                         │                   Validar concepto RL
                         │
    ODEs custom          ●─────────────────► Maximo control
                         │                   Mas trabajo
                         │
                    ─────┴─────────────────────────────►
                                              Realismo
```

---

## Enfoque por Fases Recomendado

### Fase 1: Validar concepto (Semanas 1-4)
```
PC-Gym solo → Probar que SAC funciona en CSTR basico
```

### Fase 2: Agregar realismo (Semanas 5-8)
```
PC-Gym + NeqSim → Propiedades termodinamicas reales
```

### Fase 3: Escalar (Semanas 9-12)
```
IDAES + PC-Gym + NeqSim → Flowsheets completos, multiples equipos
```

---

## Instalacion del Stack Completo

```bash
# 1. PC-Gym
pip install pcgym

# 2. IDAES-PSE (requiere Pyomo)
pip install idaes-pse
idaes get-extensions  # Descarga solvers

# 3. NeqSim
pip install neqsim

# Verificar
python -c "import pcgym; print('PC-Gym OK')"
python -c "from idaes.core import FlowsheetBlock; print('IDAES OK')"
python -c "from neqsim.thermo import fluid; print('NeqSim OK')"
```

---

## Links de Documentacion

| Libreria | Documentacion | GitHub |
|----------|---------------|--------|
| **PC-Gym** | https://maximilianb2.github.io/pc-gym/ | https://github.com/MaximilianB2/pc-gym |
| **IDAES-PSE** | https://idaes-pse.readthedocs.io/ | https://github.com/IDAES/idaes-pse |
| **NeqSim** | https://equinor.github.io/neqsimhome/ | https://github.com/equinor/neqsim-python |

---

**Fecha de creacion**: 2025-01-29  
**Ultima actualizacion**: 2025-01-29  
**Proyecto**: SimPlant  
**Categoria**: Arquitectura de Control y Stack Tecnologico
