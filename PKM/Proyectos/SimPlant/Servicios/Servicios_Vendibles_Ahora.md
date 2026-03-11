# Servicios Vendibles HOY con Activos Actuales de SimPlant

**Fecha:** 20 de Enero de 2026  
**Objetivo:** Identificar revenue streams inmediatos mientras se desarrolla el producto completo

---

## Inventario de Activos Actuales

Antes de definir servicios, este es lo que YA TENEMOS funcionando:

| Activo | Status | Uso Comercial Potencial |
|--------|--------|------------------------|
| Simulador FCC (scipy ODE) | Funcionando | Demos, estudios, entrenamiento |
| Pipeline BC -> SAC | Funcionando | Benchmark de control optimo |
| Politicas MPC (do-mpc) | Funcionando | Referencia de control avanzado |
| Politicas PID | Funcionando | Baseline para comparacion |
| Export ONNX/TensorRT | Funcionando | PoC de deployment |
| Metricas de validacion | Funcionando | Cuantificacion de gaps |
| CLI operativo | Funcionando | Demostraciones tecnicas |
| Prometheus + Grafana | Funcionando | Dashboards de performance |

---

## SERVICIO #1: Auditoria de Performance de Control

### Pain Point

> "20-30% of controls are in manual mode, up to 85% of control loops have sub-optimal settings, 20-40% of controllers are oscillating" - Valmet PlantTriage

**Cita real de la industria:** *"A typical process plant is losing millions annually from poorly-performing control systems."* - Valmet

### Target Audience

- **Primario:** Refinerias medianas (50-100k bpd) sin equipo APC dedicado
- **Secundario:** Plantas quimicas con DCS envejecido (+10 anos)
- **Decisor:** Gerente de Operaciones, Jefe de Instrumentacion
- **Geografia inicial:** Argentina (YPF, Axion), regional (Ecopetrol, Petrobras)

### The "Why" - Impacto Cuantificado

| Problema                | Impacto                                            | Fuente       |
| ----------------------- | -------------------------------------------------- | ------------ |
| Lazos en manual         | Oportunidad de optimizacion perdida                | ABB Research |
| Lazos oscilando         | 2-5% exceso de consumo energetico                  | Valmet       |
| Variabilidad            | 1-3% perdida de margen por cada 1% de variabilidad | Industria    |
| **Total para 100k bpd** | **$2-10M/ano recuperables**                        | Benchmarks   |

### Solucion Lean: Servicio de Auditoria

**Alcance del servicio:**
1. Extraer 3-6 meses de datos del historian (OPC UA / CSV export)
2. Calcular KPIs por lazo (IAE, oscilacion, factor de servicio, tiempo en manual)
3. **DIFERENCIADOR:** Comparar contra modelo RL entrenado como "gold standard"
4. Cuantificar gap economico en USD
5. Entregar recomendaciones priorizadas con ROI

**Lo que usamos de SimPlant:**
- Modelo SAC entrenado como benchmark de "control optimo"
- Framework de metricas para cuantificar performance
- Simulador para validar mejoras propuestas

**Entregables:**
- Reporte de Performance (30-50 paginas)
- Base de datos lazo-por-lazo con scores
- Lista de Quick Wins (top 10 lazos criticos)
- Presentacion ejecutiva

**Timeline:** 4-6 semanas

### Monetizacion

| Alcance | Precio LATAM | Benchmark US/EU |
|---------|--------------|-----------------|
| Unidad chica (<100 lazos) | $25,000 - $35,000 | $80,000 - $150,000 |
| Unidad grande (200-500 lazos) | $50,000 - $80,000 | $100,000 - $200,000 |

**Modelo:** Precio fijo por proyecto. 50% adelanto, 50% contra entrega.

### Competencia y Gaps

| Competidor | Debilidad | Nuestra Ventaja |
|------------|-----------|-----------------|
| Valmet PlantTriage | Software-first, caro | Services-first, 50% mas barato |
| Emerson DeltaV InSight | Atado a DCS DeltaV | Vendor-agnostic |
| PiControl Solutions | Foco US, estructura de costo alta | Presencia LATAM, costo competitivo |
| Integradores locales | Sin benchmark AI | **Unico con comparacion RL** |

**Gap explotado:** NADIE compara contra un "baseline optimo" generado por AI. Todos usan metodos estadisticos tradicionales.

---

## SERVICIO #2: Campana de Sintonizacion PID

### Pain Point

> "PID tuning automation typically has a half-life of six months. The manufacturers lose millions of dollars each year due to poor control performance." - ABB

**Frustracion real:** Los lazos se sintonizan una vez durante commissioning y nunca mas. Las condiciones cambian y la performance se degrada silenciosamente.

