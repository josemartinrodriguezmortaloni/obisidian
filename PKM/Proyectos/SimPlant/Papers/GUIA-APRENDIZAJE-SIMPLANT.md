# Guia de Aprendizaje - SimPlant
## Reinforcement Learning, SAC, PID y OPC-UA

> **Metodologia**: Primeros principios. Derivacion sobre memorizacion.  
> **Duracion total**: 8 semanas (2-3 horas/dia)  
> **Objetivo**: Dominar los fundamentos para construir SimPlant

---

## Arquitectura Mental del Proyecto

Antes de estudiar, entiende como se conecta todo:

```
┌─────────────────────────────────────────────────────────────────┐
│                    SIMPLANT - CAPAS                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   [TU CEREBRO APRENDE ESTO]                                     │
│                                                                  │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │ CAPA 4: Reinforcement Learning (SAC)                    │   │
│   │ - Observa el estado del proceso                         │   │
│   │ - Calcula el SETPOINT optimo                            │   │
│   │ - Aprende de la experiencia                             │   │
│   └─────────────────────────────────────────────────────────┘   │
│                           │                                      │
│                           ▼ setpoint                             │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │ CAPA 3: Motor de Restricciones                          │   │
│   │ - Valida que el setpoint sea seguro                     │   │
│   │ - Nunca viola limites fisicos                           │   │
│   └─────────────────────────────────────────────────────────┘   │
│                           │                                      │
│                           ▼ setpoint validado                    │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │ CAPA 2: OPC-UA (Comunicacion)                           │   │
│   │ - Lee sensores del PLC/DCS                              │   │
│   │ - Escribe setpoints al controlador                      │   │
│   └─────────────────────────────────────────────────────────┘   │
│                           │                                      │
│                           ▼ comando fisico                       │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │ CAPA 1: Controlador PID (Ya existe en planta)           │   │
│   │ - Recibe setpoint                                       │   │
│   │ - Manipula valvulas/bombas/compresores                  │   │
│   │ - Mantiene la variable en el objetivo                   │   │
│   └─────────────────────────────────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Orden de estudio**: De abajo hacia arriba (PID → OPC-UA → RL → SAC)

---

# FASE 1: CONTROL PID (Semanas 1-2)

## Por que primero

SimPlant NO reemplaza los PIDs. Los DIRIGE. Si no entendes como funciona un PID, no podes diseñar un sistema que los optimice.

## Conceptos Fundamentales (5 max por semana)

### Semana 1: Teoria de Control Basica

| # | Concepto | Pregunta Clave | Nivel |
|---|----------|----------------|-------|
| 1 | **Sistema dinamico** | ¿Que significa que un sistema tenga "dinamica"? | Axiomatico |
| 2 | **Lazo abierto vs cerrado** | ¿Por que necesitamos feedback? | Axiomatico |
| 3 | **Setpoint (SP)** | ¿Que representa el objetivo del control? | Axiomatico |
| 4 | **Variable de proceso (PV)** | ¿Que estamos midiendo? | Axiomatico |
| 5 | **Error (e = SP - PV)** | ¿Por que el error es el corazon de todo control? | Axiomatico |

### Semana 2: El Controlador PID

| # | Concepto | Pregunta Clave | Nivel |
|---|----------|----------------|-------|
| 1 | **Accion Proporcional (P)** | ¿Por que P solo no elimina el error? | Axiomatico |
| 2 | **Accion Integral (I)** | ¿Como I elimina el error en estado estacionario? | Axiomatico |
| 3 | **Accion Derivativa (D)** | ¿Como D anticipa el futuro? | Axiomatico |
| 4 | **Sintonizacion (Kp, Ki, Kd)** | ¿Por que sintonizar mal causa oscilaciones? | Estructural |
| 5 | **Modo Manual vs Automatico** | ¿Por que operadores ponen lazos en manual? (EL PROBLEMA QUE RESOLVES) | Estructural |

## Material de Estudio

### Videos (Ver en orden)

| Orden | Video | Duracion | Link | Objetivo |
|-------|-------|----------|------|----------|
| 1 | Understanding PID Control (Part 1) - MATLAB | 14 min | https://www.youtube.com/watch?v=wkfEZmsQqiA | Intuicion visual |
| 2 | Understanding PID Control (Part 2) - MATLAB | 12 min | https://www.youtube.com/watch?v=NVLXCwc8HzM | Efecto de cada termino |
| 3 | Understanding PID Control (Part 3) - MATLAB | 15 min | https://www.youtube.com/watch?v=7dUVdrs1e18 | Sintonizacion |
| 4 | PID Control - Brian Douglas | 20 min | https://www.youtube.com/watch?v=UR0hOmjaHp0 | Profundizacion matematica |

### Lectura

| Orden | Recurso | Tipo | Capitulos | Tiempo |
|-------|---------|------|-----------|--------|
| 1 | Control Systems Engineering - Nise | Libro | Cap 1-2, 9 | 4h |
| 2 | PID Controllers: Theory, Design and Tuning - Astrom | Libro | Cap 1-3 | 6h |
| 3 | PID Tuning Guide - Emerson | PDF Gratuito | Todo | 2h |

**Link PDF Emerson**: https://www.emerson.com/documents/automation/article-pid-tuning-guide-deltav-en-42362.pdf

### Practica Obligatoria

1. **Simulador Online**: https://pidtuner.com/
   - Jugar con Kp, Ki, Kd y ver efecto en respuesta
   - Tomar screenshots de al menos 3 configuraciones diferentes
   
2. **Python Simulation** (crear archivo `pid_simulation.py`):
```python
import numpy as np
import matplotlib.pyplot as plt

