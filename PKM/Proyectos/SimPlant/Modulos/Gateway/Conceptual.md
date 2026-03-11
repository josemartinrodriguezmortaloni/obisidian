Diagrama de clases
```mermaid
classDiagram

    direction TB

    class Gate {

        <<Interface>>

        +connect() bool

        +disconnect() void

        +read_state() PlantState

        +write_setpoints(ValidatedSetpoints) WriteResult

        +health_check() HealthStatus

    }

    class Protocol {

        <<Abstract>>

        #_config: Config

        #_connected: bool

        #_last_values_buffer: Dict

        #_last_read_timestamp: datetime

        #_current_backoff_s: int

        +connect() bool

        +disconnect() void

        +read_state() PlantState

        +write_setpoints(ValidatedSetpoints) WriteResult

        +health_check() HealthStatus

        #_reconnect_with_backoff() void

        #_store_in_buffer(measurements: Dict) void

        #_get_from_buffer() PlantState

        #_reset_backoff() void

        #_do_connect()* bool

        #_do_disconnect()* void

        #_do_read_tag(TagMapping)* TagReadResult

        #_do_write_tag(TagMapping, float)* bool

    }

    class OPCAdapter {

        -_client: OPCUAClient

        -_node_cache: Dict

        #_do_connect() bool

        #_do_disconnect() void

        #_do_read_tag(TagMapping) TagReadResult

        #_do_write_tag(TagMapping, float) bool

    }

    class ModbusAdapter {

        -_client: ModbusTcpClient

        #_do_connect() bool

        #_do_disconnect() void

        #_do_read_tag(TagMapping) TagReadResult

        #_do_write_tag(TagMapping, float) bool

    }

    class SimulationAdapter {

        -_simulated_values: Dict

        #_do_connect() bool

        #_do_disconnect() void

        #_do_read_tag(TagMapping) TagReadResult

        #_do_write_tag(TagMapping, float) bool

    }

    class Config {

        <<ValueObject>>

        +protocol_type: ProtocolType

        +host: str

        +port: int

        +credentials: Credentials

        +tag_mappings: List~TagMapping~

        +get_mapping_for_tag(tag_name: str) TagMapping

    }

    class TagMapping {

        <<ValueObject>>

        +internal_name: str

        +protocol_identifier: str

        +data_type: DataType

    }

    class PlantState {

        <<ValueObject>>

        +timestamp: datetime

        +measurements: Dict~str, float~

        +quality: Dict~str, QualityCode~

        +get_value(tag: str) float

        +get_quality(tag: str) QualityCode

        +is_all_good() bool

    }

    class ValidatedSetpoints {

        <<ValueObject>>

        +setpoints: List~Setpoint~

        +timestamp: datetime

    }

    class WriteResult {

        <<ValueObject>>

        +results: List~WriteTagResult~

        +timestamp: datetime

        +is_all_success() bool

        +get_failed_tags() List~str~

    }

    class WriteTagResult {

        <<ValueObject>>

        +tag: str

        +success: bool

        +error_message: Optional~str~

    }

    class HealthStatus {

        <<ValueObject>>

        +connected: bool

        +latency_ms: Optional~int~

        +last_successful_read: Optional~datetime~

        +is_healthy() bool

    }

    class QualityCode {

        <<Enum>>

        GOOD

        BAD

        UNCERTAIN

        STALE

    }

    Gate <|.. Protocol

    Protocol <|-- OPCAdapter

    Protocol <|-- ModbusAdapter

    Protocol <|-- SimulationAdapter

    Protocol --> Config

    Config --> TagMapping

    Gate --> PlantState

    Gate --> ValidatedSetpoints

    Gate --> WriteResult

    Gate --> HealthStatus

    PlantState --> QualityCode

    WriteResult --> WriteTagResult
```

---

## Interfaces

### GatewayInterface (implementada por cada adaptador)

```
GatewayInterface
├── connect()            → Establece conexión con sistema de control
├── disconnect()         → Cierra conexión limpiamente
├── read_state()         → Retorna PlantState con mediciones y calidades
├── write_setpoints(validated_setpoints) → Escribe setpoints, retorna resultado por tag
└── health_check()       → Retorna estado de conexión y latencia
```

### Tipos de Datos

