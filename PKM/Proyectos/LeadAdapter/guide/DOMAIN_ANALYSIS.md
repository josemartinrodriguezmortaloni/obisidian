# Análisis de la Capa de Dominio - LeadAdapter

> **Propósito**: Este documento proporciona un análisis exhaustivo de la capa `src/domain/`, incluyendo diagramas de clases, diagramas de secuencia e insights arquitectónicos.

---

## Tabla de Contenidos

1. [Resumen General](#1-resumen-general)
2. [Arquitectura](#2-arquitectura)
3. [Diagrama de Clases](#3-diagrama-de-clases)
4. [Entidades](#4-entidades)
5. [Value Objects](#5-value-objects)
6. [Enums](#6-enums)
7. [Servicios de Dominio](#7-servicios-de-dominio)
8. [Excepciones](#8-excepciones)
9. [Diagramas de Secuencia](#9-diagramas-de-secuencia)
10. [Patrones de Diseño](#10-patrones-de-diseño)

---

## 1. Resumen General

La capa de dominio representa la **lógica de negocio central** de LeadAdapter, un sistema para generar mensajes personalizados de outreach para leads de ventas. Esta capa es:

- **Agnóstica de frameworks**: Sin dependencias de FastAPI, bases de datos o servicios externos
- **Python puro**: Usa solo biblioteca estándar + dataclasses
- **Alineada con DDD**: Sigue los principios de Domain-Driven Design

### Estructura de Directorios

```
src/domain/
├── entities/           # Objetos de negocio mutables con identidad
│   ├── lead.py         # Core: La persona a contactar
│   ├── message.py      # Output: Mensaje generado
│   ├── playbook.py     # Config: Playbook de ventas
│   └── sender.py       # Contexto: Quién envía el mensaje
│
├── value_objects/      # Objetos inmutables sin identidad
│   ├── campaign_history.py   # Historial de contacto del lead
│   ├── icp_profile.py        # Perfil de Cliente Ideal
│   ├── product.py            # Producto que se vende
│   └── work_experience.py    # Historial laboral del lead
│
├── enums/              # Constantes type-safe
│   ├── channel.py          # LINKEDIN | EMAIL
│   ├── message_strategy.py # TECHNICAL_PEER | BUSINESS_VALUE | ...
│   ├── seniority.py        # C_LEVEL | VP | DIRECTOR | ...
│   └── sequence_step.py    # FIRST_CONTACT | FOLLOW_UP_1 | ...
│
├── services/           # Servicios de dominio (lógica stateless)
│   ├── icp_matcher.py       # Matchea leads con ICPs
│   ├── seniority_inferrer.py # Infiere seniority desde job title
│   └── strategy_selector.py  # Selecciona estrategia de mensaje
│
└── exceptions/         # Errores específicos del dominio
    └── domain_exceptions.py  # Jerarquía de excepciones estructuradas
```

---

## 2. Arquitectura

### Flujo de Dependencias

```
┌─────────────────────────────────────────────────────────────┐
│                        Capa API                              │
│              (Routers FastAPI, middleware)                   │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Capa de Aplicación                        │
│        (Casos de Uso, DTOs, Ports, Servicios de App)         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      CAPA DE DOMINIO                         │
│                                                              │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────────────┐ │
│  │  Entidades  │  │ Value Objects│  │ Servicios de Dominio│ │
│  │  - Lead     │  │ - Product    │  │   - ICPMatcher      │ │
│  │  - Message  │  │ - ICPProfile │  │   - SeniorityInferrer│
│  │  - Playbook │  │ - WorkExp    │  │   - StrategySelector│ │
│  │  - Sender   │  │ - Campaign   │  └─────────────────────┘ │
│  └─────────────┘  └──────────────┘                          │
│                                                              │
│  ┌─────────────┐  ┌──────────────────────────────────────┐  │
│  │    Enums    │  │          Excepciones                 │  │
│  │ - Channel   │  │  - DomainException                   │  │
│  │ - Seniority │  │  - ValidationException               │  │
│  │ - Strategy  │  │  - QualityThresholdNotMetError       │  │
│  └─────────────┘  └──────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   Capa de Infraestructura                    │
│          (Adapters, APIs Externas, Bases de Datos)           │
└─────────────────────────────────────────────────────────────┘
```

### Principios Clave

| Principio | Implementación |
|-----------|----------------|
| **Las entidades tienen identidad** | `Lead`, `Message` son mutables, tienen significado más allá de sus atributos |
| **Los Value Objects son inmutables** | `Product`, `WorkExperience` usan `@dataclass(frozen=True)` |
| **Los servicios son stateless** | `ICPMatcher`, `SeniorityInferrer` no tienen estado de instancia |
| **Validación en creación** | Todas las entidades validan en `__post_init__` |
| **Excepciones de dominio ricas** | Excepciones estructuradas con `field`, `reason`, soporte RFC 7807 |

---

## 3. Diagrama de Clases

### Modelo de Dominio Completo

```mermaid
classDiagram
    direction TB
    
    %% ===== ENTIDADES =====
    class Lead {
        +str first_name
        +str job_title
        +str company_name
        +str? last_name
        +list~WorkExperience~ work_experience
        +CampaignHistory? campaign_history
        +str? bio
        +list~str~ skills
        +str? linkedin_url
        --
        +full_name: str
        +years_in_current_role() int?
        +has_previous_contact() bool
    }
    
    class Message {
        +str content
        +Channel channel
        +SequenceStep sequence_step
        +MessageStrategy strategy_used
        +float quality_score
        +str message_id
        +dict quality_breakdown
        +int tokens_used
        +int generation_time_ms
        +datetime created_at
        --
        +word_count: int
        +char_count: int
        +passes_quality_gate(threshold) bool
    }
    
    class Playbook {
        +str communication_style
        +list~Product~ products
        +list~ICPProfile~ icp_profiles
        +list~str~ success_cases
        +list~str~ common_objections
        +list~str~ value_propositions
        --
        +get_product_for_icp(icp) Product?
    }
    
    class Sender {
        +str name
        +str company_name
        +str? job_title
        +str? email
        --
        +signature: str
    }
    
    %% ===== VALUE OBJECTS =====
    class WorkExperience {
        <<frozen>>
        +str company
        +str title
        +date start_date
        +date? end_date
        +str? description
        --
        +is_current: bool
        +duration_years() int
        +duration_months() int
    }
    
    class CampaignHistory {
        <<frozen>>
        +int total_attempts
        +datetime? last_contact_date
        +str? last_channel
        +int responses_received
        +str? last_response_sentiment
        --
        +has_responded: bool
        +response_rate: float
        +days_since_last_contact() int?
    }
    
    class Product {
        <<frozen>>
        +str name
        +str description
        +list~str~ key_benefits
        +list~str~ target_problems
        +list~str~ differentiators
        --
        +get_benefit_for_pain(pain) str?
    }
    
    class ICPProfile {
        <<frozen>>
        +str name
        +list~str~ target_titles
        +list~str~ target_industries
        +tuple~int,int~ company_size_range
        +list~str~ pain_points
        +list~str~ keywords_sector
        --
        +matches_title(job_title) bool
        +get_relevant_pain_points(seniority) list~str~
    }
    
    %% ===== ENUMS =====
    class Channel {
        <<enumeration>>
        LINKEDIN
        EMAIL
        --
        +max_length: int
        +requires_subject: bool
    }
    
    class Seniority {
        <<enumeration>>
        C_LEVEL
        VP
        DIRECTOR
        MANAGER
        SENIOR
        MID
        JUNIOR
        UNKNOWN
        --
        +is_decision_maker: bool
        +is_technical: bool
    }
    
    class MessageStrategy {
        <<enumeration>>
        TECHNICAL_PEER
        BUSINESS_VALUE
        PROBLEM_SOLUTION
        SOCIAL_PROOF
        CURIOSITY_HOOK
        MUTUAL_CONNECTION
        --
        +description: str
        +for_seniority(str) list
    }
    
    class SequenceStep {
        <<enumeration>>
        FIRST_CONTACT
        FOLLOW_UP_1
        FOLLOW_UP_2
        BREAKUP
        --
        +message_tone: str
        +urgency_level: int
    }
    
    %% ===== SERVICIOS DE DOMINIO =====
    class ICPMatcher {
        +match(lead, playbook) ICPProfile?
        -_calculate_match_score(lead, icp) float
    }
    
    class SeniorityInferrer {
        +PATTERNS: list~tuple~
        +infer(job_title) Seniority
    }
    
    class StrategySelector {
        +select(lead, channel, step, playbook, seniority) MessageStrategy
    }
    
    %% ===== RELACIONES =====
    Lead "1" *-- "0..*" WorkExperience : contiene
    Lead "1" *-- "0..1" CampaignHistory : tiene
    
    Playbook "1" *-- "1..*" Product : contiene
    Playbook "1" *-- "0..*" ICPProfile : contiene
    
    Message --> Channel : usa
    Message --> SequenceStep : usa
    Message --> MessageStrategy : usa
    
    ICPProfile --> Seniority : usa para filtrar
    
    ICPMatcher --> Lead : matchea
    ICPMatcher --> Playbook : usa
    ICPMatcher --> ICPProfile : retorna
    
    SeniorityInferrer --> Seniority : retorna
    
    StrategySelector --> Lead : analiza
    StrategySelector --> Playbook : usa
    StrategySelector --> MessageStrategy : retorna
```

### Jerarquía de Excepciones

```mermaid
classDiagram
    direction TB
    
    class DomainException {
        <<abstract>>
        +HTTP_STATUS: int = 500
        +ERROR_TYPE: str
        +ERROR_TITLE: str
        +str message
        +str instance_id
        +str timestamp
        --
        +error_code: str
        +to_dict() dict
        +to_problem_detail() dict
    }
    
    class ValidationException {
        +HTTP_STATUS: int = 422
        +str field
        +str reason
    }
    
    class InvalidLeadError
    class InvalidPlaybookError
    class InvalidMessageError
    class InvalidSenderError
    class InvalidProductError
    class InvalidWorkExperienceError
    
    class QualityThresholdNotMetError {
        +HTTP_STATUS: int = 422
        +float score
        +float threshold
    }
    
    class NoMatchingICPError {
        +HTTP_STATUS: int = 404
        +dict lead_criteria
    }
    
    DomainException <|-- ValidationException
    DomainException <|-- QualityThresholdNotMetError
    DomainException <|-- NoMatchingICPError
    
    ValidationException <|-- InvalidLeadError
    ValidationException <|-- InvalidPlaybookError
    ValidationException <|-- InvalidMessageError
    ValidationException <|-- InvalidSenderError
    ValidationException <|-- InvalidProductError
    ValidationException <|-- InvalidWorkExperienceError
```

---

## 4. Entidades

### Lead (Entidad Central)

El `Lead` representa la persona a quien se enviarán los mensajes.

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| `first_name` | `str` | ✅ | Nombre del contacto |
| `job_title` | `str` | ✅ | Posición actual (usado para inferir seniority) |
| `company_name` | `str` | ✅ | Empleador actual |
| `last_name` | `str?` | ❌ | Apellido del contacto |
| `work_experience` | `list[WorkExperience]` | ❌ | Historial laboral |
| `campaign_history` | `CampaignHistory?` | ❌ | Intentos de contacto previos |
| `bio` | `str?` | ❌ | Bio de LinkedIn o resumen |
| `skills` | `list[str]` | ❌ | Skills para matching de ICP |
| `linkedin_url` | `str?` | ❌ | URL del perfil |

**Métodos Clave:**
- `full_name` → Combina nombre/apellido
- `years_in_current_role()` → Extrae de work experience
- `has_previous_contact()` → Verifica historial de campaña

### Message (Entidad de Salida)

El `Message` es la salida generada del sistema.

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| `content` | `str` | ✅ | El texto del mensaje |
| `channel` | `Channel` | ✅ | LINKEDIN o EMAIL |
| `sequence_step` | `SequenceStep` | ✅ | Posición en la secuencia |
| `strategy_used` | `MessageStrategy` | ✅ | Estrategia aplicada |
| `quality_score` | `float` | ✅ | Score 0-10 |

### Playbook (Entidad de Configuración)

El `Playbook` contiene la configuración de ventas para generar mensajes.

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| `communication_style` | `str` | ✅ | Guías de tono/estilo |
| `products` | `list[Product]` | ✅ | Productos que se venden |
| `icp_profiles` | `list[ICPProfile]` | ❌ | Perfiles de cliente objetivo |
| `success_cases` | `list[str]` | ❌ | Ejemplos de prueba social |
| `common_objections` | `list[str]` | ❌ | Manejo de objeciones |
| `value_propositions` | `list[str]` | ❌ | Propuestas de valor |

---

## 5. Value Objects

Todos los value objects son **inmutables** (`frozen=True`) y representan conceptos sin identidad.

| Value Object | Propósito | Campos Clave |
|--------------|-----------|--------------|
| `WorkExperience` | Entrada de historial laboral | company, title, dates |
| `CampaignHistory` | Seguimiento de intentos de contacto | attempts, responses, sentiment |
| `Product` | Producto que se vende | name, benefits, problems solved |
| `ICPProfile` | Perfil de Cliente Ideal | target_titles, pain_points, keywords |

### Detalle de ICPProfile

El `ICPProfile` es el value object más complejo, usado para **calificación de leads**:

```python
ICPProfile(
    name="Decision Makers Tech",
    target_titles=["CTO", "VP Engineering", "Director"],
    target_industries=["SaaS", "Fintech"],
    pain_points=["Escalar equipo de ingeniería", "Deuda técnica"],
    keywords_sector=["cloud", "devops", "microservices"]
)
```

**Métodos Clave:**
- `matches_title(job_title)` → Check rápido de elegibilidad
- `get_relevant_pain_points(seniority)` → Filtra pain points por tipo de rol


## 7. Servicios de Dominio

### SeniorityInferrer

**Propósito**: Infiere nivel de seniority desde el job title usando patrones regex.

```mermaid
flowchart TD
    A[job_title] --> B{¿Vacío?}
    B -->|Sí| C[UNKNOWN]
    B -->|No| D{¿Matchea patrón C-Level?}
    D -->|CEO/CTO/CFO| E[C_LEVEL]
    D -->|No| F{¿Matchea patrón VP?}
    F -->|VP/Vice President| G[VP]
    F -->|No| H{¿Matchea patrón Director?}
    H -->|Director/Head of| I[DIRECTOR]
    H -->|No| J{¿Matchea patrón Manager?}
    J -->|Manager/Lead| K[MANAGER]
    J -->|No| L{¿Matchea patrón Senior?}
    L -->|Sr/Senior/Staff| M[SENIOR]
    L -->|No| N{¿Matchea patrón Junior?}
    N -->|Jr/Junior/Intern| O[JUNIOR]
    N -->|No| P[MID - Default]
```

### ICPMatcher

**Propósito**: Matchea un lead con el mejor ICP usando scoring ponderado.

**Fórmula de Scoring:**
```
score = (title_score × 0.5) + (keyword_score × 0.3) + (skills_score × 0.2)
```

| Componente | Peso | Cálculo |
|------------|------|---------|
| Match de título | 50% | % de target_titles encontrados en job_title |
| Match de keywords | 30% | % de keywords_sector encontrados en job_title |
| Match de skills | 20% | % de skills que matchean keywords_sector |

**Umbral mínimo**: 0.3 (30%)

### StrategySelector

**Propósito**: Selecciona la estrategia óptima de mensaje basada en múltiples factores.

**Factores de decisión (orden de prioridad):**
1. **Contacto previo** → Usar PROBLEM_SOLUTION (prospecto conocido)
2. **Paso breakup** → Usar CURIOSITY_HOOK (último intento)
3. **Primer contacto + LinkedIn** → Usar MUTUAL_CONNECTION
4. **Basado en seniority** → Usar mapeo de `MessageStrategy.for_seniority()`

---

## 9. Diagramas de Secuencia

### Secuencia 1: Flujo de Procesamiento de Lead

Este diagrama muestra cómo se procesa un lead desde datos crudos hasta ICP matcheado:

```mermaid
sequenceDiagram
    autonumber
    participant Cliente
    participant UseCase as GenerateMessageUseCase
    participant SI as SeniorityInferrer
    participant IM as ICPMatcher
    participant SS as StrategySelector
    participant Lead
    participant Playbook
    
    Cliente->>UseCase: generate(lead_dto, playbook_dto)
    
    rect rgb(240, 248, 255)
        Note over UseCase,Lead: 1. Creación y Validación de Entidad
        UseCase->>Lead: create(first_name, job_title, ...)
        Lead-->>Lead: validación __post_init__
        alt Validación falla
            Lead-->>UseCase: raise InvalidLeadError
        end
    end
    
    rect rgb(255, 248, 240)
        Note over UseCase,SI: 2. Inferencia de Seniority
        UseCase->>SI: infer(lead.job_title)
        SI-->>SI: Matchear patrones (CEO→C_LEVEL, etc.)
        SI-->>UseCase: Seniority.SENIOR
    end
    
    rect rgb(240, 255, 240)
        Note over UseCase,IM: 3. Matching de ICP
        UseCase->>IM: match(lead, playbook)
        loop Por cada ICP en playbook
            IM-->>IM: calculate_match_score()
        end
        alt Score >= 0.3
            IM-->>UseCase: mejor_icp_matcheado
        else Sin match
            IM-->>UseCase: None
        end
    end
    
    rect rgb(255, 240, 255)
        Note over UseCase,SS: 4. Selección de Estrategia
        UseCase->>SS: select(lead, channel, step, playbook, seniority)
        SS-->>SS: Verificar contacto previo
        SS-->>SS: Verificar paso de secuencia
        SS-->>SS: Aplicar mapeo de seniority
        SS-->>UseCase: MessageStrategy.TECHNICAL_PEER
    end
    
    UseCase-->>Cliente: Contexto listo para LLM
```

### Secuencia 2: Flujo de Validación de Entidad

```mermaid
sequenceDiagram
    autonumber
    participant Codigo as Código Cliente
    participant Lead
    participant VE as ValidationException
    participant Handler as Error Handler
    
    Codigo->>Lead: Lead(first_name="", job_title="Dev", ...)
    Lead->>Lead: __post_init__()
    Lead->>Lead: verificar first_name.strip()
    
    alt first_name está vacío
        Lead->>VE: raise InvalidLeadError(field="first_name", reason="cannot be empty")
        VE-->>VE: Setear HTTP_STATUS=422
        VE-->>VE: Setear instance_id, timestamp
        VE-->>Codigo: Excepción se propaga
        
        Codigo->>Handler: catch InvalidLeadError
        Handler->>VE: e.to_problem_detail()
        VE-->>Handler: dict RFC 7807
        Handler-->>Codigo: JSONResponse (422)
    else first_name es válido
        Lead-->>Codigo: Instancia de Lead creada
    end
```

### Secuencia 3: Algoritmo de Matching de ICP

```mermaid
sequenceDiagram
    autonumber
    participant Llamador
    participant IM as ICPMatcher
    participant ICP1 as ICP: "Tech Leaders"
    participant ICP2 as ICP: "Developers"
    participant Lead
    
    Llamador->>IM: match(lead, playbook)
    
    Note over IM: Inicializar best_score=0, best_match=None
    
    rect rgb(240, 248, 255)
        Note over IM,ICP1: Evaluar ICP 1
        IM->>IM: _calculate_match_score(lead, icp1)
        IM->>Lead: leer job_title="Senior Developer"
        IM->>ICP1: leer target_titles=["CTO", "Director"]
        Note over IM: title_matches = 0
        IM->>ICP1: leer keywords_sector=["leadership"]
        Note over IM: keyword_matches = 0
        Note over IM: Score = 0.0
    end
    
    rect rgb(255, 248, 240)
        Note over IM,ICP2: Evaluar ICP 2
        IM->>IM: _calculate_match_score(lead, icp2)
        IM->>Lead: leer job_title="Senior Developer"
        IM->>ICP2: leer target_titles=["developer", "engineer"]
        Note over IM: title_matches = 1 (developer)
        Note over IM: title_score = 0.5 * (1/2) = 0.25
        IM->>ICP2: leer keywords_sector=["python", "aws"]
        Note over IM: keyword_matches = 0
        IM->>Lead: leer skills=["python", "docker"]
        Note over IM: skills_matches = 1 (python)
        Note over IM: skills_score = 0.2 * (1/2) = 0.1
        Note over IM: Score Total = 0.35
    end
    
    Note over IM: Mejor: ICP2 con score 0.35 >= 0.3
    IM-->>Llamador: return ICP: "Developers"
```

---

## 10. Patrones de Diseño

### Patrones Utilizados

| Patrón | Dónde | Por Qué |
|--------|-------|---------|
| **Value Object** | `Product`, `WorkExperience`, `ICPProfile` | Inmutabilidad, sin identidad |
| **Entity** | `Lead`, `Message`, `Playbook` | Identidad + estado mutable |
| **Domain Service** | `ICPMatcher`, `SeniorityInferrer`, `StrategySelector` | Lógica stateless cross-entity |
| **Self-Validation** | Todas las entidades en `__post_init__` | Fail-fast, objetos siempre válidos |
| **Template Method** | `DomainException.to_problem_detail()` | Comportamiento base + personalización |
| **Strategy** | Enum `MessageStrategy` | Enfoques de mensaje intercambiables |
| **Factory Method** | `MessageStrategy.for_seniority()` | Crear lista de estrategias por seniority |

### Cumplimiento SOLID

| Principio | Implementación |
|-----------|----------------|
| **SRP** | Cada clase tiene una sola razón para cambiar (Lead para datos de lead, ICPMatcher para lógica de matching) |
| **OCP** | Nuevas estrategias pueden agregarse al enum sin modificar código existente |
| **LSP** | Todas las subclases de `ValidationException` funcionan con handlers que esperan `ValidationException` |
| **ISP** | Las entidades exponen solo interfaces relevantes (Lead no conoce a Message) |
| **DIP** | Los servicios de dominio dependen de entidades/VOs, no de infraestructura |

---

## Resumen

La capa `src/domain/` es una **implementación DDD bien estructurada** con:

- **4 Entidades**: Lead, Message, Playbook, Sender
- **4 Value Objects**: WorkExperience, CampaignHistory, Product, ICPProfile
- **4 Enums**: Channel, Seniority, MessageStrategy, SequenceStep
- **3 Servicios de Dominio**: ICPMatcher, SeniorityInferrer, StrategySelector
- **10 Excepciones**: Jerárquicas, compatibles con RFC 7807

El dominio está **completamente aislado** de concerns de infraestructura, haciéndolo testeable, mantenible y agnóstico de frameworks.