class PIDController:
    def __init__(self, kp, ki, kd, setpoint):
        self.kp = kp
        self.ki = ki
        self.kd = kd
        self.setpoint = setpoint
        self.integral = 0
        self.prev_error = 0
    
    def compute(self, pv, dt):
        error = self.setpoint - pv
        self.integral += error * dt
        derivative = (error - self.prev_error) / dt
        output = self.kp * error + self.ki * self.integral + self.kd * derivative
        self.prev_error = error
        return output

# Simular un tanque de temperatura
# Tu tarea: implementar el proceso y graficar la respuesta
```

## Test de Dominio (Semana 2, Dia 7)

Responder SIN mirar notas:

1. ¿Cual es la ecuacion del controlador PID en el dominio del tiempo?
2. ¿Por que un controlador solo-P tiene "offset" (error permanente)?
3. ¿Que pasa si Ki es muy alto?
4. ¿Que pasa si Kd es muy alto?
5. ¿Por que el 85% de los lazos industriales funcionan mal? (Conectar con SimPlant)

**Criterio de aprobacion**: 4/5 respuestas correctas y derivadas (no memorizadas)

---

# FASE 2: OPC-UA (Semana 3)

## Por que segundo

OPC-UA es tu UNICO canal con el mundo fisico. Sin esto, tu RL es un simulador bonito que no hace nada real.

## Conceptos Fundamentales

| # | Concepto | Pregunta Clave | Nivel |
|---|----------|----------------|-------|
| 1 | **Que es OPC-UA** | ¿Por que se creo este protocolo? ¿Que problema resuelve? | Axiomatico |
| 2 | **Address Space** | ¿Por que todo es un "nodo"? | Axiomatico |
| 3 | **NodeId** | ¿Como identifico una variable especifica en el PLC? | Estructural |
| 4 | **Read/Write** | ¿Como leo sensores y escribo setpoints? | Estructural |
| 5 | **Subscriptions** | ¿Por que no hacer polling constante? | Estructural |

## Material de Estudio

### Videos

| Orden | Video | Duracion | Link | Objetivo |
|-------|-------|----------|------|----------|
| 1 | What is OPC UA? | 8 min | https://www.youtube.com/watch?v=6K8ykHQsLsI | Vision general |
| 2 | OPC UA Information Modeling | 15 min | https://www.youtube.com/watch?v=PfRb4vSklnE | Address Space |
| 3 | OPC UA Security | 12 min | https://www.youtube.com/watch?v=XL_N23s7Yjc | Seguridad industrial |

### Lectura

| Orden | Recurso | Tipo | Seccion | Tiempo |
|-------|---------|------|---------|--------|
| 1 | OPC UA Specification Part 1 | Spec Oficial | Overview | 2h |
| 2 | asyncua Documentation | Docs | Getting Started | 2h |
| 3 | Your Paper: `applsci-15-12904-v2.pdf` | Paper | Revisar uso de OPC | 1h |

**Link Spec**: https://reference.opcfoundation.org/  
**Link asyncua**: https://github.com/freeopcua/opcua-asyncio

### Practica Obligatoria

1. **Instalar servidor de prueba**:
```bash
pip install asyncua
python -m asyncua --populate
```

2. **Crear cliente basico** (`opcua_client.py`):
```python
import asyncio
from asyncua import Client