```
PlantState
├── timestamp            → Momento de la lectura
├── measurements         → Diccionario {tag: valor}
└── quality              → Diccionario {tag: código_calidad}

ValidatedSetpoints
├── setpoints            → Lista de Setpoint validados
├── timestamp            → Momento de validación
└── constraint_report    → Reporte de validación

WriteResult
├── results              → Lista de WriteTagResult
└── timestamp            → Momento de escritura

WriteTagResult
├── tag                  → Tag que se intentó escribir
├── success              → Boolean
└── error_message        → Mensaje si falló (opcional)

HealthStatus
├── connected            → Boolean
├── latency_ms           → Latencia en milisegundos (None si desconectado)
└── last_successful_read → Timestamp de última lectura exitosa

QualityCode (enum)
├── GOOD                 → Lectura exitosa, valor confiable
├── BAD                  → Lectura falló
├── UNCERTAIN            → Valor leído pero calidad dudosa
└── STALE                → Valor antiguo (de buffer)
```

---

## Historias de Usuario

### US-G01: Conectar con servidor OPC-UA

**Como** ingeniero de control  
**Quiero** configurar conexión a un servidor OPC-UA especificando endpoint y credenciales  
**Para** leer y escribir tags del sistema de control

**Criterios de aceptación:**
- [ ] Puedo especificar URL del endpoint OPC-UA
- [ ] Puedo especificar usuario y contraseña si se requiere
- [ ] El sistema intenta conectar y reporta éxito o error específico
- [ ] La conexión se mantiene activa mientras el Gateway esté corriendo

---

### US-G02: Mapear tags internos a identificadores de protocolo

**Como** ingeniero de control  
**Quiero** definir un mapeo entre nombres de tags de SimPlant y los identificadores del sistema de control  
**Para** que el Gateway sepa qué leer y escribir

**Criterios de aceptación:**
- [ ] Puedo definir mapeo en archivo de configuración
- [ ] Para OPC-UA: mapeo a NodeIds
- [ ] Para Modbus: mapeo a direcciones de registros
- [ ] El sistema valida que todos los tags requeridos tengan mapeo

---

### US-G03: Leer estado de planta

**Como** Runtime  
**Quiero** invocar una operación que retorne todas las mediciones configuradas con sus calidades  
**Para** tener el estado actual del proceso

**Criterios de aceptación:**
- [ ] Una sola llamada retorna todas las mediciones
- [ ] Cada medición incluye valor y código de calidad
- [ ] Si una lectura falla, las demás continúan
- [ ] El resultado indica claramente qué tags fallaron

---

### US-G04: Escribir setpoints validados

**Como** Runtime  
**Quiero** invocar una operación que escriba los setpoints en el sistema de control  
**Para** aplicar las decisiones del modelo

**Criterios de aceptación:**
- [ ] Recibo setpoints validados y los escribo a los tags correspondientes
- [ ] Retorno resultado de cada escritura (éxito/fallo)
- [ ] No modifico los valores recibidos bajo ninguna circunstancia
- [ ] Los errores de escritura no abortan las demás escrituras

---

### US-G05: Reconectar automáticamente

**Como** Gateway  
**Quiero** reconectar automáticamente cuando se pierde conexión  
**Para** minimizar el tiempo fuera de línea

**Criterios de aceptación:**
- [ ] Detecto pérdida de conexión (timeout, excepción)
- [ ] Intento reconectar con backoff exponencial (1s, 2s, 4s, 8s, max 60s)
- [ ] Durante desconexión, reporto estado al Runtime en cada health_check
- [ ] Al reconectar, notifico al Runtime que la conexión está disponible

---

### US-G06: Operar con adaptador de simulación

**Como** desarrollador  
**Quiero** usar un adaptador que genere datos sintéticos sin conectar a hardware real  
**Para** desarrollar y probar sin acceso a planta

**Criterios de aceptación:**
- [ ] El adaptador implementa la misma interfaz que OPC-UA/Modbus
- [ ] Genera valores que varían realísticamente en el tiempo
- [ ] Puedo configurar rangos y comportamiento de las variables simuladas
- [ ] Las escrituras se "aceptan" y afectan las lecturas siguientes

---

# Documentos relacionados

## Módulo Gateway
Este documento describe el diseño conceptual del módulo Gateway.

## Documentos del módulo
- [[Gateway]] - Documento principal (requerimientos e historias de usuario)
- [[Tecnico]] - Especificación técnica y diagramas de secuencia

