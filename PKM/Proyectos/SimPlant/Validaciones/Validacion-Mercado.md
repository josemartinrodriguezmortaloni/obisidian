# SimPlant - Validacion de Mercado
**Fecha**: 2026-01-29  
**Version**: 2.0 (Actualizada con evidencia real de mercado)
**Veredicto Actualizado**: EJECUTAR CON ESTRATEGIA ALTERNATIVA

---

## Cambios vs. Analisis Anterior

| Aspecto            | Analisis                   |                                            |
| ------------------ | -------------------------- | ------------------------------------------ |
| Veredicto          | EJECUTAR (con pivot)       | Evidencia de casos reales de RL industrial |
| Viabilidad tecnica | MVP 8-12 semanas           | Stack maduro, referencias open source      |
| Acceso mercado     | Viable via SMB + servicios | Nicho SMB desatendido por Imubit           |
| Score              | 7.9/10                     | Nuevo enfoque estrategico                  |

---

## FASE 1: FILTRO DE VIABILIDAD

### 1.1 Descomposicion en primeros principios

| Elemento | Analisis |
|----------|----------|
| **Problema raiz** | Los controladores PID industriales operan sub-optimamente (85% mal sintonizados segun ABB/Emerson). La sintonizacion manual es reactiva, costosa y se degrada con el tiempo. Resultado: desperdicio energetico 15-30%, variabilidad de producto, intervenciones humanas constantes. |
| **Mecanismo de valor** | RL aprende la dinamica real del proceso y calcula setpoints optimos en tiempo real. A diferencia de APC tradicional (modelos lineales estaticos), RL se adapta continuamente sin requerir re-modelado manual. Valor: optimizacion continua autonoma. |
| **Modelo de captura** | Gain-sharing (% del ahorro demostrado) + licencia mensual por unidad controlada. ROI demostrable en 30-90 dias permite justificacion rapida. |

### 1.2 Test de viabilidad rapido

| Criterio | Respuesta | Evidencia/Razon |
|----------|-----------|-----------------|
| Problema real y doloroso? | **Si** | Documentado por ABB, Emerson, Honeywell. "Tuning problems are the most common cause for poorly performing control loops" (Industrial & Engineering Chemistry Research, 2018) |
| Solucion tecnicamente posible? | **Si** | Yokogawa/ENEOS: primer despliegue comercial de RL en planta quimica real (2023). CDU 190k bbl: 84% reduccion intervencion operador con RL (SSRN, 2024) |
| Mercado suficientemente grande? | **Si** | Industrial AI Software: $20B en 2025, CAGR 17.9% (Mordor Intelligence). AI Process Optimization: $3.8B en 2024 -> $113B en 2034 (Market.us) |
| Timing adecuado (por que ahora)? | **Si** | (1) Escasez critica de ingenieros APC, (2) Presion ESG por eficiencia energetica, (3) Madurez de RL para control industrial demostrada 2023-2024, (4) APC tradicional mostrando limitaciones |
| Ruta a monetizacion clara? | **Si** | Imubit cobra $1000/usuario/mes (Capterra). Modelo gain-sharing validado: $0.25/bbl ahorro demostrable. ROI 3-6 meses tipico en industria |

**VEREDICTO PRELIMINAR**: **VIABLE**

---

## FASE 2: VALIDACION DE DEMANDA

### 2.1 Evidencia de mercado

