# LeadAdapter - Technical Design Document

**Versión:** 3.0 | **Fecha:** Enero 2026 | **Estado:** Ready for Implementation

---

## 1. Qué Vamos a Construir

**API REST stateless** que genera mensajes de prospección B2B personalizados usando IA (OpenAI).

### Pipeline de Generación
```
Synapsale (datos) → LeadAdapter API → OpenAI → Mensaje Personalizado
                         │
            ┌────────────┼────────────┐
            ▼            ▼            ▼
       Análisis     Generación    Quality Gate
       Contexto     Contenido     (score ≥ 6)
```

### Stack Tecnológico
| Componente | Tecnología |
|------------|------------|
| Framework | FastAPI |
| Validación | Pydantic v2 |
| LLM | OpenAI gpt-4o-mini / gpt-4o |
| Cache | Redis (prod) / Memory (dev) |
| Testing | pytest |

---

## 2. Arquitectura Hexagonal

```
src/
├── domain/           # Core negocio - SIN dependencias externas
│   ├── entities/     # Lead, Sender, Playbook, Message
│   ├── value_objects/# WorkExperience, ICPProfile, etc.
│   ├── enums/        # Channel, SequenceStep, MessageStrategy
│   ├── services/     # StrategySelector, ICPMatcher, SeniorityInferrer
│   └── exceptions/
│
├── application/      # Casos de uso y orquestación
│   ├── ports/        # Interfaces: LLMPort, CachePort
│   ├── use_cases/    # GenerateMessage, GenerateSequence
│   ├── dtos/         # Request/Response schemas
│   └── services/     # PromptChainOrchestrator, MessageScorer
│
├── infrastructure/   # Implementaciones concretas
│   ├── adapters/     # OpenAIAdapter, RedisCacheAdapter
│   ├── prompts/      # Templates de prompts
│   └── config/       # Settings
│
└── api/              # HTTP Layer
    ├── routers/      # messages, sequences, health
    ├── dependencies/ # DI container
    └── middleware/   # Error handling
```

---

## 3. API Endpoints

### POST `/api/v1/messages/generate`

**Request (simplificado):**
```json
{
  "channel": "linkedin | email",
  "sequence_step": "first_contact | follow_up_1 | follow_up_2 | breakup",
  "lead": {
    "first_name": "Mateo",
    "last_name": "Gutierrez", 
    "job_title": "Senior PHP Developer",
    "company_name": "Tecnocom",
    "work_experience": [...],
    "campaign_history": {...}
  },
  "sender": {
    "name": "Fernando",
    "job_title": "CTO",
    "company_name": "Synapsale"
  },
  "playbook": {
    "communication_style": "B2B directo",
    "products": [...],
    "icp_profiles": [...]
  }
}
```

**Response:**
```json
{
  "message_id": "msg_abc123",
  "content": "Hola Mateo, vi que llevas más de 6 años...",
  "quality": { "score": 7.5, "breakdown": {...} },
  "strategy_used": "TECHNICAL_PEER",
  "metadata": { "tokens_used": 450, "generation_time_ms": 2847 }
}
```

### Otros Endpoints
| Endpoint | Método | Descripción |
|----------|--------|-------------|
| `/api/v1/health` | GET | Health check |
| `/api/v1/sequences/generate` | POST | Genera secuencia completa |

---

## 4. Flujo de Generación

### 4.1 Pipeline de IA (Prompt Chaining)
```
Step 1: Clasificar Lead (~100 tokens)
    → decision_maker | influencer | end_user | gatekeeper
    
Step 2: Inferir Pain Points (~150 tokens)
    → dolores probables + hooks de personalización
    
Step 3: Generar Mensaje (~500 tokens)
    → mensaje final con estrategia seleccionada
    
Step 4: Quality Gate
    → score < 6? → regenerar (max 3 intentos)
```

### 4.2 Criterios de Scoring (10 puntos)
| Criterio | Peso | Evalúa |
|----------|------|--------|
| Personalización | 3 | Nombre, empresa, contexto específico |
| Anti-spam | 3 | Penaliza frases genéricas |
| Estructura | 2 | Hook, propuesta valor, CTA |
| Tono | 2 | Match con playbook y seniority |

**Threshold mínimo:** 6 puntos

### 4.3 Inferencias Automáticas
| Campo | Estrategia |
|-------|------------|
| Seniority | Parsear job_title: "CEO" → C_LEVEL, "Senior" → SENIOR |
| Industry | Match job_title + company vs keywords_sector |
| Years in role | Calcular desde work_experience[0].start |

---

## 5. Campos Requeridos vs Opcionales

### Requeridos (error 400 si faltan)
- `lead.first_name`, `lead.company_name`, `lead.job_title`
- `sender.name`, `sender.company_name`
- `playbook.communication_style`, `playbook.products[]`

### Opcionales (con fallback)
| Campo Faltante | Fallback |
|----------------|----------|
| lead.bio | Usar job_title + skills |
| lead.work_experience | Solo empresa actual |
| lead.campaign_history | Asumir primer contacto |
| playbook.success_cases | Usar common_use_cases |

---

## 6. Plan de Implementación