---

## Diagramas de Secuencia del Sistema (DSS)

### DSS 1: Conectar

```mermaid
sequenceDiagram
    participant R as Runtime
    participant G as Gate
    participant P as Protocol
    participant A as Adapter
    participant S as Sistema Control

    R->>G: connect()
    G->>P: connect()
    P->>A: _do_connect()
    A->>S: establecer conexión
    
    alt Conexión exitosa
        S-->>A: conexión establecida
        A-->>P: true
        P->>P: _connected = true
        P->>P: _reset_backoff()
        P-->>G: true
        G-->>R: true
    else Conexión fallida
        S-->>A: error/timeout
        A-->>P: false
        P->>P: _connected = false
        P-->>G: false
        G-->>R: false
    end
```

---

### DSS 2: Leer estado de planta

```mermaid
sequenceDiagram
    participant R as Runtime
    participant G as Gate
    participant P as Protocol
    participant A as Adapter
    participant S as Sistema Control

    R->>G: read_state()
    G->>P: read_state()
    
    alt Conectado
        loop Para cada TagMapping en Config
            P->>A: _do_read_tag(tagMapping)
            A->>S: leer valor (NodeId/Register)
            
            alt Lectura exitosa
                S-->>A: valor + status
                A-->>P: TagReadResult(value, GOOD)
            else Lectura fallida
                S-->>A: error/timeout
                A-->>P: TagReadResult(null, BAD)
            end
        end
        
        P->>P: _store_in_buffer(measurements)
        P-->>G: PlantState(measurements, quality)
        G-->>R: PlantState
        
    else Desconectado
        P->>P: _get_from_buffer()
        P-->>G: PlantState(buffer, quality=STALE)
        G-->>R: PlantState (datos STALE)
        P--)P: _reconnect_with_backoff()
        Note right of P: Reconexión async en background
    end
```

---

### DSS 3: Escribir setpoints

```mermaid
sequenceDiagram
    participant R as Runtime
    participant G as Gate
    participant P as Protocol
    participant A as Adapter
    participant S as Sistema Control

    R->>G: write_setpoints(validatedSetpoints)
    G->>P: write_setpoints(validatedSetpoints)
    
    alt Conectado
        P->>P: results = []
        
        loop Para cada Setpoint en validatedSetpoints
            P->>P: tagMapping = config.get_mapping_for_tag(setpoint.tag)
            P->>A: _do_write_tag(tagMapping, setpoint.value)
            A->>S: escribir valor (NodeId/Register)
            
            alt Escritura exitosa
                S-->>A: OK
                A-->>P: true
                P->>P: results.add(WriteTagResult(tag, true, null))
            else Escritura fallida
                S-->>A: error
                A-->>P: false
                P->>P: results.add(WriteTagResult(tag, false, error_msg))
            end
        end
        
        P-->>G: WriteResult(results, timestamp)
        G-->>R: WriteResult
        
    else Desconectado
        P-->>G: WriteResult(all failed, "No connection")
        G-->>R: WriteResult (todos fallidos)
    end
```

---

### DSS 4: Health check

```mermaid
sequenceDiagram
    participant R as Runtime
    participant G as Gate
    participant P as Protocol

    R->>G: health_check()
    G->>P: health_check()
    
    P->>P: calcular latency desde _last_read_timestamp
    
    P-->>G: HealthStatus(connected, latency_ms, last_successful_read)
    G-->>R: HealthStatus
```

---

### DSS 5: Reconexión con backoff exponencial

```mermaid
sequenceDiagram
    participant P as Protocol
    participant A as Adapter
    participant S as Sistema Control

    Note over P: Detecta desconexión (_connected = false)
    
    P->>P: _reconnect_with_backoff()
    P->>P: backoff = 1s
    
    loop Mientras no conectado
        P->>P: esperar(backoff)
        P->>A: _do_connect()
        A->>S: intentar conexión
        
        alt Conexión exitosa
            S-->>A: conexión establecida
            A-->>P: true
            P->>P: _connected = true
            P->>P: _reset_backoff()
            Note over P: Fin del loop
        else Conexión fallida
            S-->>A: error/timeout
            A-->>P: false
            P->>P: backoff = min(backoff * 2, max_backoff)
            Note over P: Continúa loop con backoff aumentado
        end
    end
```