async def main():
    url = "opc.tcp://localhost:4840/freeopcua/server/"
    
    async with Client(url=url) as client:
        # 1. Obtener el nodo raiz
        root = client.nodes.root
        print(f"Root node: {root}")
        
        # 2. Navegar al espacio de objetos
        objects = client.nodes.objects
        children = await objects.get_children()
        
        print("Hijos de Objects:")
        for child in children:
            name = await child.read_browse_name()
            print(f"  - {name}")
        
        # 3. Leer una variable (ajustar NodeId segun tu servidor)
        # var = client.get_node("ns=2;i=2")
        # value = await var.read_value()
        # print(f"Valor: {value}")
        
        # 4. Escribir un valor
        # await var.write_value(42.0)

asyncio.run(main())
```

3. **Tarea**: Modificar el cliente para:
   - Leer 3 variables diferentes
   - Escribir un valor
   - Crear una subscription que imprima cambios

## Test de Dominio (Semana 3, Dia 7)

1. ¿Que problema resuelve OPC-UA que no resolvian protocolos anteriores (Modbus, etc)?
2. ¿Que es un NodeId y como se estructura?
3. ¿Por que usar subscriptions en lugar de polling para leer sensores?
4. Escribir de memoria el codigo para conectar, leer y escribir un nodo
5. ¿Que consideraciones de seguridad son criticas en un entorno industrial?

---

# FASE 3: REINFORCEMENT LEARNING FUNDAMENTOS (Semanas 4-5)

## Por que tercero

Ahora que entendes el problema (PID) y la comunicacion (OPC-UA), podes aprender la solucion: RL.

## Conceptos Fundamentales

### Semana 4: Fundamentos MDP

| # | Concepto | Pregunta Clave | Nivel |
|---|----------|----------------|-------|
| 1 | **Agente y Entorno** | ¿Quien toma decisiones y quien responde? | Axiomatico |
| 2 | **Estado (s)** | ¿Que informacion necesita el agente para decidir? | Axiomatico |
| 3 | **Accion (a)** | ¿Que puede hacer el agente? | Axiomatico |
| 4 | **Reward (r)** | ¿Como definimos "bueno" y "malo"? | Axiomatico |
| 5 | **MDP (Markov Decision Process)** | ¿Por que el futuro solo depende del presente? | Axiomatico |

### Semana 5: Aprendizaje

| #   | Concepto                        | Pregunta Clave                                   | Nivel                   |            |
| --- | ------------------------------- | ------------------------------------------------ | ----------------------- | ---------- |
| 1   | **Policy π(a                    | s)**                                             | ¿Que es una estrategia? | Axiomatico |
| 2   | **Value Function V(s)**         | ¿Como evaluamos estados?                         | Axiomatico              |            |
| 3   | **Q-Function Q(s,a)**           | ¿Como  evaluamos acciones en estados?            | Axiomatico              |            |
| 4   | **Bellman Equation**            | ¿Como se relacionan valores presentes y futuros? | Axiomatico              |            |
| 5   | **Exploration vs Exploitation** | ¿Probar cosas nuevas o usar lo que funciona?     | Estructural             |            |

## Material de Estudio

### Videos (OBLIGATORIOS)

| Orden | Video | Duracion | Link | Objetivo |
|-------|-------|----------|------|----------|
| 1 | David Silver Lecture 1 | 80 min | https://www.youtube.com/watch?v=2pWv7GOvuf0 | Introduccion RL |
| 2 | David Silver Lecture 2 | 90 min | https://www.youtube.com/watch?v=lfHX2hHRMVQ | MDPs |
| 3 | David Silver Lecture 3 | 80 min | https://www.youtube.com/watch?v=Nd1-UUMVfz4 | Dynamic Programming |
| 4 | David Silver Lecture 4 | 80 min | https://www.youtube.com/watch?v=PnHCvfgC_ZA | Model-Free Prediction |
| 5 | David Silver Lecture 5 | 80 min | https://www.youtube.com/watch?v=0g4j2k_Ggc4 | Model-Free Control |

### Lectura

| Orden | Recurso                                                                              | Tipo         | Capitulos | Tiempo |
| ----- | ------------------------------------------------------------------------------------ | ------------ | --------- | ------ |
| 1     | **Sutton & Barto "RL: An Introduction"**                                             | Libro GRATIS | Cap 1-6   | 15h    |
| 2     | Tu paper: `Reinforcement+Learning+in+Process+Industries_+Review+and+Perspective.pdf` | Paper        | Todo      | 3h     |
| 3     | "Deep RL that Matters" - Henderson 2018                                              | Paper        | Todo      | 2h     |

**Link Sutton & Barto**: http://incompleteideas.net/book/the-book-2nd.html (GRATIS)

### Practica Obligatoria

1. **Instalar dependencias**:
```bash
pip install gymnasium stable-baselines3 numpy matplotlib
```

2. **Tu primer agente** (`first_rl_agent.py`):
```python
import gymnasium as gym
from stable_baselines3 import PPO