### Fase 0: Setup (1 día)
- [ ] Crear estructura `src/` hexagonal
- [ ] Eliminar: `app/db/`, `app/models/`, `alembic/`
- [ ] Actualizar dependencias (agregar openai, redis)

### Fase 1: Domain Layer (2 días)
- [ ] Entities: Lead, Sender, Playbook, Message
- [ ] Value Objects: WorkExperience, ICPProfile, CampaignHistory
- [ ] Enums: Channel, SequenceStep, MessageStrategy, Tone
- [ ] Services: StrategySelector, SeniorityInferrer, ICPMatcher

### Fase 2: Application Layer (2 días)
- [ ] Ports: LLMPort, CachePort (interfaces)
- [ ] Use Cases: GenerateMessage, GenerateSequence
- [ ] DTOs: GenerateMessageRequest, GenerateMessageResponse
- [ ] Services: PromptChainOrchestrator, MessageScorer, QualityGate

### Fase 3: Infrastructure Layer (2 días)
- [ ] OpenAIAdapter implementando LLMPort
- [ ] MemoryCacheAdapter (dev) / RedisCacheAdapter (prod)
- [ ] Prompt templates optimizados
- [ ] Rate limiter con backoff exponencial

### Fase 4: API Layer (1 día)
- [ ] Router `/api/v1/messages/generate`
- [ ] Router `/api/v1/health`
- [ ] Middleware error handling
- [ ] Dependency injection container

### Fase 5: Testing & Docs (2 días)
- [ ] Tests unitarios domain + application
- [ ] Tests integración adapters
- [ ] Tests E2E API
- [ ] README actualizado

---

## 7. Milestones

| Semana | Entregable |
|--------|------------|
| 1 | Arquitectura hexagonal + genera mensajes básicos |
| 2 | API completa + Quality Gate funcionando |
| 2-3 | Tests + documentación + deploy ready |

**Duración total:** 2-3 semanas (~60-80 horas)

---

## 8. Decisiones Técnicas Clave

### ¿Por qué stateless?
Synapsale envía todos los datos en cada request. No necesitamos persistencia local.

### ¿Por qué Arquitectura Hexagonal?
- Cambiar OpenAI por Anthropic sin tocar el dominio
- Testear core sin dependencias externas
- Agregar nuevos proveedores fácilmente

### ¿Por qué Prompt Chaining?
- ~850 tokens vs ~1500 tokens (single prompt)
- Mejor calidad por pasos especializados
- Más fácil de debuggear y optimizar

### ¿Por qué Quality Gate?
- Evita mensajes spam-like
- Regeneración automática si score < 6
- Máximo 3 intentos, luego warning

---

## 9. Archivos a Crear

```
src/
├── __init__.py
├── main.py
├── domain/
│   ├── entities/
│   │   ├── lead.py
│   │   ├── sender.py
│   │   ├── playbook.py
│   │   └── message.py
│   ├── value_objects/
│   │   ├── work_experience.py
│   │   ├── campaign_history.py
│   │   ├── icp_profile.py
│   │   └── product.py
│   ├── enums/
│   │   ├── channel.py
│   │   ├── sequence_step.py
│   │   └── message_strategy.py
│   ├── services/
│   │   ├── strategy_selector.py
│   │   ├── seniority_inferrer.py
│   │   └── icp_matcher.py
│   └── exceptions/
│       └── domain_exceptions.py
├── application/
│   ├── ports/
│   │   ├── llm_port.py
│   │   └── cache_port.py
│   ├── use_cases/
│   │   └── generate_message.py
│   ├── dtos/
│   │   ├── requests.py
│   │   └── responses.py
│   └── services/
│       ├── prompt_chain_orchestrator.py
│       ├── message_scorer.py
│       └── quality_gate.py
├── infrastructure/
│   ├── adapters/
│   │   ├── openai_adapter.py
│   │   └── memory_cache_adapter.py
│   ├── prompts/
│   │   ├── classify_lead.py
│   │   └── generate_message.py
│   └── config/
│       └── settings.py
└── api/
    ├── routers/
    │   ├── messages.py
    │   └── health.py
    ├── dependencies/
    │   └── container.py
    └── middleware/
        └── error_handler.py
```

---

## 10. Archivos a Eliminar

```bash
# Eliminar (no necesarios para API stateless)
rm -rf app/db/
rm -rf app/models/
rm -rf app/repositories/
rm -rf alembic/
rm alembic.ini

# Quitar de requirements.txt
# sqlalchemy, alembic, psycopg2-binary
```

---

## 11. Dependencias Finales

```txt
# requirements.txt
fastapi>=0.109.0
uvicorn>=0.27.0
pydantic>=2.5.0
pydantic-settings>=2.1.0
openai>=1.10.0
redis>=5.0.0
httpx>=0.26.0
python-dotenv>=1.0.0

# Dev
pytest>=7.4.0
pytest-asyncio>=0.23.0
pytest-cov>=4.1.0
ruff>=0.1.0
mypy>=1.8.0
```

---

## Glosario

| Término | Definición |
|---------|------------|
| Lead | Prospecto a contactar |
| Sender | Usuario que envía mensajes |
| Playbook | Config comercial (ICP, productos, tono) |
| ICP | Ideal Customer Profile |
| Quality Gate | Validación de calidad antes de entregar |