### Target Audience

- **Primario:** Clientes que completaron Auditoria con nosotros (conversion natural)
- **Secundario:** Plantas en turnaround/parada de mantenimiento
- **Terciario:** Plantas con alta tasa de alarmas (sintoma de mala sintonizacion)

### The "Why" - Impacto Cuantificado

| Beneficio | Cuantificacion |
|-----------|----------------|
| Reduccion de variabilidad | 30-50% por lazo sintonizado |
| Ahorro de energia | 2-5% en sistemas afectados |
| Aumento de throughput | 1-3% por reduccion de variabilidad |
| Reduccion de carga operador | 40% menos intervenciones manuales |
| **Valor por lazo** | **$1,000 - $10,000/ano** |

### Solucion Lean: Campana de Tuning

**Alcance:**
1. Seleccion de lazos prioritarios (de auditoria o cliente)
2. Step testing durante operacion o parada
3. Calculo de parametros usando metodologia RBT + validacion en simulacion
4. Implementacion y validacion (metricas antes/despues)
5. Documentacion y entrenamiento

**Lo que usamos de SimPlant:**
- Simulador para testear parametros antes de implementar
- Modelo RL como referencia de comportamiento deseado
- Metricas para cuantificar mejora

**Entregables:**
- Parametros nuevos documentados (Kp, Ti, Td)
- Comparacion antes/despues con graficos
- Sesion de entrenamiento para operadores (2-4 horas)

**Timeline:** 1-4 semanas dependiendo de cantidad

### Monetizacion

| Modelo | Precio |
|--------|--------|
| Por-lazo simple | $300 - $500 USD |
| Por-lazo complejo (cascada) | $800 - $1,500 USD |
| Campana 50 lazos | $20,000 - $35,000 USD |
| Campana 100 lazos | $40,000 - $60,000 USD |

**Margen:** ~70% (principalmente mano de obra)

### Competencia y Gaps

| Competidor | Precio | Debilidad |
|------------|--------|-----------|
| PiControl | $1,000-2,000/lazo | Foco US, caro |
| Valmet | $1,500-2,500/lazo | Requiere su software |
| Tecnicos locales | $200-400/lazo | Sin metodologia, prueba y error |

**Gap:** Precio competitivo + metodologia sistematica + validacion en simulacion.

---

## SERVICIO #3: Estudio de Factibilidad APC

### Pain Point

> "APC projects have substantial benefits ($500K-2M payback) but require significant investment. Plants need justification before committing." - Control Global

**Frustracion real:** Las plantas escuchan sobre beneficios de APC pero no saben si aplica a SU situacion especifica.

### Target Audience

- **Primario:** Plantas considerando APC por primera vez
- **Secundario:** Plantas con proyectos APC fallidos buscando reinicio
- **Terciario:** Plantas con APC envejecido evaluando upgrade

### The "Why" - Impacto Cuantificado

**Beneficios APC documentados:**

| Caso de Estudio | Beneficio | Fuente |
|-----------------|-----------|--------|
| Axens FCC (60k bpd) | $5.6M/ano | Papers industriales |
| Emerson refineria | $700K/ano | Case study |
| Rompetrol crude unit | $1.9M/ano | Documentado |
| **Benchmark general** | **2-6% reduccion costos operativos** | Industria |

### Solucion Lean: Estudio de Factibilidad

**Alcance:**
1. Analizar datos del historian (3-6 meses)
2. Caracterizar operacion actual y variabilidad
3. Identificar constraints y oportunidades de push
4. **DIFERENCIADOR:** Simular escenarios con modelo RL para cuantificar potencial
5. Comparar MPC tradicional vs RL-APC (nosotros podemos entregar ambos)
6. Entregar roadmap con presupuesto y timeline

**Lo que usamos de SimPlant:**
- Simulador calibrado al proceso del cliente
- Modelo RL entrenado para demostrar "que es posible"
- Framework de metricas para proyectar beneficios

**Entregables:**
- Reporte de Factibilidad (40-60 paginas)
- Modelo de calculo de beneficios (Excel)
- Demo de simulacion (opcional pero poderoso)
- Roadmap de implementacion
- Presentacion ejecutiva

**Timeline:** 8-10 semanas

### Monetizacion

| Alcance | Precio |
|---------|--------|
| Unidad unica | $60,000 - $100,000 USD |
| Multi-unidad (2-3) | $120,000 - $180,000 USD |
| Refinery-wide | $200,000 - $350,000 USD |

**Upsell natural:** El estudio lleva a implementacion APC ($450K-750K)

### Competencia y Gaps