# 1. Crear entorno
env = gym.make("CartPole-v1")

# 2. Crear agente
model = PPO("MlpPolicy", env, verbose=1)

# 3. Entrenar
model.learn(total_timesteps=10000)

# 4. Evaluar
obs, info = env.reset()
for _ in range(1000):
    action, _states = model.predict(obs, deterministic=True)
    obs, reward, terminated, truncated, info = env.step(action)
    env.render()
    if terminated or truncated:
        obs, info = env.reset()

env.close()
```

3. **Tarea**: Modificar para:
   - Probar con entorno "Pendulum-v1" (continuo, como control industrial)
   - Graficar la curva de aprendizaje (reward vs episodios)
   - Experimentar con diferentes hiperparametros

## Test de Dominio (Semana 5, Dia 7)

1. Definir MDP formalmente: (S, A, P, R, γ) - ¿que es cada elemento?
2. Escribir la ecuacion de Bellman para V(s)
3. ¿Cual es la diferencia entre on-policy y off-policy?
4. ¿Por que el discount factor γ es importante?
5. En SimPlant: ¿que seria el estado, la accion y el reward?

---

# FASE 4: SAC - SOFT ACTOR-CRITIC (Semanas 6-7)

## Por que cuarto

SAC es el algoritmo que vas a usar. Es off-policy (eficiente en samples), bueno para acciones continuas (setpoints), y robusto.

## Conceptos Fundamentales

### Semana 6: Actor-Critic y Entropy

| # | Concepto | Pregunta Clave | Nivel |
|---|----------|----------------|-------|
| 1 | **Actor-Critic Architecture** | ¿Por que separar quien propone y quien evalua? | Axiomatico |
| 2 | **Policy Gradient** | ¿Como optimizamos directamente la politica? | Axiomatico |
| 3 | **Entropy en RL** | ¿Por que queremos "incertidumbre" en la politica? | Axiomatico |
| 4 | **Maximum Entropy RL** | ¿Como balancea SAC reward y exploracion? | Axiomatico |
| 5 | **Temperature Parameter (α)** | ¿Como controlamos exploration vs exploitation? | Estructural |

### Semana 7: Implementacion SAC

| # | Concepto | Pregunta Clave | Nivel |
|---|----------|----------------|-------|
| 1 | **Twin Q-Networks** | ¿Por que dos criticos? | Estructural |
| 2 | **Target Networks** | ¿Por que necesitamos redes "lentas"? | Estructural |
| 3 | **Replay Buffer** | ¿Por que guardar experiencia pasada? | Estructural |
| 4 | **Automatic α Tuning** | ¿Como SAC ajusta la temperatura solo? | Estructural |
| 5 | **SAC vs PPO** | ¿Cuando usar cada uno? | Estructural |

## Material de Estudio

### Papers (OBLIGATORIOS)

| Orden | Paper | Link | Tiempo | Notas |
|-------|-------|------|--------|-------|
| 1 | **SAC Original (Haarnoja 2018)** | https://arxiv.org/abs/1801.01290 | 4h | Paper fundacional |
| 2 | **SAC v2 con Auto-α (Haarnoja 2019)** | https://arxiv.org/abs/1812.05905 | 3h | Mejoras importantes |
| 3 | Tu paper: `2505.03356v1.pdf` | Local | 2h | Revisar aplicacion |

### Videos

| Orden | Video | Duracion | Link | Objetivo |
|-------|-------|----------|------|----------|
| 1 | SAC Explained - Yannic Kilcher | 40 min | https://www.youtube.com/watch?v=ioidsRlf79o | Intuicion |
| 2 | Soft Actor-Critic - Pieter Abbeel | 60 min | https://www.youtube.com/watch?v=xvRrgxcpaHY | Derivacion matematica |

### Documentacion

| Recurso | Link | Uso |
|---------|------|-----|
| Ray RLlib SAC | https://docs.ray.io/en/master/rllib/rllib-algorithms.html#sac | Implementacion |
| Stable Baselines3 SAC | https://stable-baselines3.readthedocs.io/en/master/modules/sac.html | Alternativa |

### Practica Obligatoria

1. **SAC con Stable Baselines3** (`sac_training.py`):
```python
import gymnasium as gym
from stable_baselines3 import SAC
from stable_baselines3.common.callbacks import EvalCallback
import matplotlib.pyplot as plt

