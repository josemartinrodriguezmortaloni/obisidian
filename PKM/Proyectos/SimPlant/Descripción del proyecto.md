# SimPlant

## Qué es

Una capa de **Control Supervisorio** que usa **Aprendizaje por Refuerzo (RL)** para calcular setpoints óptimos en procesos industriales. No manipula equipos físicamente: ingiere el estado del proceso, calcula matemáticamente los objetivos óptimos, los valida contra restricciones de seguridad, y los envía a los controladores PID existentes.

```
┌─────────────────────────────────────────────────────────────┐
│                    SimPlant (Supervisorio)                  │
│  ┌──────────────┐   ┌───────────────┐   ┌───────────────┐  │
│  │  Ingesta de  │──▶│  Agente RL    │──▶│  Motor de     │  │
│  │  Estado      │   │  (Optimiza)   │   │  Restricciones│  │
│  └──────────────┘   └───────────────┘   └───────┬───────┘  │
└─────────────────────────────────────────────────┼──────────┘
                                                  │ Setpoints validados
                                                  ▼
                              ┌────────────────────────────┐
                              │   Controladores PID        │
                              │   (Ejecutan acción física) │
                              └────────────────────────────┘
```

**Principio clave**: El motor de restricciones es **determinista**. Ningún setpoint llega a los PIDs sin pasar validación matemática de límites operativos. Esto garantiza seguridad independientemente de lo que sugiera el modelo de RL.

---

## El Problema

### Evidencia documentada