| Competidor | Precio | Debilidad |
|------------|--------|-----------|
| AspenTech | $100K-200K | Sesgado a recomendar DMC3 |
| Honeywell | $100K-200K | Sesgado a recomendar RMPCT |
| Yokogawa | $80K-150K | Limitado fuera de su DCS |

**Gap explotado:** Somos vendor-neutral. Podemos comparar objetivamente MPC vs RL y recomendar lo mejor para EL CLIENTE, no para nuestro producto.

---

## SERVICIO #4: Desarrollo de Escenarios de Entrenamiento (OTS Lite)

### Pain Point

> "The investment in an OTS system is rarely small ($500K-2M). Unfortunately, most licensed OTS are rarely used again after initial operator training." - SimulateLive/Inprocess

**Frustracion real:** OTS completo es caro y complejo. Las plantas necesitan entrenamiento pero no pueden justificar $1M+.

### Target Audience

- **Primario:** Plantas sin OTS que necesitan entrenar operadores
- **Secundario:** Instituciones educativas (universidades, escuelas tecnicas)
- **Terciario:** Plantas con OTS obsoleto buscando contenido nuevo

### The "Why" - Impacto Cuantificado

| Beneficio | Cuantificacion |
|-----------|----------------|
| Reduccion de incidentes | ~90% prevenibles con entrenamiento (industria) |
| Tiempo de competency | 30-50% mas rapido con simulacion |
| Costo de error real | $125K-220K/hora de downtime |
| Costo de OTS completo | $500K-2M (fuera de alcance para muchos) |

### Solucion Lean: Escenarios de Entrenamiento Web-Based

**Alcance:**
1. Desarrollar modelo dinamico del proceso del cliente
2. Crear 10-20 escenarios de entrenamiento (arranque, parada, emergencias, upset)
3. Interfaz web simple (no emulacion completa de DCS)
4. Metricas de performance del operador

**Lo que usamos de SimPlant:**
- Simulador FCC existente (adaptable a otros procesos)
- Framework de validacion para medir performance
- Capacidad de generar perturbaciones y escenarios

**Entregables:**
- Acceso web a simulador (SaaS o on-premise)
- 10-20 escenarios documentados
- Guia del instructor
- Metricas de evaluacion

**Timeline:** 8-12 semanas para desarrollo inicial

### Monetizacion

| Modelo | Precio |
|--------|--------|
| 10-20 escenarios | $50,000 - $100,000 USD (one-time) |
| Contenido de unidad completa | $150,000 - $300,000 USD |
| Mantenimiento anual | $15,000 - $30,000 USD |
| **SaaS por usuario** | $200-500/usuario/mes (potencial) |

### Competencia y Gaps

| Competidor | Precio | Debilidad |
|------------|--------|-----------|
| TSC Simulation | $300K-1M | Enterprise, caro |
| AspenTech AOT | $200K-500K | Requiere licencia HYSYS |
| Yokogawa OmegaLand | $400K-800K | Atado a Yokogawa |

**Gap:** "OTS Lite" asequible para plantas medianas. Sin necesidad de emulacion completa de DCS.

---

## SERVICIO #5: Retainer de Data Science para Proceso

### Pain Point

> "Plants lack internal data science capacity. They need ongoing support for analytics initiatives but don't want to hire full-time."

**Frustracion real:** Tienen datos pero no saben que hacer con ellos. Quieren "AI" pero no tienen expertise interno.

### Target Audience

- **Primario:** Clientes existentes post-auditoria/tuning
- **Secundario:** Plantas con iniciativas de digitalizacion
- **Terciario:** Gerentes buscando "quick wins" de analytics

### The "Why" - Impacto Cuantificado

| Tipo de Analisis | Valor Tipico |
|------------------|--------------|
| Soft sensor (inferencial) | $50K-200K/ano en lab savings |
| Deteccion de anomalias | Prevencion de downtime ($125K/hr) |
| Optimizacion de energia | 2-5% reduccion de consumo |
| Root cause analysis | Reduccion de tiempo de investigacion |

### Solucion Lean: Retainer Mensual

**Tiers:**

| Tier | Horas/Mes | Precio | Incluye |
|------|-----------|--------|---------|
| Bronze | 40 horas | $6,000/mes | Analisis ad-hoc, reportes, soporte |
| Silver | 80 horas | $10,000/mes | + Soft sensors, dashboards |
| Gold | 160 horas | $18,000/mes | + Desarrollo de modelos, optimizacion |

**Lo que usamos de SimPlant:**
- Expertise en ML/RL
- Framework de metricas
- Capacidad de simulacion para validar modelos

**Entregables tipicos:**
- Dashboards de KPIs
- Soft sensors (inferential analyzers)
- Analisis de root cause
- Modelos predictivos

