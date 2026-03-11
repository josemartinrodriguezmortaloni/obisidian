# Verdad Fundamental (Axioma Cero)

> **El software existe para resolver problemas humanos, y debe poder evolucionar mientras lo hace.**

Este es el axioma irreducible del cual todo lo demás deriva. No es negociable, no es contextual. Si tu software no resuelve un problema humano, no debería existir. Si no puede evolucionar, morirá.

---

## Primera Derivación: El Modelo

Del Axioma Cero se deriva:

> **El software es un modelo ejecutable de la realidad. Su estructura debe reflejar la estructura del problema, no la de la solución técnica.**

_¿Por qué?_ Porque los problemas humanos existen en dominios humanos (refinerías, hospitales, finanzas). Si el código refleja el dominio, cuando el dominio evoluciona, sabés exactamente dónde cambiar. Si refleja la tecnología, cada cambio de negocio requiere arqueología técnica.

---

## Segunda Derivación: La Ley de Localidad

Del modelo ejecutable se deriva:

> **Cada decisión, cada dato, cada comportamiento debe vivir en exactamente un lugar —el lugar donde tiene sentido conceptualmente— y ese lugar debe poder cambiar sin propagar ondas de choque al resto del sistema.**

_¿Por qué?_ Porque la evolución requiere cambio. Y el cambio solo es sostenible si es local. Si mover una pieza derrumba el edificio, el edificio no puede evolucionar.

---

## Los Tres Pilares

De la Ley de Localidad emergen tres pilares estructurales:

```
┌─────────────────────────────────────────────────────────────────────┐
│                         AXIOMA CERO                                 │
│      "Resolver problemas humanos + Evolucionar"                     │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    MODELO EJECUTABLE                                │
│      "Reflejar el problema, no la tecnología"                       │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────┐
│                     LEY DE LOCALIDAD                                │
│      "Cada cosa en su único lugar, cambios sin propagación"         │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                ┌─────────────────┼─────────────────┐
                ▼                 ▼                 ▼
        ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
        │RESPONSABI-  │   │ COHESIÓN    │   │ACOPLAMIENTO │
        │LIDAD ÚNICA  │   │ POR RAZÓN   │   │ INTENCIONAL │
        │ LOCALIZADA  │   │ DE CAMBIO   │   │             │
        └─────────────┘   └─────────────┘   └─────────────┘
```

---

### Pilar I: Responsabilidad Única Localizada

```
Un módulo existe si y solo si:
  1. Tiene una responsabilidad clara e indivisible
  2. Esa responsabilidad no puede vivir en otro lugar sin aumentar el acoplamiento
  3. Sin él, el sistema pierde una capacidad esencial
```

**Heurística de validación**: Si podés eliminar un módulo y el sistema sigue funcionando igual, ese módulo no debería existir como tal.

**Contra-heurística**: Si eliminar un módulo rompe partes no relacionadas del sistema, el acoplamiento es excesivo.

---

### Pilar II: Cohesión por Razón de Cambio

```
Agrupar código NO por similitud técnica, sino por volatilidad compartida.
Lo que cambia junto, vive junto.
Lo que cambia por razones distintas, se separa.
```

**Jerarquía de agrupación**:

|Nivel|Unidad|Criterio de cohesión|
|---|---|---|
|1|Método|Un objetivo, una operación atómica|
|2|Clase|Aspectos que cambian por la misma razón de negocio|
|3|Módulo|Una capacidad que evoluciona independientemente|
|4|Bounded Context|Un subdominio con su propio lenguaje ubicuo|

---

### Pilar III: Acoplamiento Intencional

```
El acoplamiento no es malo. El acoplamiento ACCIDENTAL es malo.
Cada dependencia debe ser:
  - Explícita (visible en el contrato)
  - Direccional (flujo claro)
  - Justificada (resuelve un problema real)
  - Mínima (solo lo necesario para colaborar)
```

**Asigná las responsabilidades de manera que su localización no incremente el acoplamiento hasta un nivel que produzca efectos negativos.**

**Espectro de acoplamiento** (de menor a mayor riesgo):

|Tipo|Ejemplo|Riesgo|Acción|
|---|---|---|---|
|Datos|Pasar un DTO|Bajo|Aceptable|
|Stamp|Pasar objeto, usar parte|Medio-bajo|Vigilar|
|Control|Flags que alteran comportamiento|Medio|Evitar|
|Externo|Dependencia de API externa|Medio-alto|Abstraer|
|Común|Estado global compartido|Alto|Prohibir|
|Contenido|Acceder a internos de otro módulo|Crítico|Nunca|

