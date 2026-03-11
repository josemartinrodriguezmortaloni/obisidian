**1. ¿Qué problema humano resuelve?** Evita que decisiones de control críticas (que pueden causar explosiones o pérdidas millonarias) se tomen basadas en sensores rotos, ruidosos o desincronizados. El operador necesita que el software opere sobre la _verdad_ física, no sobre la _ilusión_ eléctrica de un cable dañado.

**2. ¿Cuál es su única razón de existir?** Transformar telemetría industrial caótica y asíncrona en un vector de estado coherente, temporalmente alineado y libre de anomalías.

**3. ¿Qué pasa cuando esto cambie?** Los algoritmos de filtrado (ej. cambiar de media móvil a un filtro de Kalman) o las reglas de imputación de valores nulos evolucionarán. Esto cambiará internamente en este módulo. El `Runtime` nunca se entera; sigue recibiendo un vector de estado limpio.

**4. ¿Puedo eliminarlo sin romper lo no relacionado?** Sí. Si lo eliminas, el `Gateway` puede pasar los datos crudos directamente al `Runtime`. El sistema no se rompe a nivel de compilación, pero su capacidad para lidiar con la entropía física desaparece.

# **Estructura Interna:**

```plaintext
src/
└── signal_conditioning/
    ├── __init__.py
    ├── domain/
    │   ├── __init__.py
    │   ├── models.py          # Value Objects: RawSignal, CleanSignal, QualityEnum
    │   ├── filters.py         # Matemáticas puras: EMA, LowPass
    │   └── anomalies.py       # Reglas de física: Rate of Change, Outliers
    ├── application/
    │   ├── __init__.py
    │   └── pipeline.py        # Orquestador: Ingesta -> Filtro -> Validación
    └── api/
        ├── __init__.py
        └── provider.py        # Contrato público consumido por el Runtime
```
# Requisitos
### 1. Requisitos de Alineación Temporal (Sincronización)

**El Problema Base:** El `Runtime` necesita una foto del universo en el instante exacto $T$. Pero los DCS envían datos asíncronamente. Un sensor de flujo actualiza cada **100 ms**, mientras que un analizador químico actualiza cada **5 minutos**.

- **Req 1.1: Resampling Determinista.** El módulo debe ser capaz de tomar un flujo asíncrono de eventos y generar un vector síncrono a demanda (ej. cada **1 segundo**).
    
- **Req 1.2: Estrategias de Retención (Hold/Interpolation).** Para señales rápidas, debe promediar los valores recibidos en la ventana $\Delta t$. Para señales lentas (como el analizador químico), debe implementar _Zero-Order Hold_ (mantener el último valor conocido) hasta recibir una nueva actualización.
    
- **Req 1.3: Límite de Caducidad (Staleness).** Cada _tag_ debe tener un parámetro de `max_staleness` (ej. **5 segundos** para presión, **30 minutos** para calidad). Si no se recibe un valor nuevo en ese tiempo, el módulo **no debe** seguir repitiendo el valor viejo; debe marcar la señal como "Muerta/Caduca".
    

### 2. Requisitos de Integridad Física (Detección de Anomalías)

**El Problema Base:** Los sensores fallan de maneras predecibles. Un cable suelto genera picos a 0 o al máximo de escala.

- **Req 2.1: Límites Absolutos de Rango.** Si un transmisor de temperatura de 0-200°C envía **999°C** (típico valor de error en PLCs), el módulo debe interceptarlo y clasificarlo como anomalía antes de que contamine el cálculo.
    
- **Req 2.2: Tasa de Cambio Máxima (Rate of Change - RoC).** Basado en la inercia del equipo físico, el módulo debe rechazar cambios físicamente imposibles. (Ejemplo: Un tanque de 10,000 litros no puede cambiar su nivel de **10%** a **90%** en **1 segundo**; eso es ruido de sensor, no física).
    
- **Req 2.3: Detección de Congelamiento (Flatlining).** Si una variable dinámica (como el flujo turbulento) reporta exactamente el mismo valor de coma flotante durante excesivos ciclos consecutivos, el módulo debe inferir que el sensor se ha colgado.
    

### 3. Requisitos de Filtrado Matemático (Reducción de Ruido)

**El Problema Base:** La corriente eléctrica de 4-20mA tiene ruido blanco inducido por motores cercanos.

- **Req 3.1: Filtro Paso Bajo / Suavizado.** Debe implementar algoritmos de suavizado como la Media Móvil Exponencial (EMA), definida como $y_t = \alpha x_t + (1 - \alpha) y_{t-1}$, donde $\alpha$ es configurable por variable.
    
- **Req 3.2: Eficiencia de Memoria $O(1)$.** El filtrado no debe requerir almacenar ventanas masivas de datos históricos en memoria, sino calcularse de forma iterativa conservando solo el estado anterior, para evitar consumo de RAM y pausas de _Garbage Collection_.
    

### 4. Requisitos de Propagación de Estado (El Contrato con el Runtime)

**El Problema Base:** Cuando ocurre una anomalía (Req 2), ¿qué le enviamos al agente RL?

- **Req 4.1: Calidad Explícita de la Señal.** El vector de salida NO puede ser solo un array numérico tipo `[45.2, 120.5]`. Debe ser un objeto compuesto que incluya el valor de ingeniería y una enumeración de calidad: `GOOD`, `UNCERTAIN`, `BAD_SENSOR`, `STALE`, `IMPUTED`.
    
- **Req 4.2: Lógica de Imputación Aislada.** Si un sensor secundario falla, el módulo puede intentar estimarlo usando otra variable correlacionada, pero debe etiquetar ese dato como `IMPUTED`.
    
- **Req 4.3: Condición de Parada Segura (Trip Signal).** Si el módulo detecta que un porcentaje crítico de los sensores clave están caídos o son anómalos, el vector de estado debe incluir un flag de `ENV_UNSAFE`. El `Runtime` leerá este flag y detendrá inmediatamente el control en lazo cerrado (Closed-Loop), pasando a un estado seguro o a modo manual.