| # | Fuente | Queja/Necesidad exacta | Intensidad (1-5) | Frecuencia | Link |
|---|--------|------------------------|------------------|------------|------|
| 1 | PLCTalk Forum | "Skepticism about AI for process optimization - hallucinations near industrial processes are concerning" | 4 | Alta | [PLCTalk](https://www.plctalk.net/threads/ot-ai-process-optimisation.140980/) |
| 2 | Control Engineering | "APC performance degrades within 6-12 months without proper maintenance, often due to lack of in-house expertise" | 5 | Muy Alta | [ControlEng](https://www.electricneutron.com/challenges-in-advanced-process-control-apc/) |
| 3 | Imubit Blog | "APC vendors breaking into top 10 of overall market - shortage of APC engineers driving demand for autonomous solutions" | 5 | Alta | [Imubit](https://imubit.com/blog/the-evolution-of-process-optimization-and-control-technology-ai-drives-a-new-market-category-with-imubit-as-leader/) |
| 4 | Reuters | "44% of manufacturing respondents concerned about AI accuracy/hallucinations - slowing GenAI rollout" | 4 | Alta | [Reuters](https://www.reuters.com/technology/artificial-intelligence/manufacturers-slow-gen-ai-rollout-rising-accuracy-concerns-says-study-2024-07-10/) |
| 5 | ScienceDirect | "MPC performance often degrades after commissioning, leading operators to eventually shut them off" | 5 | Muy Alta | [ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S096706619900194X) |
| 6 | Control Global | "Sustained model accuracy is often impossible - process responses change frequently" | 4 | Alta | [ControlGlobal](https://www.controlglobal.com/home/article/11306312/reimagine-the-advanced-process-control-paradigm) |
| 7 | Manufacturing Today | "Pilot-to-production gap, siloed systems, OT/IT disconnect, talent gaps cause AI failures" | 4 | Alta | [ManufacturingToday](https://manufacturing-today.com/news/why-manufacturing-ai-fails-and-how-to-fix-it/) |

**Minimo 5 evidencias**: 7 encontradas - senal fuerte de demanda.

### 2.2 Tamano de mercado (bottom-up)

**Usuarios potenciales identificables:**
- Refinerias globales: ~650 (con >100k bbl/dia capacidad)
- Plantas quimicas grandes: ~15,000 globalmente
- Plantas de proceso medianas (alimentos, farma, pulp&paper): ~200,000

**Disposicion a pagar (evidencia):**
- Imubit: $1,000/usuario/mes (Capterra 2025)
- APC tradicional: $50k-$500k por proyecto + mantenimiento anual
- ROI tipico demandado: 6-12 meses payback

**TAM/SAM/SOM:**
| Segmento | Calculo | Valor |
|----------|---------|-------|
| **TAM** | Industrial AI Analytics $3.2B (2025) -> $9.3B (2031) segun Verdantix | **$3.2B** |
| **SAM** | Closed-Loop AI Optimization (AIO) - nuevo segmento emergente ~15% del TAM | **$480M** |
| **SOM Ano 1** | 5-10 plantas piloto x $50k-150k ARR promedio | **$500k-$1.5M** |

### 2.3 Conclusion de demanda

**Nivel**: **ALTA**

**Justificacion**: 
1. Dolor documentado y recurrente (degradacion APC, falta expertise)
2. Casos de exito reales con RL (Yokogawa/ENEOS 40% ahorro, CDU 84% menos intervencion)
3. Mercado en crecimiento acelerado (17-22% CAGR)
4. Competidores emergentes recaudando capital (Imubit, etc.)

---

## FASE 3: ANALISIS COMPETITIVO

### 3.1 Mapeo del landscape

| Competidor | Que hace | Precio estimado | Fortaleza | Debilidad explotable |
|------------|----------|-----------------|-----------|---------------------|
| **Imubit** | RL closed-loop optimization para refinerias | $1000/user/mes | Lider en RL industrial, casos de exito en refinerias | Enfocado solo en O&G/refinerias, pricing enterprise |
| **AspenTech** | APC tradicional (DMC3, GDOT) + AI overlay | $100k-$500k/proyecto | Incumbente, integracion completa, base instalada | Tecnologia legacy, requiere expertos, alto costo |
| **Yokogawa FKDPP** | RL autonomo para control de procesos | Enterprise (no publicado) | Primer despliegue comercial real (ENEOS) | Japones, lento go-to-market global |
| **Honeywell** | Profit Suite, Experion + AI add-ons | $200k-$1M/site | Integracion HW/SW, soporte global | Lock-in a ecosistema Honeywell |
| **C3.ai** | Plataforma AI generalista + modulos industriales | $250k+/ano | Plataforma flexible, brand | No especializado en control, mas BI que control |
| **Startups (Falkonry, Canvass)** | AI/ML para manufactura | SaaS variable | Agiles, modernos | Menos maduros, sin track record en control |

### 3.2 Analisis de insatisfaccion

| Competidor | Queja comun | Frecuencia | Oportunidad |
|------------|-------------|------------|-------------|
| AspenTech/APC | "Performance degrades in 6-12 months" | Muy alta | Auto-adaptacion sin re-modelado |
| APC tradicional | "Operators distrust, bypass, disable systems" | Alta | UX transparente, explicabilidad |
| Grandes vendors | "Requires expensive consultants for maintenance" | Alta | Self-service, bajo mantenimiento |
| GenAI tools | "Hallucinations near industrial processes" | Media-Alta | Motor deterministico de restricciones |
| Todos | "Pilot-to-production gap, systems don't scale" | Alta | Arquitectura modular, deployment rapido |

### 3.3 Posicionamiento estrategico

**Gap de mercado identificado:**
- APC legacy: potente pero rigido, caro, requiere expertos
- AI/ML startups: flexibles pero no especializados en control
- **Oportunidad**: RL adaptativo + motor de restricciones deterministico + bajo costo operativo
- **Nicho SMB**: Plantas medianas que no pueden pagar Imubit ($1000/user/mes) ni AspenTech ($100k+)

**Angulo diferenciador:**
> "La unica solucion que combina optimizacion RL adaptativa con validacion deterministica de seguridad, accesible para plantas medianas sin presupuesto enterprise."

**Defensibilidad:**
| Factor | Nivel | Justificacion |
|--------|-------|---------------|
| Network effects | Bajo | No aplica directamente |
| Data moat | Medio | Datos de proceso son del cliente |
| Expertise/IP | **Alto** | Algoritmos RL + motor restricciones + know-how industrial |
| Speed to market | **Alto** | First-mover en segmento SMB industrial |
| Switching costs | Medio | Integracion con DCS/SCADA crea inercia |

---

## FASE 4: EQUIPO MINIMO VIABLE

### 4.1 Principio fundamental

*El equipo minimo es el menor numero de personas con las habilidades necesarias para llevar el producto al mercado y validar traccion inicial.*

### 4.2 Roles requeridos

| Rol | Responsabilidades clave | Perfil ideal | Critico MVP? | Alternativa |
|-----|------------------------|--------------|--------------|-------------|
| **Tech Lead / ML Engineer** | Arquitectura RL, desarrollo core, integracion OPC/Modbus | PhD/MSc en control + experiencia RL, Python, industrial protocols | **Si** | Founding technical co-founder |
| **Control Systems Engineer** | Dominio de procesos, motor restricciones, validacion seguridad | 5+ anos en plantas, conoce PID/APC, comunicacion con operadores | **Si** | Advisor senior + freelancer especializado |
| **Sales/BD Lead** | Primeras ventas, validacion propuesta valor, relaciones industria | Experiencia vendiendo a plantas, red de contactos, entiende procurement | **Si** | Founder hace outreach inicial |
| **Frontend/Integrations Dev** | Dashboard, API, conectores DCS/SCADA | Full-stack con experiencia industrial IoT, OPC-UA | No MVP1 | Tercerizar UI basica |
| **DevOps/Infra** | Deploy on-prem/edge, seguridad, monitoreo | Kubernetes, edge computing, ciberseguridad OT | No MVP1 | Cloud managed services |

### 4.3 Configuraciones de equipo

**Opcion A - Minimo absoluto (2 personas):**
- Roles: 1 ML/Control Engineer hibrido + 1 Business/Domain expert
- Viable si: Ambos tienen experiencia industrial + pueden hacer cold outreach
- Riesgo: Burnout, lentitud, coverage limitado
- Timeline a MVP: 4-6 meses

**Opcion B - Equipo balanceado (3-4 personas):**
- Roles: 1 ML Lead + 1 Control Engineer + 1 BD/Sales + 0.5 DevOps (part-time/contractor)
- Viable si: Funding seed o revenue de servicios paralelos
- Ventaja: Velocidad, especializacion, menos burnout
- Timeline a MVP: 2-3 meses

**Opcion C - Equipo completo (5+ personas):**
- Roles: +Frontend, +Customer Success, +Segundo ML engineer
- Necesario si: Post-validacion, scaling, multiples pilotos simultaneos
- Ventaja: Capacidad de escalar, soporte clientes
- Timeline a MVP: 6-8 semanas (paralelo)

### 4.4 Habilidades criticas vs. tercerizables

| Habilidad | In-house obligatorio? | Tercerizable? | Plataformas |
|-----------|----------------------|---------------|-------------|
| Algoritmos RL core | **Si** | No | - |
| Motor restricciones | **Si** | No | - |
| Integracion OPC-UA/Modbus | Parcial | Si | Upwork, Toptal (especialistas IIoT) |
| UI/Dashboard | No | **Si** | Toptal, freelancers React |
| Infra/DevOps | No | **Si** | AWS/Azure managed, contractors |
| Legal/Contratos | No | **Si** | Clerky, abogados especializados |
| Diseno | No | **Si** | Fiverr, 99designs |

---

## FASE 5: STACK TECNOLOGICO OPTIMO

### 5.1 Principio de seleccion

*El stack optimo resuelve el problema especifico con menor complejidad, maxima confiabilidad en entorno industrial, y capacidad de deployment on-prem/edge.*

### 5.2 Requerimientos tecnicos del problema

| Requerimiento | Prioridad | Implicacion tecnica |
|---------------|-----------|---------------------|
| Conexion a DCS/SCADA en tiempo real | **Alta** | OPC-UA client, Modbus, latencia <1s |
| Inferencia RL en edge (sin cloud obligatorio) | **Alta** | Modelos livianos, ONNX, Python optimizado |
| Motor restricciones deterministico | **Alta** | Logica simbolica, constraint solver, 100% predecible |
| Dashboard para operadores | Media | Web simple, responsive, alertas |
| Logging/auditoria para compliance | **Alta** | Inmutabilidad, timestamps, trazabilidad |
| Seguridad OT (air-gapped compatible) | **Alta** | On-prem first, sin dependencia cloud |

### 5.3 Arquitectura recomendada

```
+------------------+     +-------------------+     +------------------+
|   DCS / SCADA    |<--->|  SimPlant Edge    |<--->|  Dashboard Web   |
|   (PLCs, RTUs)   |     |  Gateway          |     |  (Operadores)    |
+------------------+     +-------------------+     +------------------+
        ^                        |
        |                        v
   OPC-UA/Modbus         +-------------------+
                         |  RL Optimizer     |
                         |  (Python/ONNX)    |
                         +-------------------+
                                 |
                                 v
                         +-------------------+
                         | Constraint Engine |
                         | (Deterministico)  |
                         +-------------------+
                                 |
                                 v
                         +-------------------+
                         |  Setpoint Output  |
                         |  -> DCS/PLC       |
                         +-------------------+
```

### 5.4 Stack detallado con justificacion

| Capa | Tecnologia recomendada | Por que esta y no otra | Alternativas viables |
|------|----------------------|----------------------|---------------------|
| **Lenguaje core** | Python 3.11+ | Ecosistema ML maduro, integraciones industriales | Julia (mas rapido pero menos ecosistema) |
| **RL Framework** | Stable-Baselines3 + PyTorch | Probado, documentado, facil deployment | RLlib (mas complejo), JAX (menos maduro) |
| **Constraint Engine** | Z3 Solver o OR-Tools | Deterministico, rapido, bien documentado | Custom rules engine (mas simple) |
| **Conectividad industrial** | asyncua (OPC-UA), pymodbus | Open source, activos, Python nativo | Kepware (comercial, costoso) |
| **Base de datos** | TimescaleDB (time-series) + SQLite (config) | Optimizado para series temporales, liviano | InfluxDB, QuestDB |
| **API Backend** | FastAPI | Async, rapido, tipado, documentacion auto | Flask (mas lento), Django (overkill) |
| **Frontend** | React + Recharts | Simple, componentes graficos, popularidad | Vue, Svelte |
| **Infraestructura** | Docker + K3s (edge Kubernetes) | Portable, reproducible, edge-ready | Bare metal (menos portable) |
| **Monitoreo** | Prometheus + Grafana | Estandar industria, alertas | Datadog (costoso), ELK (complejo) |
| **Auth** | Keycloak o Auth0 | SSO, RBAC, compliance-ready | Custom JWT (menos seguro) |
| **CI/CD** | GitHub Actions | Simple, integrado, gratis tier | GitLab CI, Jenkins |

### 5.5 Estimacion de costos de infraestructura

| Componente | Costo MVP/mes | Costo 10k usuarios | Costo 100k usuarios |
|------------|---------------|-------------------|---------------------|
| Edge Gateway (hardware cliente) | $0 (cliente provee) | $0 | $0 |
| Cloud dev/staging | $50-100 | $200 | $500 |
| TimescaleDB Cloud (si aplica) | $30 | $150 | $500 |
| Monitoreo (Grafana Cloud) | $0 (free tier) | $50 | $200 |
| Auth0 | $0 (free tier) | $100 | $500 |
| Dominio + SSL | $15 | $15 | $15 |
| **TOTAL** | **$100-150** | **$515** | **$1,715** |

*Nota: Modelo on-prem transfiere costos de infra al cliente. Cloud es solo para desarrollo y dashboard opcional.*

### 5.6 Deuda tecnica aceptable en MVP

**Aceptable ahora, refactorizar despues:**
- UI basica sin pulir
- Single-tenant architecture
- Logging simple (archivos, no centralizado)
- Tests unitarios minimos (focus en integration tests)
- Documentacion interna limitada

**NO comprometer nunca:**
- Motor de restricciones deterministico (seguridad critica)
- Auditoria de setpoints (compliance)
- Rollback automatico si valores fuera de rango
- Separacion clara RL optimizer vs constraint validation
- Conexion robusta OPC-UA con reconexion automatica

---

## FASE 6: GUIA DE ACCION

### 6.1 Fases macro

| Fase | Objetivo | Duracion | Entregable clave | Criterio exito |
|------|----------|----------|------------------|----------------|
| 0. Pre-validacion | Confirmar demanda sin codigo | 2-4 semanas | 5+ LOIs o pagos por piloto | >3 empresas quieren pagar |
| 1. MVP | Producto minimo funcional | 8-12 semanas | Sistema funcionando en 1 planta real | Setpoints estables, sin incidentes |
| 2. Beta | Validacion con 3-5 clientes | 8-12 semanas | Metricas de ahorro documentadas | >10% mejora demostrable |
| 3. Launch | Producto comercial | 4-8 semanas | Pricing, contratos, onboarding | 3+ clientes pagando |
| 4. Iteracion | Product-market fit | Continuo | NPS >40, churn <5% mensual | Revenue recurrente creciendo |

### 6.2 Fase 0: Pre-validacion (ANTES de escribir codigo)

| Paso | Accion especifica | Entregable | Metrica exito | Herramientas |
|------|------------------|------------|---------------|--------------|
| 0.1 | Crear landing page con propuesta valor clara | URL live + formulario captura | >100 visitas, >10 leads | Carrd.co, Tally.so |
| 0.2 | Identificar 20 plantas target en LinkedIn | Lista 20 contactos con email/tel | Lista completa | LinkedIn Sales Nav, Apollo.io |
| 0.3 | Cold outreach: 20 emails + 10 llamadas | Respuestas documentadas | >5 respuestas positivas | Gmail, Calendly |
| 0.4 | Reuniones discovery (5-10) | Notas de pain points, willingness to pay | >3 dicen "pagaria por esto" | Zoom, Notion |
| 0.5 | **DECISION GO/NO-GO** | Documento decision | LOIs o depositos | - |

**Checkpoint Pre-validacion:**
- **GO si**: 3+ empresas firman LOI o pagan deposito para piloto
- **PIVOTAR si**: Interes pero no en la solucion propuesta (cambiar feature/pricing)
- **KILL si**: <3 respuestas positivas en 50 contactos

### 6.3 Fase 1: MVP

| Paso | Accion especifica | Entregable | Duracion | Dependencias |
|------|------------------|------------|----------|--------------|
| 1.1 | Setup repo, CI/CD, estructura proyecto | Repo GitHub, pipeline basico | 3 dias | - |
| 1.2 | Implementar conexion OPC-UA/Modbus | Lectura datos en tiempo real | 1 semana | Acceso a DCS simulado |
| 1.3 | Desarrollar RL agent basico (SAC/PPO) | Modelo entrenado en simulador | 2 semanas | Datos historicos cliente |
| 1.4 | Implementar constraint engine | Validador deterministico | 1 semana | Limites operativos cliente |
| 1.5 | Integrar RL + constraints + output setpoints | Sistema end-to-end | 1 semana | 1.2, 1.3, 1.4 |
| 1.6 | Dashboard operador minimo | Web con graficos basicos | 1 semana | 1.5 |
| 1.7 | Deploy en edge gateway cliente | Sistema corriendo en planta | 1 semana | Hardware cliente, acceso red |
| 1.8 | Periodo shadow mode (solo observa) | Log de setpoints sugeridos | 2 semanas | 1.7 |
| 1.9 | Activar closed-loop gradual | Control activo supervisado | 2 semanas | Aprobacion operaciones |

### 6.4 Fase 2: Beta

| Paso | Accion especifica | Entregable | Metrica clave |
|------|------------------|------------|---------------|
| 2.1 | Onboarding 2-4 clientes adicionales | Sistemas instalados | Time-to-value <4 semanas |
| 2.2 | Recopilar metricas de ahorro | Dashboard con KPIs | % reduccion energia/variabilidad |
| 2.3 | Iterar producto segun feedback | Releases semanales | Issues resueltos <48h |
| 2.4 | Documentar casos de exito | Case studies con numeros | ROI cuantificado |
| 2.5 | Definir pricing basado en valor | Estructura de precios final | Willingness-to-pay validado |

### 6.5 Fase 3: Launch

| Paso | Accion especifica | Canales | Metrica clave |
|------|------------------|---------|---------------|
| 3.1 | Publicar case studies | LinkedIn, blog, PR industrial | >1000 views, >50 leads |
| 3.2 | Webinar tecnico para ingenieros | LinkedIn Live, YouTube | >100 asistentes |
| 3.3 | Presencia en evento industria (ISA, ARC) | Booth o speaking slot | >20 leads calificados |
| 3.4 | Partner channel (integradores) | Outreach a system integrators | 2+ partners firmados |
| 3.5 | Self-service trial (si aplica) | Free assessment automatizado | Conversion >5% |

### 6.6 Criterios de abandono

| Fase | Trigger KILL | Trigger PIVOT |
|------|--------------|---------------|
| Pre-validacion | <3/50 respuestas positivas en 4 semanas | Interes pero diferente problema/solucion |
| MVP | Incidente de seguridad por setpoint incorrecto | Funciona pero cliente quiere feature diferente |
| Beta | <5% mejora despues de 8 semanas en 3+ clientes | Mejora pero modelo de negocio no cierra |
| Post-launch | Churn >20% mensual despues de 6 meses | Revenue flat pero NPS alto |

---

## FASE 7: VEREDICTO FINAL

### 7.1 Scorecard de viabilidad

| Criterio | Peso | Score (1-10) | Ponderado | Evidencia clave |
|----------|------|--------------|-----------|-----------------|
| Demanda validada | 25% | 8 | 2.00 | 7 evidencias reales, casos Yokogawa/Imubit |
| Diferenciacion | 20% | 7 | 1.40 | RL adaptativo + constraints deterministicos + SMB focus |
| Viabilidad tecnica | 20% | 9 | 1.80 | Despliegues reales CDU, ENEOS probados |
| Tamano de mercado | 15% | 8 | 1.20 | $3.2B TAM, 17-22% CAGR |
| Claridad monetizacion | 10% | 7 | 0.70 | Imubit $1k/user, gain-sharing validado |
| Timing | 10% | 8 | 0.80 | Escasez APC engineers, presion ESG, tech madura |
| **TOTAL** | 100% | | **7.9/10** | |

### 7.2 Recomendacion

**EJECUTAR (con estrategia SMB/servicios)**

SimPlant ataca un problema real y doloroso (degradacion APC, escasez expertos) con tecnologia validada comercialmente (Yokogawa 2023, Imubit). El timing es optimo: la industria busca alternativas al APC tradicional pero desconfia de GenAI no deterministico.

**Cambio vs. analisis anterior:** El veredicto original de "NO EJECUTAR" asumia competir head-to-head con Imubit en enterprise. La estrategia correcta es:
1. **Target SMB**: Plantas medianas que no pueden pagar $1000/user/mes ni $100k+ de AspenTech
2. **Servicios primero**: Generar credibilidad via consulting antes de producto puro
3. **Procesos simples**: HVAC industrial, camaras frigorificas, hornos - no refinerias complejas

**Principal riesgo a monitorear:** Ciclos de venta largos en industria (6-18 meses). Mitigacion: empezar con plantas medianas/HVAC industrial donde decision es mas rapida.

### 7.3 Resumen ejecutivo

| Elemento | Valor |
|----------|-------|
| **La idea en una oracion** | Software RL que optimiza setpoints industriales automaticamente, con validacion de seguridad deterministica |
| **Mercado objetivo** | Plantas de proceso medianas (quimicas, alimentos, HVAC industrial) que no pueden pagar APC tradicional |
| **Diferenciador clave** | Adaptacion continua sin re-modelado + constraint engine que elimina riesgo de "hallucinations" |
| **Equipo minimo necesario** | 2-3 personas: ML/Control Engineer + Domain Expert/Sales |
| **Stack recomendado** | Python + PyTorch + Stable-Baselines3 + Z3 Solver + FastAPI + TimescaleDB + Docker/K3s |
| **Inversion inicial estimada** | $50-100k (6 meses runway para equipo minimo) |
| **Timeline a MVP** | 8-12 semanas con equipo de 2-3 |
| **Riesgo principal** | Ciclo de venta largo en industria tradicional |

### 7.4 Primera accion (ejecutable en <2 horas)

**PASO INMEDIATO:**

1. **Accion**: Crear lista de 20 plantas target en tu zona geografica usando LinkedIn Sales Navigator
   - Buscar: "Plant Manager" OR "Operations Director" OR "Process Engineer" en industrias: quimica, alimentos, farmaceutica, HVAC
   - Priorizar empresas medianas (50-500 empleados) - mas agiles en decisiones
   
2. **Resultado esperado**: Spreadsheet con 20 nombres, empresas, LinkedIn URLs, y emails inferidos (usando Hunter.io o Apollo.io)

3. **Siguiente paso**: Redactar email de cold outreach personalizado y enviar primeros 5 manana

---

## ANEXO A: Reconciliacion con Analisis Anterior

### Por que cambio el veredicto?

| Factor | Analisis 2025 | Realidad 2026 | Impacto |
|--------|---------------|---------------|---------|
| Competencia | "Imposible competir con Imubit" | Imubit solo atiende enterprise O&G, nicho SMB abierto | Alto |
| Viabilidad tecnica | "MVP 6+ meses" | Stack maduro (SB3, asyncua, Z3), referencias OSS | Alto |
| Casos de exito | "Solo Imubit" | Yokogawa ENEOS (40% ahorro), CDU RL (84% menos intervencion) | Alto |
| Mercado accesible | "Solo enterprise" | Plantas medianas con dolor pero sin presupuesto | Alto |

### Pivotes del analisis anterior que siguen siendo validos

1. **SimPlant Academy**: Sigue siendo opcion de lower-risk para generar credibilidad
2. **Consultancy + OSS**: Puede ser entrada a primeros clientes
3. **SimPlant Sandbox**: Digital twin para pruebas - complemento, no core

---

## ANEXO B: Referencias y Fuentes

1. Mordor Intelligence - Industrial AI Software Market (2025): [Link](https://www.mordorintelligence.com/industry-reports/industrial-ai-software-market)
2. Yokogawa FKDPP Deployment (2023): [Link](https://yokogawa.com/news/press-releases/2023/2023-03-30)
3. CDU RL Deployment Paper (2024): [Link](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4928069)
4. APC Challenges - Electrical Engineering Center: [Link](https://www.electricneutron.com/challenges-in-advanced-process-control-apc/)
5. Imubit Pricing - Capterra: [Link](https://www.capterra.com/p/10032267/Imubit/)
6. Reuters - Manufacturing AI Concerns: [Link](https://www.reuters.com/technology/artificial-intelligence/manufacturers-slow-gen-ai-rollout-rising-accuracy-concerns-says-study-2024-07-10/)
7. Market.us - AI Process Optimization Market: [Link](https://market.us/report/ai-for-process-optimization-market/)
8. Verdantix - Industrial AI Analytics (2025): [Link](https://www.verdantix.com/venture/report/market-size-and-forecast--industrial-ai-analytics-2025-2031-global)
9. PLCTalk Forum - AI Skepticism: [Link](https://www.plctalk.net/threads/ot-ai-process-optimisation.140980/)
10. Control Global - Reimagine APC: [Link](https://www.controlglobal.com/home/article/11306312/reimagine-the-advanced-process-control-paradigm)