---

## Las Cuatro Preguntas de Diseño

Antes de crear cualquier estructura, respondé:

|#|Pregunta|Propósito|
|---|---|---|
|1|**¿Qué problema humano resuelve esto?**|Validar existencia|
|2|**¿Cuál es su única razón de existir?**|Validar responsabilidad|
|3|**¿Qué pasa cuando esto cambie?**|Validar evolución|
|4|**¿Puedo eliminarlo sin romper lo no relacionado?**|Validar acoplamiento|

Si no podés responder la primera, no debería existir. Si no podés responder la segunda en una frase sin "y", la responsabilidad no es única. Si la tercera revela propagación, hay acoplamiento oculto. Si la cuarta es "no", hay dependencias accidentales.

---

## Definición de Modularidad

> **La modularidad es la propiedad de un sistema que se ha descompuesto en un conjunto de módulos cohesivos y débilmente acoplados.**

La modularidad se alcanza:

- Diseñando cada **método** con un único y claro objetivo
- Agrupando un conjunto de **aspectos relacionados** en una clase
- Organizando **capacidades del sistema** en módulos autónomos
- Manteniendo **dependencias explícitas y mínimas** entre módulos

---

## Reglas Operativas

### Para Métodos

```python
# ✗ Múltiples objetivos, imposible evolucionar independientemente
def process_order(order):
    validate(order)
    calculate_total(order)
    apply_discounts(order)
    save_to_db(order)
    send_email(order)

# ✓ Un objetivo claro, cada parte puede evolucionar
def place_order(order):
    validated = self.validator.validate(order)
    priced = self.pricer.calculate(validated)
    self.repository.save(priced)
    self.notifier.notify_placed(priced)
```

### Para Clases

```
Una clase agrupa aspectos que:
  ✓ Operan sobre los mismos datos
  ✓ Cambian por las mismas razones de negocio
  ✓ Tienen el mismo nivel de abstracción
  
  ✗ NO porque son "del mismo tipo técnico"
  ✗ NO porque "se parecen"
```

### Para Módulos/Directorios

```
Un directorio es un módulo si:
  ✓ Representa una capacidad del sistema (no una capa técnica)
  ✓ Tiene un boundary claro (API pública definida)
  ✓ Puede ser desarrollado/testeado/desplegado semi-independientemente
  ✓ Su eliminación remueve UNA capacidad, no corrompe otras
  ✓ Sin él, el sistema pierde algo esencial
```

---

## Estructura según ARL (Ejemplo SimPlant)

```
src/
├── process-control/           # Capacidad: Control de procesos
│   ├── domain/               
│   ├── application/          
│   ├── infrastructure/       
│   └── api/                   # Contrato público
│
├── simulation-engine/         # Capacidad: Motor de simulación
│   ├── domain/
│   ├── application/
│   ├── infrastructure/
│   └── api/
│
├── reinforcement-learning/    # Capacidad: Aprendizaje por refuerzo
│   ├── domain/
│   ├── application/
│   ├── infrastructure/
│   └── api/
│
└── shared-kernel/             # Solo abstracciones inevitables
    ├── value-objects/
    └── interfaces/
```

**Test de validación**: Si eliminás `reinforcement-learning/`, perdés RL pero simulación y control siguen funcionando. Cada módulo resuelve un problema humano específico y puede evolucionar independientemente.

---

## El Test Final

> **Si tu arquitectura no puede explicarse sin mencionar frameworks, bases de datos o tecnologías específicas, no es una arquitectura —es una configuración.**

La estructura debe gritar el problema que resuelve:

- ✓ "Soy un sistema de control de procesos industriales"
- ✗ "Soy una app NestJS con PostgreSQL"

---

## Manifiesto ARL

```
1. El software existe para humanos, no para máquinas.
2. El código que no puede evolucionar está muerto.
3. La estructura refleja el problema, no la tecnología.
4. Cada cosa tiene un único lugar; ese lugar puede cambiar sin destruir.
5. Lo que cambia junto, vive junto.
6. Toda dependencia es una deuda; solo contraer las necesarias.
7. Un módulo existe solo si es esencial e indivisible.
8. La modularidad no es un accidente; se diseña intencionalmente.
9. Si no podés explicar qué problema resuelve, no debería existir.
10. Cuestionar las prácticas es la práctica más importante.
```