# Entorno continuo (como control industrial)
env = gym.make("Pendulum-v1")

# Configurar SAC
model = SAC(
    "MlpPolicy",
    env,
    learning_rate=3e-4,
    buffer_size=1_000_000,
    batch_size=256,
    tau=0.005,          # Soft update coefficient
    gamma=0.99,         # Discount factor
    ent_coef="auto",    # Automatic entropy tuning
    verbose=1,
    tensorboard_log="./sac_tensorboard/"
)

# Entrenar
model.learn(total_timesteps=50_000)

# Guardar modelo
model.save("sac_pendulum")

# Evaluar
eval_env = gym.make("Pendulum-v1", render_mode="human")
obs, info = eval_env.reset()
total_reward = 0

for _ in range(500):
    action, _states = model.predict(obs, deterministic=True)
    obs, reward, terminated, truncated, info = eval_env.step(action)
    total_reward += reward
    if terminated or truncated:
        break

print(f"Total reward: {total_reward}")
eval_env.close()
```

2. **SAC con Ray RLlib** (`sac_ray.py`):
```python
from ray.rllib.algorithms.sac import SACConfig
import ray

ray.init()

config = (
    SACConfig()
    .environment("Pendulum-v1")
    .env_runners(num_env_runners=2)
    .training(
        gamma=0.99,
        actor_lr=3e-4,
        critic_lr=3e-4,
        tau=0.005,
        train_batch_size_per_learner=256,
        replay_buffer_config={
            "type": "PrioritizedEpisodeReplayBuffer",
            "capacity": 1_000_000,
        }
    )
)

algo = config.build()

for i in range(100):
    result = algo.train()
    print(f"Iteration {i}: reward = {result['env_runners']['episode_reward_mean']}")