| Fuente | Hallazgo |
|--------|----------|
| [AIChE, 2020](https://www.aiche.org/resources/publications/cep/2020/may/troubleshooting-process-control-problems) | "Controllers continuously operated in **manual mode** are a key indicator of a problem—operators lose patience with poorly functioning controllers" |
| [Control Engineering, 2024](https://www.controleng.com/enhancing-processes-and-improving-operations-with-pid-loop-monitoring/) | "Properly tuned control loops can **reduce energy consumption by 5 to 15%** and increase throughput" |
| [ePackaging, 2024](https://epackagingsw.com/blog/auto-mode-or-bust-why-manual-settings-are-killing-your-process-control) | "Running in manual mode prevents systems from optimizing in real-time...operators overcompensate, **wasting energy**" |

### El ciclo vicioso

```
PIDs mal sintonizados → Operadores pierden confianza → Pasan a modo manual
       ↑                                                        │
       └────────── Ajustes reactivos y conservadores ◄──────────┘
```

**Consecuencias cuantificables**:
- 5-15% de consumo energético recuperable
- 2-8% de pérdida de producto por variabilidad
- Costos ocultos de $100K-$1M/año en plantas medianas-grandes

---

## La Solución

SimPlant transforma la operación de **reactiva-humana** a **proactiva-matemática**:

| Dimensión | Sin SimPlant | Con SimPlant |
|-----------|--------------|--------------|
| **Decisión de setpoints** | Operador ajusta cuando ve problema | RL calcula óptimo continuamente |
| **Adaptación a cambios** | Re-sintonización manual (meses) | Modelo se adapta automáticamente |
| **Garantía de seguridad** | Depende del criterio humano | Motor de restricciones determinista |
| **Conocimiento** | Se pierde con rotación de personal | Codificado en el modelo |

### Beneficios esperados (conservadores)

| Métrica | Rango | Fuente |
|---------|-------|--------|
| Reducción consumo energético | 5-15% | Control Engineering, 2024 |
| Reducción de scrap/variabilidad | 2-8% | Estimación basada en literatura APC |
| Reducción intervenciones manuales | 40-60% | Objetivo de diseño |

---

## Arquitectura Técnica

### Componentes principales

| Componente | Tecnología | Función |
|------------|------------|---------|
| **Agente RL** | Ray/RLlib (PPO/SAC) | Aprende política óptima de setpoints |
| **Constraint Engine** | Python + Numba | Valida 100% de setpoints contra límites |
| **Conector Industrial** | OPC-UA (asyncua) | Lee estado, escribe setpoints a DCS/PLC |
| **Edge Runtime** | Docker + Linux | Inferencia <100ms en planta |
| **Dashboard** | React + D3.js | Visualización para operadores |

### Principios de diseño

1. **Agnóstico a vendor**: Funciona con cualquier DCS/PLC que soporte OPC-UA, Modbus o MQTT
2. **Safety-first**: Constraint engine nunca se bypasea; es código determinista, no ML
3. **Deployment en semanas**: No requiere modelado first-principles; aprende de datos históricos
4. **Modo advisory primero**: Sugiere antes de actuar; closed-loop solo con aprobación

---

## Mercado Objetivo

### Segmentos prioritarios (validados)

| Industria | Tamaño mercado | Dolor principal | Disposición a pagar |
|-----------|----------------|-----------------|---------------------|
| **Química/Petroquímica** | ~25,000 plantas globales | Consumo energético, variabilidad de producto | $100K-$500K/año |
| **Papel y celulosa** | ~10,000 plantas | Eficiencia de calderas, calidad de pulpa | $50K-$200K/año |
| **Alimentos y bebidas** | ~30,000 plantas | Consistencia, eficiencia térmica | $30K-$150K/año |
| **Farmacéutica** | ~5,000 plantas | Cumplimiento de especificaciones, trazabilidad | $100K-$300K/año |

### Por qué NO empezar con:
- **Cámaras frigoríficas pequeñas**: Bajo ticket, mercado fragmentado
- **Invernaderos**: Ciclos largos, difícil demostrar ROI rápido
- **HVAC residencial/comercial**: Mercado saturado, márgenes bajos

---

## Competencia y Diferenciación

| Competidor | Precio | Debilidad | Oportunidad SimPlant |
|------------|--------|-----------|---------------------|
| AspenTech (DMC3) | $200K-$1M+ | Requiere PhD, proyectos de 6-12 meses | Deploy en semanas |
| Honeywell Profit Suite | $150K-$500K | Lock-in a DCS propios | Agnóstico a vendor |
| AVEVA | $50K-$300K/año | Solo analytics, no actúa | Closed-loop automático |
| Consultores APC | $500K+ por proyecto | Modelos caducan, requieren re-tuning | RL adapta continuamente |

**Posicionamiento**: *"La única solución de control supervisorio que optimiza setpoints con RL adaptativo mientras garantiza 100% cumplimiento de restricciones operativas, sin requerir modelado first-principles."*

---

## Validación de Mercado

### Evidencia de demanda

| Señal | Dato | Fuente |
|-------|------|--------|
| Tamaño de mercado | $3.8B (2024) → $113.1B (2034), CAGR 40.4% | [kyp.ai, 2026](https://kyp.ai/process-optimization-software-market-landscape/) |
| Tracción académica | 50+ papers de RL para control industrial en 2023-2024 | IEEE, ScienceDirect |
| Gap de mercado | APC tradicional es caro y lento; mid-market desatendido | Análisis competitivo |
| Presión regulatoria | ESG obliga eficiencia energética | Tendencia global |

### Viabilidad técnica

| Evidencia | Referencia |
|-----------|------------|
| RL para control de flatness en acero | [ScienceDirect, 2022](https://www.sciencedirect.com/science/article/pii/S0166361522001452) |
| RL para control en semiconductores | [Taylor & Francis, 2024](https://www.tandfonline.com/doi/full/10.1080/24725854.2023.2219290) |
| PID tuning con Deep RL | [arXiv, 2022](https://arxiv.org/pdf/2210.13906) |

---

## Riesgos y Mitigaciones

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Plantas no quieren ceder control (closed-loop) | Alta | Alto | Modo advisory extenso; demostrar ROI antes de pedir autonomía |
| Ciclos de venta largos (6-12 meses) | Alta | Medio | Pilotos pagados, no gratuitos; calificar leads agresivamente |
| Competidor incumbente copia | Media | Medio | Velocidad de ejecución; especializarse en nichos |
| Falla de modelo causa incidente | Baja | Crítico | Constraint engine determinista; nunca se bypasea |

---

## Próximos Pasos

Ver documentos relacionados:
- [[Guia-de-Accion]] - Roadmap detallado fase por fase
- [[Validacion-Mercado]] - Análisis de demanda con evidencia
- [[Analisis-Viabilidad-SimPlant.tex]] - Documento completo en LaTeX

**Primera acción**: Identificar 20 plantas target y enviar mensajes de prospección ofreciendo "diagnóstico de ineficiencia energética gratuito".