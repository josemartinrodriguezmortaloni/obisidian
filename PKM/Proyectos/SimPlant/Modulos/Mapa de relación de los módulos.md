
```
┌───────────────────────────────────────────────────────────────────────────────┐
│ RED IT / NUBE (Nivel 4/5 - Exploración Estocástica y Entrenamiento)           │
│ El objetivo aquí es aprender cometiendo errores virtuales masivos.            │
│                                                                               │
│  ┌────────────────┐        ┌──────────────────┐        ┌──────────────────┐   │
│  │ UI 1: Lab      │◄───────┤ SimPlant Trainer │◄───────┤ Twin Calibration │   │
│  │ (Dashboard 3D, │        │ (Ray/RLlib,      │        │ (Identificación  │   │
│  │ Hiperparámetros)        │  Iteraciones)    │        │  de Sistemas)    │   │
│  └────────────────┘        └────────┬─────────┘        └────────▲─────────┘   │
│                                     │ Interacción               │ Telemetría  │
│                                     ▼                           │ Histórica   │
│                            ┌──────────────────┐                 │             │
│                            │ SimPlant Env     ├─────────────────┘             │
│                            │ (Gymnasium, ODEs)│                               │
│                            └──────────────────┘                               │
└─────────────────────────────────────┬─────────────────────────────────────────┘
                                      │ Exporta Modelo (.onnx)
======================================▼==========================================
[OT SECURITY BOUNDARY] - Data Diode / Validación Criptográfica (El Foso Defensivo)
======================================▼==========================================
┌─────────────────────────────────────┼─────────────────────────────────────────┐
│ RED OT / PLANTA (Nivel 3 - Ejecución Determinista y Supervisión)              │
│ El objetivo aquí es no explotar. Cero estocasticidad permitida.               │
│                                     ▼                                         │
│  ┌────────────────┐        ┌──────────────────┐        ┌──────────────────┐   │
│  │ UI 2: Operator │◄───────┤ SimPlant Runtime │◄───────┤ Signal           │   │
│  │ + Operator     │(A/C)   │ (Python, ONNX,   │(Limpio)│ Conditioning     │   │
│  │ Observability  │        │  Inferencia)     │        │ (Filtros, Limites│   │
│  └───────▲────────┘        └────────┬─────────┘        └────────▲─────────┘   │
│          │                          │ Setpoints Crudos          │ Señales     │
│          │                          ▼                           │ Crudas y    │
│          │                 ┌──────────────────┐                 │ Asíncronas  │
│          └─────────────────┤ Motor de         │                 │             │
│            (Proyección     │ Restricciones    │                 │             │
│             Económica)     │ (Numba/Determin.)│                 │             │
│                            └────────┬─────────┘                 │             │
│                                     │ Setpoints Seguros         │             │
└─────────────────────────────────────┼───────────────────────────┼─────────────┘
                                      ▼                           │
┌─────────────────────────────────────┼───────────────────────────┼─────────────┐
│ EL MUNDO FÍSICO (Nivel 1/2 - Entropía y Termodinámica)          │             │
│                                     │                           │             │
│                            ┌────────▼─────────┐                 │             │
│                            │ SimPlant Gateway ├─────────────────┘             │
│                            │ (OPC-UA/Modbus)  │                               │
│                            └────────┬─────────┘                               │
│                                     │ Traduce red a física                    │
│                                     ▼                                         │
│                            ┌──────────────────┐                               │
│                            │ DCS / PLCs / PIDs│                               │
│                            │ (Válvulas, Bombas│                               │
│                            └──────────────────┘                               │
└───────────────────────────────────────────────────────────────────────────────┘
```
# Anatomía de los Flujos (Cómo leer el mapa)

Si observas la topología aplicando tus propios axiomas, verás tres ciclos de vida distintos operando en paralelo:

1. **El Ciclo de Reacción (Corto Plazo / Segundos):**
    
    `Planta` $\rightarrow$ `Gateway` $\rightarrow$ `Signal Conditioning` $\rightarrow$ `Runtime` $\rightarrow$ `Motor de Restricciones` $\rightarrow$ `Gateway` $\rightarrow$ `Planta`.
    
    Este es el lazo de control estricto. Vive enteramente debajo de la barrera de seguridad. Nunca espera por la nube. Es un conducto cerrado de supervivencia táctica.
    
2. **El Ciclo de Confianza Humana (Medio Plazo / Minutos):**
    
    `Runtime` $\rightarrow$ `Operator Observability` $\rightarrow$ `UI 2: Operator`.
    
    Este es el sistema inmunológico contra el rechazo del usuario. Intercepta la matemática pura del Runtime y la traduce a impacto de negocio para que el humano apruebe el pase de modo _Advisory_ a _Closed-Loop_.
    
3. **El Ciclo de Evolución Cognitiva (Largo Plazo / Semanas):**
    
    `Telemetría OT` $\rightarrow$ `Twin Calibration` $\rightarrow$ `Env` $\rightarrow$ `Trainer` $\rightarrow$ `Nuevo .onnx` $\rightarrow$ `OT Security Boundary` $\rightarrow$ `Runtime`.
    
    Aquí es donde el sistema "aprende". Llevamos la realidad histórica al simulador para calibrar las leyes físicas, el agente de RL descubre una política más óptima para ganar eficiencia energética, y luego inyectamos ese nuevo cerebro hacia abajo a través de la frontera de seguridad.
    
Este modelo ejecutivo respeta la **Ley de Localidad**: si cambias el protocolo OPC-UA por MQTT, solo tocas el Gateway. Si cambias PPO por SAC, solo tocas el Trainer. Si un operador pide ver emisiones de carbono en lugar de consumo eléctrico, solo tocas _Operator Observability_. Nada colapsa.