algo.stop()
ray.shutdown()
```

3. **Tarea**: Crear un simulador simple de proceso termico y entrenarlo con SAC

## Test de Dominio (Semana 7, Dia 7)

1. ¿Cual es el objetivo de optimizacion de SAC (con entropia)?
2. ¿Por que SAC usa dos Q-networks (twin critics)?
3. ¿Que problema resuelve el automatic temperature tuning?
4. ¿Por que SAC es mejor que PPO para control de procesos?
5. Escribir el codigo para configurar SAC para un proceso industrial

---

# FASE 5: INTEGRACION SIMPLANT (Semana 8)

## Objetivo

Conectar todo lo aprendido en un prototipo funcional.

## Proyecto Final

Crear un simulador de proceso simple con:
1. **Proceso**: Tanque con calentador (control de temperatura)
2. **PID**: Controlador que mantiene temperatura
3. **RL (SAC)**: Calcula setpoint optimo para minimizar energia
4. **OPC-UA**: (Simulado) Lee temperatura, escribe setpoint

### Estructura del Proyecto

```
simplant_prototype/
├── process/
│   └── tank_simulator.py      # Simulador del tanque
├── control/
│   ├── pid_controller.py      # Controlador PID
│   └── sac_supervisor.py      # Agente SAC
├── communication/
│   └── opcua_interface.py     # Cliente OPC-UA
├── training/
│   └── train_sac.py           # Script de entrenamiento
└── main.py                    # Integracion
```

### Entregables

1. [ ] Simulador de tanque funcionando
2. [ ] PID controlando temperatura
3. [ ] Agente SAC entrenado que reduce consumo energetico vs setpoint fijo
4. [ ] Grafico comparativo: energia con setpoint fijo vs SAC
5. [ ] Documentacion de decisiones de diseño

---

# PAPERS EN TU CARPETA - GUIA DE LECTURA

## Orden de lectura recomendado

| Orden | Paper | Cuando leerlo | Objetivo |
|-------|-------|---------------|----------|
| 1 | `Reinforcement+Learning+in+Process+Industries_+Review+and+Perspective.pdf` | Semana 5 | Context de RL en industria |
| 2 | `applsci-15-12904-v2.pdf` | Semana 3 | Ver uso de OPC-UA |
| 3 | `2505.03356v1.pdf` | Semana 7 | Aplicacion avanzada |
| 4 | `1-s2.0-S0920410521002400-main.pdf` | Semana 7 | Case study |
| 5 | `FAA98D7C8B8048E09247D434924E1545.pdf` | Semana 8 | Referencia adicional |

---

# RECURSOS ADICIONALES

## Libros (Orden de prioridad)

| # | Libro | Autor | Para que |
|---|-------|-------|----------|
| 1 | Reinforcement Learning: An Introduction | Sutton & Barto | Fundamentos RL (GRATIS online) |
| 2 | Feedback Control of Dynamic Systems | Franklin, Powell | Teoria de control |
| 3 | Deep Reinforcement Learning Hands-On | Lapan | Implementacion practica |

## Cursos Online (Gratuitos)

| Curso | Plataforma | Link |
|-------|------------|------|
| Deep RL Bootcamp | Berkeley | https://sites.google.com/view/deep-rl-bootcamp/lectures |
| Spinning Up in Deep RL | OpenAI | https://spinningup.openai.com/ |
| Control Bootcamp | Steve Brunton | https://www.youtube.com/playlist?list=PLMrJAkhIeNNR20Mz-VpzgfQs5zrYi085m |

## Tools

| Tool | Uso | Link |
|------|-----|------|
| Gymnasium | Entornos RL | https://gymnasium.farama.org/ |
| Stable-Baselines3 | Algoritmos RL | https://stable-baselines3.readthedocs.io/ |
| Ray RLlib | RL escalable | https://docs.ray.io/en/master/rllib/ |
| asyncua | OPC-UA Python | https://github.com/freeopcua/opcua-asyncio |

---

# CHECKLIST SEMANAL

## Semana 1-2: PID
- [ ] Ver 4 videos de MATLAB
- [ ] Leer capitulos de Nise
- [ ] Completar practica con simulador
- [ ] Pasar test de dominio (4/5)

## Semana 3: OPC-UA
- [ ] Ver 3 videos introductorios
- [ ] Leer documentacion asyncua
- [ ] Crear cliente funcional
- [ ] Pasar test de dominio (4/5)

## Semana 4-5: RL Fundamentos
- [ ] Ver 5 lectures de David Silver
- [ ] Leer Sutton & Barto Cap 1-6
- [ ] Leer paper de RL en industria
- [ ] Entrenar primer agente
- [ ] Pasar test de dominio (4/5)

## Semana 6-7: SAC
- [ ] Leer paper SAC original
- [ ] Ver videos explicativos
- [ ] Entrenar SAC en Pendulum
- [ ] Experimentar con hiperparametros
- [ ] Pasar test de dominio (4/5)

## Semana 8: Integracion
- [ ] Crear prototipo SimPlant
- [ ] Demostrar ahorro energetico vs baseline
- [ ] Documentar aprendizajes

---

# NOTAS FINALES

## Principios del Metodo Socrates

1. **Maximo 5 conceptos nuevos por dia** - Respeta tu cerebro
2. **Descanso de 20 min entre conceptos** - Consolidacion
3. **Derivacion sobre memorizacion** - Si no podes explicarlo, no lo entendes
4. **Repeticion espaciada** - Revisar en dia 1, 3, 7, 14, 30

## Preguntas de Autoevaluacion

Antes de avanzar a la siguiente fase, debes poder responder:

- ¿Puedo explicar este concepto sin mirar notas?
- ¿Puedo predecir que pasaria si cambio X?
- ¿Puedo aplicar esto en un contexto diferente?
- ¿Puedo enseñarselo a alguien mas?

Si la respuesta a cualquiera es NO, volver a estudiar antes de avanzar.

---

**Fecha de creacion**: 2025-01-29  
**Proyecto**: SimPlant  
**Duracion estimada**: 8 semanas  
**Objetivo**: Construir fundamentos solidos para desarrollo de SimPlant