### Monetizacion

**Revenue recurrente:** $72K - $216K/ano por cliente

**Escalabilidad:** Bajo costo marginal - principalmente tiempo de ingeniero

### Competencia y Gaps

| Competidor | Modelo | Debilidad |
|------------|--------|-----------|
| EY, Accenture | Project-based, caro | $300-500/hr, ciclo de venta largo |
| Mu Sigma | Offshore, escala | Menos conocimiento de dominio O&G |
| Freelancers | Ad-hoc | Sin continuidad, calidad variable |

**Gap:** Retainer predecible con expertise de dominio O&G + AI.

---

## Matriz de Priorizacion de Servicios

| Servicio | Revenue Potencial | Esfuerzo | Time-to-Revenue | Prioridad |
|----------|-------------------|----------|-----------------|-----------|
| #1 Auditoria de Control | $25K-80K/proyecto | Medio | 6 semanas | **ALTA** |
| #2 Tuning PID | $20K-60K/campana | Bajo | 2-4 semanas | **ALTA** |
| #3 Factibilidad APC | $60K-180K/estudio | Alto | 10 semanas | **MEDIA** |
| #4 OTS Lite | $50K-300K | Muy Alto | 12+ semanas | **BAJA** |
| #5 Retainer Data Science | $72K-216K/ano | Bajo | Inmediato | **ALTA** |

---

## Roadmap de Go-to-Market

### Fase 1: Primeros 30 Dias - VALIDACION

| Semana | Actividad | Objetivo |
|--------|-----------|----------|
| 1-2 | Empaquetar oferta de Auditoria | Propuesta template lista |
| 1-2 | Identificar 5 contactos calientes | Lista de prospects |
| 3-4 | Outreach a YPF, Axion, regionales | 2-3 reuniones agendadas |
| 3-4 | Preparar demo de benchmark RL | Material de venta |

### Fase 2: Dias 30-90 - PRIMER CLIENTE

| Mes | Actividad | Objetivo |
|-----|-----------|----------|
| 2 | Cerrar primera Auditoria | Aunque sea con descuento |
| 2-3 | Ejecutar proyecto impecable | Testimonial + referral |
| 3 | Proponer Tuning como follow-up | Expansion de cuenta |

### Fase 3: Dias 90-180 - ESCALAMIENTO

| Mes | Actividad | Objetivo |
|-----|-----------|----------|
| 4-5 | 2-3 auditorias adicionales | Pipeline de $100K+ |
| 5-6 | Primer estudio de Factibilidad | Proyecto de $60K+ |
| 6 | Establecer 1-2 retainers | Revenue recurrente |

---

## Proyeccion Financiera Ano 1

| Trimestre | Servicios | Revenue Estimado |
|-----------|-----------|------------------|
| Q1 | 2 Auditorias + 1 Campana PID | $70,000 - $120,000 |
| Q2 | 2 Auditorias + 2 Campanas + 1 Factibilidad | $150,000 - $250,000 |
| Q3 | 3 Auditorias + 3 Campanas + 2 Retainers | $200,000 - $350,000 |
| Q4 | Igual Q3 + Piloto RL-APC | $300,000 - $500,000 |
| **TOTAL ANO 1** | | **$720,000 - $1,220,000** |

---

## Acciones Inmediatas (Esta Semana)

| # | Accion | Owner | Deadline |
|---|--------|-------|----------|
| 1 | Crear propuesta template para Auditoria | Tech Lead | 3 dias |
| 2 | Listar 10 contactos en YPF/Axion/regionales | PM | 3 dias |
| 3 | Preparar one-pager de diferenciacion (benchmark RL) | Marketing | 5 dias |
| 4 | Configurar CRM basico (HubSpot free) | PM | 5 dias |
| 5 | Escribir email de outreach personalizado | PM | 7 dias |

---

## Conclusion

**Con lo que YA tienen, pueden generar $70K-120K en el primer trimestre** vendiendo:

1. **Auditorias de Control** (gateway service, bajo riesgo para cliente)
2. **Campanas de Tuning** (follow-up natural, alto margen)
3. **Retainers de Data Science** (revenue recurrente)

El producto completo (SimPlant Runtime) se desarrolla EN PARALELO mientras los servicios generan:
- Cash flow para financiar desarrollo
- Relaciones con clientes potenciales
- Datos reales para mejorar modelos
- Credibilidad y case studies

**El servicio de Auditoria es el "caballo de Troya" para entrar a las cuentas.** Una vez adentro, la expansion es natural: Tuning -> Factibilidad -> Implementacion APC.

---

*Documento generado: 20 de Enero de 2026*
