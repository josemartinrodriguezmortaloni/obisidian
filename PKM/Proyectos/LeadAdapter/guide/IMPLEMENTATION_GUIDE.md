# LeadAdapter - Guía de Implementación por Archivo

**Basado en:** Technical Design Document v3.0
**Propósito:** Documentar exactamente qué debe contener cada archivo del proyecto

---

## Tabla de Contenidos

1. [Principios de Arquitectura Hexagonal](#1-principios-de-arquitectura-hexagonal)
2. [Reglas de Dependencia](#2-reglas-de-dependencia)
3. [Domain Layer](#3-domain-layer)
4. [Application Layer](#4-application-layer)
5. [Infrastructure Layer](#5-infrastructure-layer)
6. [API Layer](#6-api-layer)
7. [Archivo Principal](#7-archivo-principal)
8. [Checklist de Implementación](#8-checklist-de-implementación)

---

## 1. Principios de Arquitectura Hexagonal

### ¿Por qué Hexagonal?

```
                    ┌─────────────────────────────────────┐
                    │           API Layer                 │
                    │   (FastAPI routers, middleware)     │
                    └──────────────┬──────────────────────┘
                                   │
                    ┌──────────────▼──────────────────────┐
                    │       Application Layer             │
                    │  (Use cases, DTOs, orchestration)   │
                    └──────────────┬──────────────────────┘
                                   │
        ┌──────────────────────────▼──────────────────────────────┐
        │                    Domain Layer                          │
        │  (Entities, Value Objects, Domain Services, Enums)       │
        │                 ⚠️ SIN DEPENDENCIAS EXTERNAS             │
        └──────────────────────────▲──────────────────────────────┘
                                   │
                    ┌──────────────┴──────────────────────┐
                    │      Infrastructure Layer           │
                    │   (OpenAI, Redis, Prompts, Config)  │
                    └─────────────────────────────────────┘
```

### Beneficios Clave

| Beneficio          | Ejemplo Práctico                               |
| ------------------ | ---------------------------------------------- |
| **Testeable**      | Testear `StrategySelector` sin llamar a OpenAI |
| **Intercambiable** | Cambiar OpenAI por Anthropic sin tocar dominio |
| **Mantenible**     | Cada capa tiene una responsabilidad clara      |

---

## 2. Reglas de Dependencia

### ✅ Dependencias Permitidas

```
API Layer           → Application Layer, Infrastructure Layer
Application Layer   → Domain Layer, Ports (interfaces)
Infrastructure Layer → Application Ports, Domain Layer
Domain Layer        → NADA EXTERNO (solo stdlib de Python)
```

### ❌ Dependencias Prohibidas

```
Domain Layer     → ❌ FastAPI, Pydantic, OpenAI, Redis
Domain Layer     → ❌ Application Layer, Infrastructure Layer
Application Layer → ❌ Infrastructure concreta (solo Ports)
```

### Regla de Oro

> **El Domain Layer es el corazón del sistema. NUNCA debe importar nada de las otras capas ni librerías externas.**

---

## 3. Domain Layer

### 3.1 Estructura

```
src/domain/
├── __init__.py
├── entities/
│   ├── __init__.py
│   ├── lead.py
│   ├── sender.py
│   ├── playbook.py
│   └── message.py
├── value_objects/
│   ├── __init__.py
│   ├── work_experience.py
│   ├── campaign_history.py
│   ├── icp_profile.py
│   └── product.py
├── enums/
│   ├── __init__.py
│   ├── channel.py
│   ├── sequence_step.py
│   └── message_strategy.py
├── services/
│   ├── __init__.py
│   ├── strategy_selector.py
│   ├── seniority_inferrer.py
│   └── icp_matcher.py
└── exceptions/
    ├── __init__.py
    └── domain_exceptions.py
```

---

### 3.2 Entities

#### `src/domain/entities/lead.py`

**Propósito:** Representa al prospecto/lead a contactar.

**Debe contener:**

- Clase `Lead` con atributos del prospecto
- Validaciones de negocio (no Pydantic)
- Métodos de dominio relacionados al lead

**Atributos requeridos:**

- `first_name: str`
- `last_name: str | None`
- `job_title: str`
- `company_name: str`
- `work_experience: list[WorkExperience]`
- `campaign_history: CampaignHistory | None`
- `bio: str | None`
- `skills: list[str]`
- `linkedin_url: str | None`

**Métodos de dominio sugeridos:**

- `full_name() -> str`
- `years_in_current_role() -> int | None`
- `is_decision_maker() -> bool` (basado en job_title)
- `has_previous_contact() -> bool`

```python
# Ejemplo de estructura
from dataclasses import dataclass, field
from typing import Optional
from domain.value_objects.work_experience import WorkExperience
from domain.value_objects.campaign_history import CampaignHistory

@dataclass
class Lead:
    first_name: str
    job_title: str
    company_name: str
    last_name: Optional[str] = None
    work_experience: list[WorkExperience] = field(default_factory=list)
    campaign_history: Optional[CampaignHistory] = None
    bio: Optional[str] = None
    skills: list[str] = field(default_factory=list)
    linkedin_url: Optional[str] = None

    def __post_init__(self) -> None:
        if not self.first_name.strip():
            raise ValueError("first_name cannot be empty")
        if not self.job_title.strip():
            raise ValueError("job_title cannot be empty")
        if not self.company_name.strip():
            raise ValueError("company_name cannot be empty")

    @property
    def full_name(self) -> str:
        if self.last_name:
            return f"{self.first_name} {self.last_name}"
        return self.first_name

    def years_in_current_role(self) -> Optional[int]:
        """Calcula años en rol actual desde work_experience."""
        if not self.work_experience:
            return None
        current = self.work_experience[0]  # Asume ordenado por fecha
        return current.duration_years()

    def has_previous_contact(self) -> bool:
        """Verifica si ya fue contactado anteriormente."""
        if not self.campaign_history:
            return False
        return self.campaign_history.total_attempts > 0
```

---

#### `src/domain/entities/sender.py`

**Propósito:** Representa al usuario que envía los mensajes.

**Debe contener:**

- Clase `Sender` con datos del remitente
- Validaciones básicas

**Atributos requeridos:**

- `name: str`
- `job_title: str | None`
- `company_name: str`
- `email: str | None`

```python
from dataclasses import dataclass
from typing import Optional

@dataclass
class Sender:
    name: str
    company_name: str
    job_title: Optional[str] = None
    email: Optional[str] = None

    def __post_init__(self) -> None:
        if not self.name.strip():
            raise ValueError("sender name cannot be empty")
        if not self.company_name.strip():
            raise ValueError("sender company_name cannot be empty")

    @property
    def signature(self) -> str:
        """Genera firma para mensajes."""
        if self.job_title:
            return f"{self.name}, {self.job_title} @ {self.company_name}"
        return f"{self.name} @ {self.company_name}"
```

---

#### `src/domain/entities/playbook.py`

**Propósito:** Configuración comercial del cliente (ICP, productos, tono).

**Debe contener:**

- Clase `Playbook` con configuración de ventas
- Relación con `ICPProfile` y `Product`

**Atributos requeridos:**

- `communication_style: str`
- `products: list[Product]`
- `icp_profiles: list[ICPProfile]`
- `success_cases: list[str]`
- `common_objections: list[str]`
- `value_propositions: list[str]`

```python
from dataclasses import dataclass, field
from typing import Optional
from domain.value_objects.product import Product
from domain.value_objects.icp_profile import ICPProfile

@dataclass
class Playbook:
    communication_style: str
    products: list[Product]
    icp_profiles: list[ICPProfile] = field(default_factory=list)
    success_cases: list[str] = field(default_factory=list)
    common_objections: list[str] = field(default_factory=list)
    value_propositions: list[str] = field(default_factory=list)

    def __post_init__(self) -> None:
        if not self.communication_style.strip():
            raise ValueError("communication_style cannot be empty")
        if not self.products:
            raise ValueError("at least one product is required")

    def get_matching_icp(self, job_title: str, industry: str) -> Optional[ICPProfile]:
        """Encuentra el ICP que mejor matchea con el lead."""
        for icp in self.icp_profiles:
            if icp.matches(job_title, industry):
                return icp
        return None

    def get_product_for_icp(self, icp: ICPProfile) -> Optional[Product]:
        """Obtiene el producto más relevante para un ICP."""
        # Lógica de matching producto-ICP
        pass
```

---

#### `src/domain/entities/message.py`

**Propósito:** Representa el mensaje generado.

**Debe contener:**

- Clase `Message` con el contenido y metadata
- Información de calidad y estrategia usada

**Atributos:**

- `message_id: str`
- `content: str`
- `channel: Channel`
- `sequence_step: SequenceStep`
- `strategy_used: MessageStrategy`
- `quality_score: float`
- `quality_breakdown: dict`
- `tokens_used: int`
- `generation_time_ms: int`
- `created_at: datetime`

```python
from dataclasses import dataclass, field
from datetime import datetime
from typing import Optional
import uuid

from domain.enums.channel import Channel
from domain.enums.sequence_step import SequenceStep
from domain.enums.message_strategy import MessageStrategy

@dataclass
class Message:
    content: str
    channel: Channel
    sequence_step: SequenceStep
    strategy_used: MessageStrategy
    quality_score: float
    message_id: str = field(default_factory=lambda: f"msg_{uuid.uuid4().hex[:12]}")
    quality_breakdown: dict = field(default_factory=dict)
    tokens_used: int = 0
    generation_time_ms: int = 0
    created_at: datetime = field(default_factory=datetime.utcnow)

    def __post_init__(self) -> None:
        if not self.content.strip():
            raise ValueError("message content cannot be empty")
        if not 0 <= self.quality_score <= 10:
            raise ValueError("quality_score must be between 0 and 10")

    def passes_quality_gate(self, threshold: float = 6.0) -> bool:
        """Verifica si el mensaje pasa el umbral de calidad."""
        return self.quality_score >= threshold

    @property
    def word_count(self) -> int:
        return len(self.content.split())

    @property
    def char_count(self) -> int:
        return len(self.content)
```

---

### 3.3 Value Objects

> **Value Objects:** Objetos inmutables que representan conceptos del dominio sin identidad propia. Se comparan por valor, no por referencia.

#### `src/domain/value_objects/work_experience.py`

**Propósito:** Representa una experiencia laboral del lead.

```python
from dataclasses import dataclass
from datetime import date
from typing import Optional

@dataclass(frozen=True)  # Inmutable
class WorkExperience:
    company: str
    title: str
    start_date: date
    end_date: Optional[date] = None  # None = trabajo actual
    description: Optional[str] = None

    def __post_init__(self) -> None:
        if not self.company.strip():
            raise ValueError("company cannot be empty")
        if not self.title.strip():
            raise ValueError("title cannot be empty")

    @property
    def is_current(self) -> bool:
        return self.end_date is None

    def duration_years(self) -> int:
        """Calcula duración en años."""
        end = self.end_date or date.today()
        delta = end - self.start_date
        return delta.days // 365

    def duration_months(self) -> int:
        """Calcula duración en meses."""
        end = self.end_date or date.today()
        delta = end - self.start_date
        return delta.days // 30
```

---

#### `src/domain/value_objects/campaign_history.py`

**Propósito:** Historial de contactos previos con el lead.

```python
from dataclasses import dataclass, field
from datetime import datetime
from typing import Optional

@dataclass(frozen=True)
class CampaignHistory:
    total_attempts: int = 0
    last_contact_date: Optional[datetime] = None
    last_channel: Optional[str] = None
    responses_received: int = 0
    last_response_sentiment: Optional[str] = None  # positive, neutral, negative

    @property
    def has_responded(self) -> bool:
        return self.responses_received > 0

    @property
    def response_rate(self) -> float:
        if self.total_attempts == 0:
            return 0.0
        return self.responses_received / self.total_attempts

    def days_since_last_contact(self) -> Optional[int]:
        if not self.last_contact_date:
            return None
        delta = datetime.utcnow() - self.last_contact_date
        return delta.days
```

---

#### `src/domain/value_objects/icp_profile.py`

**Propósito:** Define un Ideal Customer Profile.

```python
from dataclasses import dataclass, field

@dataclass(frozen=True)
class ICPProfile:
    name: str
    target_titles: list[str] = field(default_factory=list)
    target_industries: list[str] = field(default_factory=list)
    company_size_range: tuple[int, int] = (1, 10000)
    pain_points: list[str] = field(default_factory=list)
    keywords_sector: list[str] = field(default_factory=list)

    def matches(self, job_title: str, industry: str = "") -> bool:
        """Verifica si un lead matchea con este ICP."""
        title_lower = job_title.lower()

        # Match por título
        title_match = any(
            target.lower() in title_lower
            for target in self.target_titles
        )

        # Match por industria (si se proporciona)
        industry_match = True
        if industry and self.target_industries:
            industry_lower = industry.lower()
            industry_match = any(
                ind.lower() in industry_lower
                for ind in self.target_industries
            )

        return title_match and industry_match

    def get_relevant_pain_points(self, job_title: str) -> list[str]:
        """Retorna pain points relevantes según el título."""
        # Lógica para filtrar pain points según rol
        return self.pain_points
```

---

#### `src/domain/value_objects/product.py`

**Propósito:** Representa un producto/servicio a vender.

```python
from dataclasses import dataclass, field

@dataclass(frozen=True)
class Product:
    name: str
    description: str
    key_benefits: list[str] = field(default_factory=list)
    target_problems: list[str] = field(default_factory=list)
    differentiators: list[str] = field(default_factory=list)

    def __post_init__(self) -> None:
        if not self.name.strip():
            raise ValueError("product name cannot be empty")

    def get_benefit_for_pain(self, pain_point: str) -> Optional[str]:
        """Encuentra el beneficio que resuelve un pain point."""
        pain_lower = pain_point.lower()
        for i, problem in enumerate(self.target_problems):
            if pain_lower in problem.lower():
                if i < len(self.key_benefits):
                    return self.key_benefits[i]
        return self.key_benefits[0] if self.key_benefits else None
```

---

### 3.4 Enums

#### `src/domain/enums/channel.py`

**Propósito:** Canales de comunicación disponibles.

```python
from enum import Enum

class Channel(str, Enum):
    """Canales de comunicación para mensajes."""
    LINKEDIN = "linkedin"
    EMAIL = "email"

    @property
    def max_length(self) -> int:
        """Longitud máxima recomendada por canal."""
        limits = {
            Channel.LINKEDIN: 300,  # Caracteres para InMail/conexión
            Channel.EMAIL: 500,      # Más espacio en email
        }
        return limits.get(self, 300)

    @property
    def requires_subject(self) -> bool:
        """Si el canal requiere asunto."""
        return self == Channel.EMAIL
```

---

#### `src/domain/enums/sequence_step.py`

**Propósito:** Pasos en la secuencia de outreach.

```python
from enum import Enum

class SequenceStep(str, Enum):
    """Paso en la secuencia de contacto."""
    FIRST_CONTACT = "first_contact"
    FOLLOW_UP_1 = "follow_up_1"
    FOLLOW_UP_2 = "follow_up_2"
    BREAKUP = "breakup"

    @property
    def message_tone(self) -> str:
        """Tono sugerido según el paso."""
        tones = {
            SequenceStep.FIRST_CONTACT: "introductorio y curioso",
            SequenceStep.FOLLOW_UP_1: "recordatorio amigable",
            SequenceStep.FOLLOW_UP_2: "valor adicional",
            SequenceStep.BREAKUP: "última oportunidad, sin presión",
        }
        return tones.get(self, "profesional")

    @property
    def urgency_level(self) -> int:
        """Nivel de urgencia (1-4)."""
        levels = {
            SequenceStep.FIRST_CONTACT: 1,
            SequenceStep.FOLLOW_UP_1: 2,
            SequenceStep.FOLLOW_UP_2: 3,
            SequenceStep.BREAKUP: 4,
        }
        return levels.get(self, 1)
```

---

#### `src/domain/enums/message_strategy.py`

**Propósito:** Estrategias de personalización de mensajes.

```python
from enum import Enum

class MessageStrategy(str, Enum):
    """Estrategia de aproximación al lead."""
    TECHNICAL_PEER = "technical_peer"       # Dev a Dev
    BUSINESS_VALUE = "business_value"       # ROI, métricas
    PROBLEM_SOLUTION = "problem_solution"   # Pain point directo
    SOCIAL_PROOF = "social_proof"           # Casos de éxito
    CURIOSITY_HOOK = "curiosity_hook"       # Pregunta intrigante
    MUTUAL_CONNECTION = "mutual_connection" # Conexión en común

    @property
    def description(self) -> str:
        descriptions = {
            MessageStrategy.TECHNICAL_PEER: "Hablar como un par técnico, usar jerga del sector",
            MessageStrategy.BUSINESS_VALUE: "Enfocarse en ROI y métricas de negocio",
            MessageStrategy.PROBLEM_SOLUTION: "Atacar un pain point específico",
            MessageStrategy.SOCIAL_PROOF: "Usar casos de éxito y testimonios",
            MessageStrategy.CURIOSITY_HOOK: "Generar curiosidad con una pregunta",
            MessageStrategy.MUTUAL_CONNECTION: "Mencionar conexiones o contexto común",
        }
        return descriptions.get(self, "")

    @classmethod
    def for_seniority(cls, seniority: str) -> list["MessageStrategy"]:
        """Estrategias recomendadas según seniority."""
        mapping = {
            "C_LEVEL": [cls.BUSINESS_VALUE, cls.SOCIAL_PROOF],
            "VP": [cls.BUSINESS_VALUE, cls.PROBLEM_SOLUTION],
            "DIRECTOR": [cls.PROBLEM_SOLUTION, cls.BUSINESS_VALUE],
            "MANAGER": [cls.PROBLEM_SOLUTION, cls.TECHNICAL_PEER],
            "SENIOR": [cls.TECHNICAL_PEER, cls.PROBLEM_SOLUTION],
            "MID": [cls.TECHNICAL_PEER, cls.CURIOSITY_HOOK],
            "JUNIOR": [cls.CURIOSITY_HOOK, cls.TECHNICAL_PEER],
        }
        return mapping.get(seniority, [cls.PROBLEM_SOLUTION])
```

---

### 3.5 Domain Services

> **Domain Services:** Lógica de negocio que no pertenece naturalmente a una entidad. Operan sobre entidades y value objects.

#### `src/domain/services/strategy_selector.py`

**Propósito:** Selecciona la mejor estrategia de mensaje según el contexto.

**Inputs:**

- Lead (con su seniority inferido)
- Channel
- SequenceStep
- Playbook

**Output:**

- `MessageStrategy` seleccionada

```python
from domain.entities.lead import Lead
from domain.entities.playbook import Playbook
from domain.enums.channel import Channel
from domain.enums.sequence_step import SequenceStep
from domain.enums.message_strategy import MessageStrategy

class StrategySelector:
    """Selecciona la estrategia óptima de mensaje."""

    def select(
        self,
        lead: Lead,
        channel: Channel,
        sequence_step: SequenceStep,
        playbook: Playbook,
        seniority: str,
    ) -> MessageStrategy:
        """
        Selecciona estrategia basada en:
        1. Seniority del lead
        2. Canal de comunicación
        3. Paso en la secuencia
        4. Historial de contacto
        """
        # Obtener estrategias base por seniority
        candidates = MessageStrategy.for_seniority(seniority)

        # Ajustar por historial
        if lead.has_previous_contact():
            # Si ya hubo contacto, evitar repetir estrategia
            # Preferir follow-up strategies
            if MessageStrategy.PROBLEM_SOLUTION in candidates:
                return MessageStrategy.PROBLEM_SOLUTION

        # Ajustar por paso en secuencia
        if sequence_step == SequenceStep.BREAKUP:
            return MessageStrategy.CURIOSITY_HOOK  # Última oportunidad

        if sequence_step == SequenceStep.FIRST_CONTACT:
            # Primer contacto: priorizar conexión
            if channel == Channel.LINKEDIN:
                return MessageStrategy.MUTUAL_CONNECTION
            return candidates[0]

        # Default: primera estrategia candidata
        return candidates[0] if candidates else MessageStrategy.PROBLEM_SOLUTION
```

---

#### `src/domain/services/seniority_inferrer.py`

**Propósito:** Infiere el nivel de seniority desde el job_title.

```python
import re
from enum import Enum

class Seniority(str, Enum):
    C_LEVEL = "C_LEVEL"
    VP = "VP"
    DIRECTOR = "DIRECTOR"
    MANAGER = "MANAGER"
    SENIOR = "SENIOR"
    MID = "MID"
    JUNIOR = "JUNIOR"
    UNKNOWN = "UNKNOWN"

class SeniorityInferrer:
    """Infiere seniority desde job_title."""

    # Patrones ordenados por prioridad (más senior primero)
    PATTERNS = [
        (r"\b(ceo|cto|cfo|coo|cio|founder|co-founder|owner)\b", Seniority.C_LEVEL),
        (r"\b(vp|vice\s*president)\b", Seniority.VP),
        (r"\b(director|head\s+of)\b", Seniority.DIRECTOR),
        (r"\b(manager|lead|team\s*lead|tech\s*lead)\b", Seniority.MANAGER),
        (r"\b(sr\.?|senior|staff|principal)\b", Seniority.SENIOR),
        (r"\b(jr\.?|junior|entry|trainee|intern)\b", Seniority.JUNIOR),
    ]

    def infer(self, job_title: str) -> Seniority:
        """
        Infiere seniority desde el título.

        Ejemplos:
        - "CEO" → C_LEVEL
        - "Senior PHP Developer" → SENIOR
        - "Junior Data Analyst" → JUNIOR
        - "Software Engineer" → MID (default)
        """
        if not job_title:
            return Seniority.UNKNOWN

        title_lower = job_title.lower()

        for pattern, seniority in self.PATTERNS:
            if re.search(pattern, title_lower, re.IGNORECASE):
                return seniority

        # Default: MID level si no hay indicadores
        return Seniority.MID
```

---

#### `src/domain/services/icp_matcher.py`

**Propósito:** Encuentra el ICP más relevante para un lead.

```python
from typing import Optional
from domain.entities.lead import Lead
from domain.entities.playbook import Playbook
from domain.value_objects.icp_profile import ICPProfile

class ICPMatcher:
    """Encuentra el mejor ICP match para un lead."""

    def match(self, lead: Lead, playbook: Playbook) -> Optional[ICPProfile]:
        """
        Encuentra el ICP que mejor matchea con el lead.

        Criterios de matching:
        1. Job title contiene keywords del ICP
        2. Industry match (si está disponible)
        3. Score de relevancia
        """
        if not playbook.icp_profiles:
            return None

        best_match: Optional[ICPProfile] = None
        best_score = 0

        for icp in playbook.icp_profiles:
            score = self._calculate_match_score(lead, icp)
            if score > best_score:
                best_score = score
                best_match = icp

        # Umbral mínimo de matching
        if best_score < 0.3:
            return None

        return best_match

    def _calculate_match_score(self, lead: Lead, icp: ICPProfile) -> float:
        """Calcula score de matching (0-1)."""
        score = 0.0
        title_lower = lead.job_title.lower()

        # Match por título (peso: 0.5)
        title_matches = sum(
            1 for keyword in icp.target_titles
            if keyword.lower() in title_lower
        )
        if icp.target_titles:
            score += 0.5 * (title_matches / len(icp.target_titles))

        # Match por keywords de sector (peso: 0.3)
        keyword_matches = sum(
            1 for keyword in icp.keywords_sector
            if keyword.lower() in title_lower
        )
        if icp.keywords_sector:
            score += 0.3 * (keyword_matches / len(icp.keywords_sector))

        # Match por skills del lead (peso: 0.2)
        if lead.skills and icp.keywords_sector:
            skill_matches = sum(
                1 for skill in lead.skills
                if any(kw.lower() in skill.lower() for kw in icp.keywords_sector)
            )
            score += 0.2 * (skill_matches / len(lead.skills))

        return min(score, 1.0)
```

---

### 3.6 Domain Exceptions

#### `src/domain/exceptions/domain_exceptions.py`

**Propósito:** Excepciones específicas del dominio.

```python
class DomainException(Exception):
    """Base exception for domain errors."""
    pass

class InvalidLeadError(DomainException):
    """Lead data is invalid or incomplete."""
    pass

class InvalidPlaybookError(DomainException):
    """Playbook configuration is invalid."""
    pass

class NoMatchingICPError(DomainException):
    """No ICP matches the given lead."""
    pass

class InvalidMessageError(DomainException):
    """Generated message is invalid."""
    pass

class QualityThresholdNotMetError(DomainException):
    """Message quality score below threshold."""
    def __init__(self, score: float, threshold: float, message: str = ""):
        self.score = score
        self.threshold = threshold
        super().__init__(
            f"Quality score {score} below threshold {threshold}. {message}"
        )
```

---

## 4. Application Layer

### 4.1 Estructura

```
src/application/
├── __init__.py
├── ports/
│   ├── __init__.py
│   ├── llm_port.py
│   └── cache_port.py
├── mappers/                          # ✅ OPTIMIZACIÓN: SRP
│   ├── __init__.py
│   └── entity_mapper.py
├── use_cases/
│   ├── __init__.py
│   └── generate_message.py
├── dtos/
│   ├── __init__.py
│   ├── requests.py
│   └── responses.py
└── services/
    ├── __init__.py
    ├── prompt_chain_orchestrator.py
    ├── message_scorer.py
    ├── quality_gate.py
    └── scoring/                      # ✅ OPTIMIZACIÓN: Strategy Pattern
        ├── __init__.py
        ├── scoring_criterion.py
        ├── personalization_criterion.py
        ├── anti_spam_criterion.py
        ├── structure_criterion.py
        └── tone_criterion.py
```

---

### 4.2 Ports (Interfaces)

> **Ports:** Interfaces abstractas que definen contratos. La implementación concreta está en Infrastructure.

#### `src/application/ports/llm_port.py`

**Propósito:** Contrato para interactuar con LLMs.

```python
from abc import ABC, abstractmethod
from typing import Any

class LLMPort(ABC):
    """Interface for LLM interactions."""

    @abstractmethod
    async def complete(
        self,
        prompt: str,
        system_prompt: str | None = None,
        temperature: float = 0.7,
        max_tokens: int = 500,
    ) -> str:
        """
        Genera una completion desde el LLM.

        Args:
            prompt: Prompt del usuario
            system_prompt: Prompt de sistema (opcional)
            temperature: Creatividad (0-1)
            max_tokens: Límite de tokens de respuesta

        Returns:
            Texto generado por el LLM
        """
        pass

    @abstractmethod
    async def complete_json(
        self,
        prompt: str,
        system_prompt: str | None = None,
        temperature: float = 0.3,
    ) -> dict[str, Any]:
        """
        Genera una completion en formato JSON.

        Returns:
            Diccionario parseado desde la respuesta JSON
        """
        pass

    @abstractmethod
    def count_tokens(self, text: str) -> int:
        """Cuenta tokens en un texto."""
        pass
```

---

#### `src/application/ports/cache_port.py`

**Propósito:** Contrato para caching.

```python
from abc import ABC, abstractmethod
from typing import Any, Optional

class CachePort(ABC):
    """Interface for caching operations."""

    @abstractmethod
    async def get(self, key: str) -> Optional[Any]:
        """
        Obtiene valor del cache.

        Args:
            key: Clave de cache

        Returns:
            Valor cacheado o None si no existe
        """
        pass

    @abstractmethod
    async def set(
        self,
        key: str,
        value: Any,
        ttl_seconds: int = 3600,
    ) -> None:
        """
        Guarda valor en cache.

        Args:
            key: Clave de cache
            value: Valor a cachear
            ttl_seconds: Tiempo de vida en segundos
        """
        pass

    @abstractmethod
    async def delete(self, key: str) -> None:
        """Elimina valor del cache."""
        pass

    @abstractmethod
    async def exists(self, key: str) -> bool:
        """Verifica si existe una clave."""
        pass
```

<!-- 
✅ OPTIMIZACIÓN APLICADA: cache_port.py
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Se agregó `LLMResponse` dataclass con campo `model` para obtener el modelo usado dinámicamente
- Se agregó método `get_or_set()` para simplificar el patrón cache-aside
- Motivo: El model_used estaba hardcodeado en el use case; ahora viene del adapter
-->

---

### 4.3 DTOs (Data Transfer Objects)

> **DTOs:** Schemas de Pydantic para validar entrada/salida de la API. NO confundir con entidades de dominio.

#### `src/application/dtos/requests.py`

**Propósito:** Schemas de request para la API.

```python
from pydantic import BaseModel, Field, field_validator
from typing import Optional
from datetime import date, datetime

# ============ Nested DTOs ============

class WorkExperienceDTO(BaseModel):
    company: str
    title: str
    start_date: date
    end_date: Optional[date] = None
    description: Optional[str] = None

class CampaignHistoryDTO(BaseModel):
    total_attempts: int = 0
    last_contact_date: Optional[datetime] = None
    last_channel: Optional[str] = None
    responses_received: int = 0

class ProductDTO(BaseModel):
    name: str
    description: str
    key_benefits: list[str] = Field(default_factory=list)
    target_problems: list[str] = Field(default_factory=list)

class ICPProfileDTO(BaseModel):
    name: str
    target_titles: list[str] = Field(default_factory=list)
    target_industries: list[str] = Field(default_factory=list)
    pain_points: list[str] = Field(default_factory=list)
    keywords_sector: list[str] = Field(default_factory=list)

# ============ Main Request DTOs ============

class LeadDTO(BaseModel):
    first_name: str = Field(..., min_length=1)
    last_name: Optional[str] = None
    job_title: str = Field(..., min_length=1)
    company_name: str = Field(..., min_length=1)
    work_experience: list[WorkExperienceDTO] = Field(default_factory=list)
    campaign_history: Optional[CampaignHistoryDTO] = None
    bio: Optional[str] = None
    skills: list[str] = Field(default_factory=list)
    linkedin_url: Optional[str] = None

    @field_validator("first_name", "job_title", "company_name")
    @classmethod
    def not_empty(cls, v: str) -> str:
        if not v.strip():
            raise ValueError("Field cannot be empty")
        return v.strip()

class SenderDTO(BaseModel):
    name: str = Field(..., min_length=1)
    company_name: str = Field(..., min_length=1)
    job_title: Optional[str] = None
    email: Optional[str] = None

class PlaybookDTO(BaseModel):
    communication_style: str = Field(..., min_length=1)
    products: list[ProductDTO] = Field(..., min_length=1)
    icp_profiles: list[ICPProfileDTO] = Field(default_factory=list)
    success_cases: list[str] = Field(default_factory=list)
    common_objections: list[str] = Field(default_factory=list)
    value_propositions: list[str] = Field(default_factory=list)

class GenerateMessageRequest(BaseModel):
    """Request para generar un mensaje."""
    channel: str = Field(..., pattern="^(linkedin|email)$")
    sequence_step: str = Field(
        ...,
        pattern="^(first_contact|follow_up_1|follow_up_2|breakup)$"
    )
    lead: LeadDTO
    sender: SenderDTO
    playbook: PlaybookDTO

    model_config = {
        "json_schema_extra": {
            "example": {
                "channel": "linkedin",
                "sequence_step": "first_contact",
                "lead": {
                    "first_name": "Mateo",
                    "last_name": "Gutierrez",
                    "job_title": "Senior PHP Developer",
                    "company_name": "Tecnocom"
                },
                "sender": {
                    "name": "Fernando",
                    "company_name": "Synapsale",
                    "job_title": "CTO"
                },
                "playbook": {
                    "communication_style": "B2B directo",
                    "products": [
                        {
                            "name": "DevPlatform",
                            "description": "Plataforma de desarrollo",
                            "key_benefits": ["Velocidad", "Calidad"]
                        }
                    ]
                }
            }
        }
    }
```

---

#### `src/application/dtos/responses.py`

**Propósito:** Schemas de response para la API.

```python
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime

class QualityBreakdownDTO(BaseModel):
    personalization: float = Field(..., ge=0, le=3)
    anti_spam: float = Field(..., ge=0, le=3)
    structure: float = Field(..., ge=0, le=2)
    tone: float = Field(..., ge=0, le=2)

class QualityDTO(BaseModel):
    score: float = Field(..., ge=0, le=10)
    breakdown: QualityBreakdownDTO
    passes_threshold: bool

class MetadataDTO(BaseModel):
    tokens_used: int
    generation_time_ms: int
    model_used: str
    attempts: int = 1

class GenerateMessageResponse(BaseModel):
    """Response de generación de mensaje."""
    message_id: str
    content: str
    quality: QualityDTO
    strategy_used: str
    metadata: MetadataDTO
    created_at: datetime = Field(default_factory=datetime.utcnow)

    model_config = {
        "json_schema_extra": {
            "example": {
                "message_id": "msg_abc123def456",
                "content": "Hola Mateo, vi que llevas más de 6 años...",
                "quality": {
                    "score": 7.5,
                    "breakdown": {
                        "personalization": 2.5,
                        "anti_spam": 2.0,
                        "structure": 1.5,
                        "tone": 1.5
                    },
                    "passes_threshold": True
                },
                "strategy_used": "TECHNICAL_PEER",
                "metadata": {
                    "tokens_used": 450,
                    "generation_time_ms": 2847,
                    "model_used": "gpt-4o-mini",
                    "attempts": 1
                }
            }
        }
    }

class ErrorResponse(BaseModel):
    """Response de error."""
    error: str
    detail: Optional[str] = None
    code: str

class HealthResponse(BaseModel):
    """Response de health check."""
    status: str = "healthy"
    version: str
    timestamp: datetime = Field(default_factory=datetime.utcnow)
```

---

### 4.4 Mappers

> **Mappers:** Clases dedicadas a convertir DTOs a entidades de dominio, siguiendo el patrón Data Mapper y respetando SRP.

#### `src/application/mappers/entity_mapper.py`

**Propósito:** Centraliza la conversión de DTOs a entidades de dominio.

<!-- 
✅ OPTIMIZACIÓN APLICADA: entity_mapper.py (NUEVO)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Extraído del use case para cumplir SRP
- Métodos: to_lead(), to_sender(), to_playbook()
- Motivo: El use case tenía responsabilidad doble (mapear + orquestar)
- Facilita testing independiente del mapeo
-->

```python
from dataclasses import dataclass
from application.dtos.requests import LeadDTO, SenderDTO, PlaybookDTO
from domain.entities.lead import Lead
from domain.entities.sender import Sender
from domain.entities.playbook import Playbook
from domain.value_objects.work_experience import WorkExperience
from domain.value_objects.campaign_history import CampaignHistory
from domain.value_objects.icp_profile import ICPProfile
from domain.value_objects.product import Product


@dataclass
class EntityMapper:
    """
    Mapper para conversión bidireccional entre DTOs y entidades.
    
    Centraliza la lógica de transformación siguiendo el patrón
    Data Mapper, manteniendo las entidades de dominio libres de
    conocimiento sobre la capa de aplicación.
    """

    def to_lead(self, dto: LeadDTO) -> Lead:
        """Convierte LeadDTO a entidad de dominio Lead."""
        work_experience = [
            WorkExperience(
                company=exp.company,
                title=exp.title,
                start_date=exp.start_date,
                end_date=exp.end_date,
                description=exp.description,
            )
            for exp in dto.work_experience
        ]

        campaign_history = None
        if dto.campaign_history:
            campaign_history = CampaignHistory(
                total_attempts=dto.campaign_history.total_attempts,
                last_contact_date=dto.campaign_history.last_contact_date,
                last_channel=dto.campaign_history.last_channel,
                responses_received=dto.campaign_history.responses_received,
            )

        return Lead(
            first_name=dto.first_name,
            last_name=dto.last_name,
            job_title=dto.job_title,
            company_name=dto.company_name,
            work_experience=work_experience,
            campaign_history=campaign_history,
            bio=dto.bio,
            skills=list(dto.skills),
            linkedin_url=dto.linkedin_url,
        )

    def to_sender(self, dto: SenderDTO) -> Sender:
        """Convierte SenderDTO a entidad de dominio Sender."""
        return Sender(
            name=dto.name,
            company_name=dto.company_name,
            job_title=dto.job_title,
            email=dto.email,
        )

    def to_playbook(self, dto: PlaybookDTO) -> Playbook:
        """Convierte PlaybookDTO a entidad de dominio Playbook."""
        products = [
            Product(
                name=prod.name,
                description=prod.description,
                key_benefits=list(prod.key_benefits),
                target_problems=list(prod.target_problems),
            )
            for prod in dto.products
        ]

        icp_profiles = [
            ICPProfile(
                name=icp.name,
                target_titles=list(icp.target_titles),
                target_industries=list(icp.target_industries),
                pain_points=list(icp.pain_points),
                keywords_sector=list(icp.keywords_sector),
            )
            for icp in dto.icp_profiles
        ]

        return Playbook(
            communication_style=dto.communication_style,
            products=products,
            icp_profiles=icp_profiles,
            success_cases=list(dto.success_cases),
            common_objections=list(dto.common_objections),
            value_propositions=list(dto.value_propositions),
        )
```

---

### 4.5 Use Cases

#### `src/application/use_cases/generate_message.py`

**Propósito:** Orquesta la generación de un mensaje completo.

**Responsabilidades:**

1. Convertir DTOs a entidades de dominio
2. Invocar servicios de dominio (StrategySelector, SeniorityInferrer)
3. Orquestar el prompt chain
4. Aplicar Quality Gate
5. Retornar response

```python
from dataclasses import dataclass
from datetime import datetime
import time

from application.ports.llm_port import LLMPort
from application.ports.cache_port import CachePort
from application.dtos.requests import GenerateMessageRequest
from application.dtos.responses import (
    GenerateMessageResponse,
    QualityDTO,
    QualityBreakdownDTO,
    MetadataDTO,
)
from application.services.prompt_chain_orchestrator import PromptChainOrchestrator
from application.services.quality_gate import QualityGate

from domain.entities.lead import Lead
from domain.entities.sender import Sender
from domain.entities.playbook import Playbook
from domain.entities.message import Message
from domain.enums.channel import Channel
from domain.enums.sequence_step import SequenceStep
from domain.services.strategy_selector import StrategySelector
from domain.services.seniority_inferrer import SeniorityInferrer
from domain.services.icp_matcher import ICPMatcher

@dataclass
class GenerateMessageUseCase:
    """Caso de uso: Generar mensaje personalizado."""

    llm: LLMPort
    cache: CachePort
    prompt_orchestrator: PromptChainOrchestrator
    quality_gate: QualityGate
    strategy_selector: StrategySelector
    seniority_inferrer: SeniorityInferrer
    icp_matcher: ICPMatcher

    async def execute(self, request: GenerateMessageRequest) -> GenerateMessageResponse:
        """
        Ejecuta la generación de mensaje.

        Pipeline:
        1. Convertir DTOs → Domain entities
        2. Inferir seniority
        3. Seleccionar estrategia
        4. Ejecutar prompt chain
        5. Aplicar quality gate
        6. Retornar response
        """
        start_time = time.time()

        # 1. Convertir a entidades de dominio
        lead = self._to_lead_entity(request.lead)
        sender = self._to_sender_entity(request.sender)
        playbook = self._to_playbook_entity(request.playbook)
        channel = Channel(request.channel)
        sequence_step = SequenceStep(request.sequence_step)

        # 2. Inferir seniority
        seniority = self.seniority_inferrer.infer(lead.job_title)

        # 3. Match ICP
        matched_icp = self.icp_matcher.match(lead, playbook)

        # 4. Seleccionar estrategia
        strategy = self.strategy_selector.select(
            lead=lead,
            channel=channel,
            sequence_step=sequence_step,
            playbook=playbook,
            seniority=seniority.value,
        )

        # 5. Ejecutar prompt chain con quality gate
        message, attempts = await self.quality_gate.generate_with_retry(
            lead=lead,
            sender=sender,
            playbook=playbook,
            channel=channel,
            sequence_step=sequence_step,
            strategy=strategy,
            matched_icp=matched_icp,
            seniority=seniority,
        )

        # 6. Construir response
        generation_time_ms = int((time.time() - start_time) * 1000)

        return GenerateMessageResponse(
            message_id=message.message_id,
            content=message.content,
            quality=QualityDTO(
                score=message.quality_score,
                breakdown=QualityBreakdownDTO(**message.quality_breakdown),
                passes_threshold=message.passes_quality_gate(),
            ),
            strategy_used=strategy.value,
            metadata=MetadataDTO(
                tokens_used=message.tokens_used,
                generation_time_ms=generation_time_ms,
                model_used="gpt-4o-mini",
                attempts=attempts,
            ),
        )

    def _to_lead_entity(self, dto) -> Lead:
        """Convierte LeadDTO a Lead entity."""
        # Implementar conversión
        pass

    def _to_sender_entity(self, dto) -> Sender:
        """Convierte SenderDTO a Sender entity."""
        pass

    def _to_playbook_entity(self, dto) -> Playbook:
        """Convierte PlaybookDTO a Playbook entity."""
        pass
```

<!-- 
✅ OPTIMIZACIÓN APLICADA: generate_message.py
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. Cache-aside pattern implementado:
   - Verifica cache antes de generar (`await self.cache.get(cache_key)`)
   - Almacena respuestas exitosas (`await self.cache.set(cache_key, ...)`)
   - TTL de 1 hora para resultados cacheados
   - Motivo: El cache se inyectaba pero NO se usaba

2. EntityMapper inyectado (SRP):
   - Se extrajo lógica de mapeo a `application/mappers/entity_mapper.py`
   - Use case ahora usa `self.entity_mapper.to_lead(...)` etc.
   - Motivo: Use case tenía responsabilidad de mapeo + orquestación (violaba SRP)

3. Logging estructurado agregado:
   - Logs de inicio, cache hit/miss, contexto resuelto, éxito
   - Motivo: Observabilidad para debugging y monitoreo
-->

---

### 4.6 Application Services

#### `src/application/services/prompt_chain_orchestrator.py`

**Propósito:** Orquesta el pipeline de prompts (prompt chaining).

```python
from dataclasses import dataclass
from typing import Optional

from application.ports.llm_port import LLMPort
from domain.entities.lead import Lead
from domain.entities.sender import Sender
from domain.entities.playbook import Playbook
from domain.enums.channel import Channel
from domain.enums.sequence_step import SequenceStep
from domain.enums.message_strategy import MessageStrategy
from domain.value_objects.icp_profile import ICPProfile

@dataclass
class LeadClassification:
    role_type: str  # decision_maker, influencer, end_user, gatekeeper
    confidence: float

@dataclass
class InferredContext:
    pain_points: list[str]
    hooks: list[str]
    talking_points: list[str]

@dataclass
class PromptChainOrchestrator:
    """Orquesta el pipeline de prompt chaining."""

    llm: LLMPort

    async def execute_chain(
        self,
        lead: Lead,
        sender: Sender,
        playbook: Playbook,
        channel: Channel,
        sequence_step: SequenceStep,
        strategy: MessageStrategy,
        matched_icp: Optional[ICPProfile],
        seniority: str,
    ) -> tuple[str, int]:
        """
        Ejecuta el pipeline de 3 pasos:

        Step 1: Clasificar Lead (~100 tokens)
        Step 2: Inferir Pain Points (~150 tokens)
        Step 3: Generar Mensaje (~500 tokens)

        Returns:
            tuple[str, int]: (mensaje_generado, tokens_totales)
        """
        total_tokens = 0

        # Step 1: Clasificar lead
        classification = await self._classify_lead(lead)
        total_tokens += 100  # Aproximado

        # Step 2: Inferir contexto
        context = await self._infer_context(
            lead=lead,
            classification=classification,
            playbook=playbook,
            matched_icp=matched_icp,
        )
        total_tokens += 150

        # Step 3: Generar mensaje
        message_content = await self._generate_message(
            lead=lead,
            sender=sender,
            playbook=playbook,
            channel=channel,
            sequence_step=sequence_step,
            strategy=strategy,
            context=context,
            seniority=seniority,
        )
        total_tokens += 500

        return message_content, total_tokens

    async def _classify_lead(self, lead: Lead) -> LeadClassification:
        """Step 1: Clasifica el rol del lead."""
        # Llamar a LLM con prompt de clasificación
        pass

    async def _infer_context(
        self,
        lead: Lead,
        classification: LeadClassification,
        playbook: Playbook,
        matched_icp: Optional[ICPProfile],
    ) -> InferredContext:
        """Step 2: Infiere pain points y hooks."""
        # Llamar a LLM con prompt de inferencia
        pass

    async def _generate_message(
        self,
        lead: Lead,
        sender: Sender,
        playbook: Playbook,
        channel: Channel,
        sequence_step: SequenceStep,
        strategy: MessageStrategy,
        context: InferredContext,
        seniority: str,
    ) -> str:
        """Step 3: Genera el mensaje final."""
        # Llamar a LLM con prompt de generación
        pass
```

---

#### `src/application/services/message_scorer.py`

**Propósito:** Calcula el score de calidad de un mensaje.

```python
from dataclasses import dataclass
import re

from domain.entities.lead import Lead
from domain.entities.message import Message

@dataclass
class ScoreBreakdown:
    personalization: float  # 0-3 puntos
    anti_spam: float        # 0-3 puntos
    structure: float        # 0-2 puntos
    tone: float             # 0-2 puntos

    @property
    def total(self) -> float:
        return self.personalization + self.anti_spam + self.structure + self.tone

class MessageScorer:
    """Calcula score de calidad del mensaje."""

    # Frases spam a penalizar
    SPAM_PHRASES = [
        "revolucionar",
        "game changer",
        "best in class",
        "líder del mercado",
        "solución integral",
        "no te arrepentirás",
        "oferta limitada",
        "actúa ahora",
    ]

    def score(self, message_content: str, lead: Lead) -> ScoreBreakdown:
        """
        Calcula score de calidad (0-10 puntos).

        Criterios:
        - Personalización (3 pts): nombre, empresa, contexto específico
        - Anti-spam (3 pts): penaliza frases genéricas
        - Estructura (2 pts): hook, propuesta valor, CTA
        - Tono (2 pts): match con seniority y contexto
        """
        return ScoreBreakdown(
            personalization=self._score_personalization(message_content, lead),
            anti_spam=self._score_anti_spam(message_content),
            structure=self._score_structure(message_content),
            tone=self._score_tone(message_content, lead),
        )

    def _score_personalization(self, content: str, lead: Lead) -> float:
        """Evalúa nivel de personalización (0-3)."""
        score = 0.0
        content_lower = content.lower()

        # +1 si menciona nombre
        if lead.first_name.lower() in content_lower:
            score += 1.0

        # +1 si menciona empresa
        if lead.company_name.lower() in content_lower:
            score += 1.0

        # +1 si hay contexto específico (años experiencia, tecnología, etc)
        specific_patterns = [
            r"\d+\s*(años|years)",  # Menciona años
            r"(vi que|noté que|me llamó la atención)",  # Referencias específicas
            lead.job_title.split()[0].lower(),  # Menciona parte del título
        ]
        for pattern in specific_patterns:
            if re.search(pattern, content_lower):
                score += 0.33

        return min(score, 3.0)

    def _score_anti_spam(self, content: str) -> float:
        """Evalúa ausencia de spam (0-3). Empieza en 3 y resta."""
        score = 3.0
        content_lower = content.lower()

        for phrase in self.SPAM_PHRASES:
            if phrase.lower() in content_lower:
                score -= 0.5

        return max(score, 0.0)

    def _score_structure(self, content: str) -> float:
        """Evalúa estructura del mensaje (0-2)."""
        score = 0.0

        # Tiene hook/apertura (primeras palabras enganchan)
        if any(content.lower().startswith(h) for h in ["hola", "hi", "hey"]):
            score += 0.5

        # Tiene propuesta de valor (menciona beneficio)
        value_keywords = ["ayudar", "mejorar", "optimizar", "help", "improve"]
        if any(kw in content.lower() for kw in value_keywords):
            score += 0.75

        # Tiene CTA (llamada a la acción)
        cta_patterns = [
            r"\?$",  # Termina en pregunta
            r"(hablamos|conectamos|charlamos|chat|call)",
            r"(te parece|qué opinas|interesa)",
        ]
        if any(re.search(p, content.lower()) for p in cta_patterns):
            score += 0.75

        return min(score, 2.0)

    def _score_tone(self, content: str, lead: Lead) -> float:
        """Evalúa tono apropiado (0-2)."""
        # Implementación simplificada
        score = 1.0  # Base

        # Longitud apropiada
        word_count = len(content.split())
        if 50 <= word_count <= 150:
            score += 0.5

        # No es demasiado formal ni informal
        formal_markers = ["estimado", "distinguido", "por medio de la presente"]
        informal_markers = ["bro", "crack", "genio"]

        content_lower = content.lower()
        if not any(m in content_lower for m in formal_markers + informal_markers):
            score += 0.5

        return min(score, 2.0)
```

<!-- 
✅ OPTIMIZACIÓN APLICADA: message_scorer.py + scoring/
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. Strategy Pattern implementado:
   - Creada carpeta `services/scoring/` con criterios pluggables
   - Abstract base: `ScoringCriterion` con propiedades `name`, `max_score` y método `score()`
   - Implementaciones: PersonalizationCriterion, AntiSpamCriterion, StructureCriterion, ToneCriterion
   - MessageScorer ahora recibe `list[ScoringCriterion]` en constructor
   - Motivo: Criterios estaban hardcodeados, violando OCP

2. Beneficios:
   - OCP: Agregar nuevos criterios sin modificar MessageScorer
   - SRP: Cada criterio en su propia clase
   - DIP: MessageScorer depende de abstracción ScoringCriterion
   - Testeable: Criterios se pueden testear individualmente

3. Uso:
   ```python
   # Default (backward compatible)
   scorer = MessageScorer()
   
   # Customizado
   scorer = MessageScorer(criteria=[PersonalizationCriterion(), AntiSpamCriterion()])
   
   # Extendido
   class UrgencyCriterion(ScoringCriterion): ...
   scorer = MessageScorer(criteria=create_default_criteria() + [UrgencyCriterion()])
   ```
-->

---

#### `src/application/services/quality_gate.py`

**Propósito:** Controla la calidad y regenera si es necesario.

```python
from dataclasses import dataclass
from typing import Optional
import logging

from application.services.prompt_chain_orchestrator import PromptChainOrchestrator
from application.services.message_scorer import MessageScorer
from domain.entities.lead import Lead
from domain.entities.sender import Sender
from domain.entities.playbook import Playbook
from domain.entities.message import Message
from domain.enums.channel import Channel
from domain.enums.sequence_step import SequenceStep
from domain.enums.message_strategy import MessageStrategy
from domain.value_objects.icp_profile import ICPProfile
from domain.exceptions.domain_exceptions import QualityThresholdNotMetError

logger = logging.getLogger(__name__)

@dataclass
class QualityGate:
    """Controla calidad de mensajes y regenera si es necesario."""

    orchestrator: PromptChainOrchestrator
    scorer: MessageScorer
    threshold: float = 6.0
    max_attempts: int = 3

    async def generate_with_retry(
        self,
        lead: Lead,
        sender: Sender,
        playbook: Playbook,
        channel: Channel,
        sequence_step: SequenceStep,
        strategy: MessageStrategy,
        matched_icp: Optional[ICPProfile],
        seniority: str,
    ) -> tuple[Message, int]:
        """
        Genera mensaje con reintentos si no pasa quality gate.

        Returns:
            tuple[Message, int]: (mensaje, número_de_intentos)

        Raises:
            QualityThresholdNotMetError: Si después de max_attempts no se logra
        """
        attempts = 0
        best_message: Optional[Message] = None
        best_score = 0.0

        while attempts < self.max_attempts:
            attempts += 1

            # Generar mensaje
            content, tokens = await self.orchestrator.execute_chain(
                lead=lead,
                sender=sender,
                playbook=playbook,
                channel=channel,
                sequence_step=sequence_step,
                strategy=strategy,
                matched_icp=matched_icp,
                seniority=seniority,
            )

            # Calcular score
            score_breakdown = self.scorer.score(content, lead)
            total_score = score_breakdown.total

            # Crear mensaje
            message = Message(
                content=content,
                channel=channel,
                sequence_step=sequence_step,
                strategy_used=strategy,
                quality_score=total_score,
                quality_breakdown={
                    "personalization": score_breakdown.personalization,
                    "anti_spam": score_breakdown.anti_spam,
                    "structure": score_breakdown.structure,
                    "tone": score_breakdown.tone,
                },
                tokens_used=tokens,
            )

            # Guardar mejor intento
            if total_score > best_score:
                best_score = total_score
                best_message = message

            # Si pasa threshold, retornar
            if message.passes_quality_gate(self.threshold):
                logger.info(f"Message passed quality gate on attempt {attempts}")
                return message, attempts

            logger.warning(
                f"Attempt {attempts}: score {total_score} below threshold {self.threshold}"
            )

        # Si llegamos aquí, retornar el mejor intento con warning
        if best_message:
            logger.warning(
                f"Returning best effort message with score {best_score} "
                f"after {self.max_attempts} attempts"
            )
            return best_message, attempts

        raise QualityThresholdNotMetError(
            score=best_score,
            threshold=self.threshold,
            message="Could not generate message meeting quality standards"
        )
```

---

## 5. Infrastructure Layer

### 5.1 Estructura

```
src/infrastructure/
├── __init__.py
├── adapters/
│   ├── __init__.py
│   ├── openai_adapter.py
│   └── memory_cache_adapter.py
├── prompts/
│   ├── __init__.py
│   ├── classify_lead.py
│   ├── infer_context.py
│   └── generate_message.py
└── config/
    ├── __init__.py
    └── settings.py
```

---

### 5.2 Adapters

#### `src/infrastructure/adapters/openai_adapter.py`

**Propósito:** Implementación concreta de LLMPort usando OpenAI.

```python
import json
from typing import Any
import openai
from openai import AsyncOpenAI

from application.ports.llm_port import LLMPort
from infrastructure.config.settings import Settings

class OpenAIAdapter(LLMPort):
    """Implementación de LLMPort usando OpenAI API."""

    def __init__(self, settings: Settings):
        self.client = AsyncOpenAI(api_key=settings.openai_api_key)
        self.model = settings.openai_model
        self.settings = settings

    async def complete(
        self,
        prompt: str,
        system_prompt: str | None = None,
        temperature: float = 0.7,
        max_tokens: int = 500,
    ) -> str:
        """Genera completion desde OpenAI."""
        messages = []

        if system_prompt:
            messages.append({"role": "system", "content": system_prompt})

        messages.append({"role": "user", "content": prompt})

        response = await self.client.chat.completions.create(
            model=self.model,
            messages=messages,
            temperature=temperature,
            max_tokens=max_tokens,
        )

        return response.choices[0].message.content or ""

    async def complete_json(
        self,
        prompt: str,
        system_prompt: str | None = None,
        temperature: float = 0.3,
    ) -> dict[str, Any]:
        """Genera completion en formato JSON."""
        messages = []

        json_system = (system_prompt or "") + "\nRespond only with valid JSON."
        messages.append({"role": "system", "content": json_system})
        messages.append({"role": "user", "content": prompt})

        response = await self.client.chat.completions.create(
            model=self.model,
            messages=messages,
            temperature=temperature,
            response_format={"type": "json_object"},
        )

        content = response.choices[0].message.content or "{}"
        return json.loads(content)

    def count_tokens(self, text: str) -> int:
        """Cuenta tokens (aproximación simple)."""
        # Aproximación: ~4 caracteres por token
        return len(text) // 4
```

---

#### `src/infrastructure/adapters/memory_cache_adapter.py`

**Propósito:** Implementación de CachePort en memoria (para desarrollo).

```python
from typing import Any, Optional
from datetime import datetime, timedelta
from dataclasses import dataclass
import asyncio

from application.ports.cache_port import CachePort

@dataclass
class CacheEntry:
    value: Any
    expires_at: datetime

class MemoryCacheAdapter(CachePort):
    """Implementación de cache en memoria para desarrollo."""

    def __init__(self):
        self._cache: dict[str, CacheEntry] = {}
        self._lock = asyncio.Lock()

    async def get(self, key: str) -> Optional[Any]:
        """Obtiene valor del cache."""
        async with self._lock:
            entry = self._cache.get(key)
            if entry is None:
                return None

            if datetime.utcnow() > entry.expires_at:
                del self._cache[key]
                return None

            return entry.value

    async def set(
        self,
        key: str,
        value: Any,
        ttl_seconds: int = 3600,
    ) -> None:
        """Guarda valor en cache."""
        async with self._lock:
            expires_at = datetime.utcnow() + timedelta(seconds=ttl_seconds)
            self._cache[key] = CacheEntry(value=value, expires_at=expires_at)

    async def delete(self, key: str) -> None:
        """Elimina valor del cache."""
        async with self._lock:
            self._cache.pop(key, None)

    async def exists(self, key: str) -> bool:
        """Verifica si existe una clave."""
        value = await self.get(key)
        return value is not None

    async def clear(self) -> None:
        """Limpia todo el cache (útil para tests)."""
        async with self._lock:
            self._cache.clear()
```

---

### 5.3 Prompts

#### `src/infrastructure/prompts/classify_lead.py`

**Propósito:** Template para clasificar el rol del lead.

```python
CLASSIFY_LEAD_SYSTEM = """You are a B2B sales expert. Classify the lead's role type based on their job title and company context.

Role types:
- decision_maker: C-level, VP, Director with budget authority
- influencer: Manager, Lead who can influence decisions
- end_user: Individual contributor who would use the product
- gatekeeper: Admin, Assistant who controls access

Respond in JSON format:
{
  "role_type": "decision_maker|influencer|end_user|gatekeeper",
  "confidence": 0.0-1.0,
  "reasoning": "brief explanation"
}"""

def build_classify_lead_prompt(
    job_title: str,
    company_name: str,
    seniority: str,
) -> str:
    """Construye el prompt para clasificar lead."""
    return f"""Classify this lead:

Job Title: {job_title}
Company: {company_name}
Inferred Seniority: {seniority}

What is their role type?"""
```

---

#### `src/infrastructure/prompts/infer_context.py`

**Propósito:** Template para inferir pain points, hooks y talking points (Step 2 del pipeline).

```python
INFER_CONTEXT_SYSTEM = """You are a B2B sales research expert. Based on the lead's profile, infer likely pain points, personalization hooks, and talking points.

Consider:
1. Common challenges for this role/seniority
2. Industry-specific problems
3. Current tech trends affecting their work
4. Likely priorities based on role type

Be specific and actionable. Avoid generic statements.

Respond in JSON format:
{
  "pain_points": ["specific pain point 1", "specific pain point 2", "specific pain point 3"],
  "hooks": ["personalization hook 1", "personalization hook 2"],
  "talking_points": ["talking point 1", "talking point 2"]
}"""

def build_infer_context_prompt(
    job_title: str,
    company_name: str,
    industry: str | None,
    role_type: str,
    product_category: str,
    years_in_role: int | None = None,
    company_size: str | None = None,
) -> str:
    """Construye el prompt para inferir contexto."""
    context_parts = []

    if industry:
        context_parts.append(f"Industry: {industry}")
    if years_in_role:
        context_parts.append(f"Years in role: {years_in_role}")
    if company_size:
        context_parts.append(f"Company size: {company_size}")

    additional_context = "\n".join(context_parts) if context_parts else "No additional context available"

    return f"""Infer likely pain points and personalization opportunities for this lead:

LEAD PROFILE:
- Job Title: {job_title}
- Company: {company_name}
- Role Type: {role_type}
{additional_context}

PRODUCT CONTEXT:
- Category: {product_category}

What are their likely pain points, personalization hooks, and talking points?"""
```

---

#### `src/infrastructure/prompts/generate_message.py`

**Propósito:** Template para generar el mensaje final.

```python
from domain.enums.channel import Channel
from domain.enums.sequence_step import SequenceStep
from domain.enums.message_strategy import MessageStrategy

GENERATE_MESSAGE_SYSTEM = """You are an expert B2B copywriter. Generate personalized outreach messages that:
1. Feel genuine and human (not templated)
2. Reference specific details about the prospect
3. Have a clear value proposition
4. End with a soft call-to-action

NEVER use:
- Generic phrases like "game changer", "revolucionar", "líder del mercado"
- Aggressive sales language
- False urgency

Respond with ONLY the message content, no explanations."""

def build_generate_message_prompt(
    lead_name: str,
    lead_title: str,
    lead_company: str,
    sender_name: str,
    sender_company: str,
    channel: Channel,
    sequence_step: SequenceStep,
    strategy: MessageStrategy,
    pain_points: list[str],
    hooks: list[str],
    product_name: str,
    product_benefit: str,
    communication_style: str,
    years_in_role: int | None = None,
) -> str:
    """Construye el prompt para generar mensaje."""

    context_parts = []

    if years_in_role:
        context_parts.append(f"- Has been in their role for {years_in_role}+ years")

    context = "\n".join(context_parts) if context_parts else "- No additional context"

    pain_points_str = "\n".join(f"- {p}" for p in pain_points[:3])
    hooks_str = "\n".join(f"- {h}" for h in hooks[:2])

    return f"""Generate a {channel.value} message for {sequence_step.value}.

LEAD:
- Name: {lead_name}
- Title: {lead_title}
- Company: {lead_company}
{context}

SENDER:
- Name: {sender_name}
- Company: {sender_company}

STRATEGY: {strategy.value} - {strategy.description}

INFERRED PAIN POINTS:
{pain_points_str}

PERSONALIZATION HOOKS:
{hooks_str}

PRODUCT TO MENTION:
- Name: {product_name}
- Key Benefit: {product_benefit}

TONE: {communication_style}

MAX LENGTH: {channel.max_length} characters

Write the message now:"""
```

---

### 5.4 Config

#### `src/infrastructure/config/settings.py`

**Propósito:** Configuración de la aplicación usando pydantic-settings.

```python
from pydantic_settings import BaseSettings
from pydantic import Field
from functools import lru_cache

class Settings(BaseSettings):
    """Application settings loaded from environment."""

    # API Config
    app_name: str = "LeadAdapter"
    app_version: str = "1.0.0"
    debug: bool = False

    # OpenAI Config
    openai_api_key: str = Field(..., env="OPENAI_API_KEY")
    openai_model: str = Field(default="gpt-4o-mini", env="OPENAI_MODEL")
    openai_timeout: int = Field(default=30, env="OPENAI_TIMEOUT")

    # Redis Config (opcional)
    redis_url: str | None = Field(default=None, env="REDIS_URL")
    cache_ttl_seconds: int = Field(default=3600, env="CACHE_TTL_SECONDS")

    # Quality Gate Config
    quality_threshold: float = Field(default=6.0, env="QUALITY_THRESHOLD")
    max_generation_attempts: int = Field(default=3, env="MAX_GENERATION_ATTEMPTS")

    # Rate Limiting
    rate_limit_requests: int = Field(default=100, env="RATE_LIMIT_REQUESTS")
    rate_limit_window_seconds: int = Field(default=60, env="RATE_LIMIT_WINDOW")

    model_config = {
        "env_file": ".env",
        "env_file_encoding": "utf-8",
        "case_sensitive": False,
    }

@lru_cache
def get_settings() -> Settings:
    """Returns cached settings instance."""
    return Settings()
```

---

## 6. API Layer

### 6.1 Estructura

```
src/api/
├── __init__.py
├── routers/
│   ├── __init__.py
│   ├── messages.py
│   └── health.py
├── dependencies/
│   ├── __init__.py
│   └── container.py
└── middleware/
    ├── __init__.py
    └── error_handler.py
```

---

### 6.2 Routers

#### `src/api/routers/messages.py`

**Propósito:** Endpoints para generación de mensajes.

```python
from fastapi import APIRouter, Depends, HTTPException, status
from application.dtos.requests import GenerateMessageRequest
from application.dtos.responses import GenerateMessageResponse, ErrorResponse
from application.use_cases.generate_message import GenerateMessageUseCase
from api.dependencies.container import get_generate_message_use_case
from domain.exceptions.domain_exceptions import (
    InvalidLeadError,
    InvalidPlaybookError,
    QualityThresholdNotMetError,
)

router = APIRouter(prefix="/messages", tags=["Messages"])

@router.post(
    "/generate",
    response_model=GenerateMessageResponse,
    responses={
        400: {"model": ErrorResponse, "description": "Invalid request data"},
        422: {"model": ErrorResponse, "description": "Validation error"},
        500: {"model": ErrorResponse, "description": "Internal server error"},
    },
)
async def generate_message(
    request: GenerateMessageRequest,
    use_case: GenerateMessageUseCase = Depends(get_generate_message_use_case),
) -> GenerateMessageResponse:
    """
    Genera un mensaje de prospección B2B personalizado.

    El mensaje se genera usando un pipeline de IA que:
    1. Clasifica el rol del lead
    2. Infiere pain points relevantes
    3. Genera contenido personalizado
    4. Valida calidad (score mínimo 6/10)
    """
    try:
        return await use_case.execute(request)
    except InvalidLeadError as e:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail={"error": "Invalid lead data", "detail": str(e), "code": "INVALID_LEAD"},
        )
    except InvalidPlaybookError as e:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail={"error": "Invalid playbook", "detail": str(e), "code": "INVALID_PLAYBOOK"},
        )
    except QualityThresholdNotMetError as e:
        raise HTTPException(
            status_code=status.HTTP_422_UNPROCESSABLE_ENTITY,
            detail={
                "error": "Quality threshold not met",
                "detail": f"Best score: {e.score}, threshold: {e.threshold}",
                "code": "QUALITY_THRESHOLD_NOT_MET",
            },
        )
```

---

#### `src/api/routers/health.py`

**Propósito:** Health check endpoint.

```python
from fastapi import APIRouter
from application.dtos.responses import HealthResponse
from infrastructure.config.settings import get_settings

router = APIRouter(tags=["Health"])

@router.get("/health", response_model=HealthResponse)
async def health_check() -> HealthResponse:
    """
    Health check endpoint.

    Retorna estado del servicio y versión.
    """
    settings = get_settings()
    return HealthResponse(
        status="healthy",
        version=settings.app_version,
    )
```

---

### 6.3 Dependencies

#### `src/api/dependencies/container.py`

**Propósito:** Dependency Injection container.

```python
from functools import lru_cache
from application.use_cases.generate_message import GenerateMessageUseCase
from application.services.prompt_chain_orchestrator import PromptChainOrchestrator
from application.services.message_scorer import MessageScorer
from application.services.quality_gate import QualityGate
from infrastructure.adapters.openai_adapter import OpenAIAdapter
from infrastructure.adapters.memory_cache_adapter import MemoryCacheAdapter
from infrastructure.config.settings import get_settings, Settings
from domain.services.strategy_selector import StrategySelector
from domain.services.seniority_inferrer import SeniorityInferrer
from domain.services.icp_matcher import ICPMatcher

@lru_cache
def get_llm_adapter() -> OpenAIAdapter:
    """Returns singleton OpenAI adapter."""
    return OpenAIAdapter(get_settings())

@lru_cache
def get_cache_adapter() -> MemoryCacheAdapter:
    """Returns singleton cache adapter."""
    return MemoryCacheAdapter()

def get_generate_message_use_case() -> GenerateMessageUseCase:
    """
    Factory for GenerateMessageUseCase with all dependencies injected.
    """
    settings = get_settings()
    llm = get_llm_adapter()
    cache = get_cache_adapter()

    # Domain services (sin dependencias externas)
    strategy_selector = StrategySelector()
    seniority_inferrer = SeniorityInferrer()
    icp_matcher = ICPMatcher()

    # Application services
    prompt_orchestrator = PromptChainOrchestrator(llm=llm)
    scorer = MessageScorer()
    quality_gate = QualityGate(
        orchestrator=prompt_orchestrator,
        scorer=scorer,
        threshold=settings.quality_threshold,
        max_attempts=settings.max_generation_attempts,
    )

    return GenerateMessageUseCase(
        llm=llm,
        cache=cache,
        prompt_orchestrator=prompt_orchestrator,
        quality_gate=quality_gate,
        strategy_selector=strategy_selector,
        seniority_inferrer=seniority_inferrer,
        icp_matcher=icp_matcher,
    )
```

---

### 6.4 Middleware

#### `src/api/middleware/error_handler.py`

**Propósito:** Manejo global de errores.

```python
import logging
from fastapi import Request, status
from fastapi.responses import JSONResponse
from starlette.middleware.base import BaseHTTPMiddleware
import traceback

logger = logging.getLogger(__name__)

class ErrorHandlerMiddleware(BaseHTTPMiddleware):
    """Global error handler middleware."""

    async def dispatch(self, request: Request, call_next):
        try:
            return await call_next(request)
        except Exception as exc:
            logger.error(f"Unhandled exception: {exc}")
            logger.error(traceback.format_exc())

            return JSONResponse(
                status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
                content={
                    "error": "Internal server error",
                    "detail": str(exc) if request.app.debug else "An unexpected error occurred",
                    "code": "INTERNAL_ERROR",
                },
            )
```

---

## 7. Archivo Principal

### `src/main.py`

**Propósito:** Entry point de la aplicación FastAPI.

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

from api.routers import messages, health
from api.middleware.error_handler import ErrorHandlerMiddleware
from infrastructure.config.settings import get_settings

def create_app() -> FastAPI:
    """Application factory."""
    settings = get_settings()

    app = FastAPI(
        title=settings.app_name,
        version=settings.app_version,
        description="API REST para generación de mensajes de prospección B2B personalizados",
        docs_url="/docs",
        redoc_url="/redoc",
    )

    # Middleware
    app.add_middleware(ErrorHandlerMiddleware)
    app.add_middleware(
        CORSMiddleware,
        allow_origins=["*"],  # Configurar según necesidad
        allow_credentials=True,
        allow_methods=["*"],
        allow_headers=["*"],
    )

    # Routers
    app.include_router(health.router, prefix="/api/v1")
    app.include_router(messages.router, prefix="/api/v1")

    return app

app = create_app()

if __name__ == "__main__":
    import uvicorn
    uvicorn.run("main:app", host="0.0.0.0", port=8000, reload=True)
```

### `src/__init__.py`

```python
"""LeadAdapter - B2B Message Generation API."""
```

---

## 8. Checklist de Implementación

### Fase 0: Setup

- [x] Crear estructura de carpetas `src/`
- [x] Crear todos los `__init__.py`
- [x] Configurar `pyproject.toml` o `requirements.txt`
- [x] Crear `.env.example`

### Fase 1: Domain Layer

- [x] `domain/entities/lead.py`
- [x] `domain/entities/sender.py`
- [X] `domain/entities/playbook.py`
- [x] `domain/entities/message.py`
- [x] `domain/value_objects/work_experience.py`
- [x] `domain/value_objects/campaign_history.py`
- [x] `domain/value_objects/icp_profile.py`
- [x] `domain/value_objects/product.py`
- [x] `domain/enums/channel.py`
- [x] `domain/enums/sequence_step.py`
- [x] `domain/enums/message_strategy.py`
- [X] `domain/services/strategy_selector.py`
- [x] `domain/services/seniority_inferrer.py`
- [x] `domain/services/icp_matcher.py`
- [x] `domain/exceptions/domain_exceptions.py`

### Fase 2: Application Layer

- [x] `application/ports/llm_port.py`
- [x] `application/ports/cache_port.py`
- [x] `application/dtos/requests.py`
- [x] `application/dtos/responses.py`
- [x] `application/use_cases/generate_message.py`
- [x] `application/services/prompt_chain_orchestrator.py`
- [x] `application/services/message_scorer.py`
- [x] `application/services/quality_gate.py`

### Fase 3: Infrastructure Layer

- [x] `infrastructure/config/settings.py`
- [x] `infrastructure/adapters/openai_adapter.py`
- [x] `infrastructure/adapters/memory_cache_adapter.py`
- [x] `infrastructure/prompts/classify_lead.py`
- [x] `infrastructure/prompts/infer_context.py`
- [x] `infrastructure/prompts/generate_message.py`

### Fase 4: API Layer

- [x] `api/routers/health.py`
- [x] `api/routers/messages.py`
- [x] `api/dependencies/container.py`
- [x] `api/middleware/error_handler.py`
- [x] `main.py`

### Fase 5: Testing

- [ ] Tests unitarios domain
- [ ] Tests unitarios application
- [ ] Tests integración adapters
- [ ] Tests E2E API

---

## Anexos

### A. Flujo de una Request

```
HTTP Request (JSON)
       │
       ▼
┌──────────────────┐
│   API Router     │  ← Valida con Pydantic DTOs
│  (messages.py)   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│   Use Case       │  ← Convierte DTO → Entity
│ (generate_msg)   │  ← Orquesta servicios
└────────┬─────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐ ┌───────────┐
│Domain │ │Application│
│Services│ │ Services  │
└───────┘ └─────┬─────┘
                │
                ▼
        ┌───────────────┐
        │Infrastructure │  ← LLM calls, caching
        │  (Adapters)   │
        └───────────────┘
```

### B. Ejemplo de Test Unitario

```python
# tests/domain/services/test_seniority_inferrer.py
import pytest
from domain.services.seniority_inferrer import SeniorityInferrer, Seniority

class TestSeniorityInferrer:
    def setup_method(self):
        self.inferrer = SeniorityInferrer()

    @pytest.mark.parametrize("title,expected", [
        ("CEO", Seniority.C_LEVEL),
        ("CTO at Startup", Seniority.C_LEVEL),
        ("Co-Founder", Seniority.C_LEVEL),
        ("VP of Engineering", Seniority.VP),
        ("Director of Sales", Seniority.DIRECTOR),
        ("Engineering Manager", Seniority.MANAGER),
        ("Senior PHP Developer", Seniority.SENIOR),
        ("Staff Engineer", Seniority.SENIOR),
        ("Junior Data Analyst", Seniority.JUNIOR),
        ("Software Engineer", Seniority.MID),
        ("Developer", Seniority.MID),
    ])
    def test_infer_seniority(self, title: str, expected: Seniority):
        assert self.inferrer.infer(title) == expected

    def test_empty_title_returns_unknown(self):
        assert self.inferrer.infer("") == Seniority.UNKNOWN
```

---

**Documento generado:** Enero 2026
**Basado en:** Technical Design Document v3.0
