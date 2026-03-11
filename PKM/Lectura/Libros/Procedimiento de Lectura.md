# Procedimiento de Lectura Activa

## Principio Fundamental

> **Lo que retenés y aplicás son patrones estructurales, no datos.**

Los patrones estructurales describen *cómo se relacionan las partes* de un sistema, no *qué son las partes*. Por eso el mismo patrón aplica en física, economía, psicología y biología.

---

## Antes de Leer

1. Tener a mano solo **lapicera/lápiz**
2. Definir módulo de lectura: **30 minutos**
3. Mentalidad: "Estoy buscando patrones, no acumulando datos"

---

## Durante la Lectura: Sistema de Marcado

| Tipo                 | Símbolo          | Cómo se Marca                     |
| -------------------- | ---------------- | --------------------------------- |
| **Patrón**           | `[texto] !`      | Corchetes + exclamación al margen |
| **Ejemplo**          | `• nota`         | Punto + nota breve al margen      |
| **Contraejemplo**    | `• ✗ nota`       | Punto + X + nota al margen        |
| **Conexión**         | `[texto] → nota` | Flecha + nota al margen           |
| **Pregunta/duda**    | `? subrayado`    | Signo de pregunta + subrayar      |
| **Dato**             | `d subrayado`    | "d" al margen + subrayar          |
| **Cita textual**     | `" "`            | Remarcar/resaltar el texto        |
| **Advertencia**      | `! subrayado`    | Exclamación + subrayar            |
| **Insight personal** | `* subrayado`    | Asterisco + subrayar              |

### Reglas de Marcado

- **Patrón**: encerrarlo en corchetes + `!` al margen — es el oro
- **Ejemplo**: poner `•` al margen + nota breve indicando a qué patrón pertenece
- **Contraejemplo**: poner `• ✗` al margen — caso donde el patrón NO aplica
- **Conexión**: poner `→` al margen — "esto conecta con otro dominio"
- **Pregunta/duda**: poner `?` al margen + subrayar el texto
- **Dato importante**: poner `d` al margen + subrayar — información de soporte
- **Cita textual**: remarcar/resaltar — palabras exactas del autor
- **Advertencia**: poner `!` al margen + subrayar — errores comunes o trampas
- **Insight personal**: poner `*` al margen + subrayar — revelación propia

---

## La Pregunta Clave

Cada vez que identifiques un patrón, pregúntate inmediatamente:

> **"¿En qué otro lugar puedo aplicar este patrón?"**

Intentá generar **2-3 ejemplos** en dominios diferentes al del libro. Si no podés, todavía no entendiste el patrón.

---

## Después de Cada Capítulo

### Transferir a Obsidian

1. Abrir el libro en las páginas marcadas del capítulo
2. **Crear nota de capítulo** usando el template `Notas de Capítulo`:
   - Incluir: resumen, datos, citas, preguntas, advertencias, insights
3. Para cada **patrón** encontrado:
   - Crear una nota en `PKM/Patrones/` usando el template `Patrón`
   - Linkear desde la nota del capítulo al patrón
4. Vincular las notas de patrones entre sí cuando corresponda

### Estructura de Nota de Patrón

> Usar template: `PKM/Templates/Patrón.md`

```markdown
# [Nombre del Patrón]

## Definición
[Explicación en tus palabras]

## Cuándo Aplica
[Condiciones donde este patrón se manifiesta]

## Ejemplos
- Dominio 1: [ejemplo]
- Dominio 2: [ejemplo]
- Dominio 3: [ejemplo]

## Contraejemplos (opcional)
[Casos donde el patrón NO aplica y por qué]

## Advertencias (opcional)
[Errores comunes o trampas al aplicar este patrón]

## Insights (opcional)
[Revelaciones personales o conexiones que descubriste]

## Conexiones
- [[Otro Patrón Relacionado]]
- [[Concepto Vinculado]]

## Fuente
- Libro: [título]
- Página: [número]
```

### Estructura de Nota de Capítulo

> Usar template: `PKM/Templates/Notas de Capítulo.md`

```markdown
# [Nombre del Libro] — Cap. [N]: [Título del Capítulo]

**Libro**: [[Nombre del Libro]]
**Páginas**: [inicio] - [fin]
**Fecha de lectura**: [fecha]

---

## Resumen
[Resumen breve del capítulo en 2-3 oraciones]

## Datos Importantes
- [dato 1]
- [dato 2]

## Citas Textuales
> "[cita exacta del autor]" — p. [número]

## Preguntas Abiertas
- [ ] [pregunta que quedó sin resolver]

## Advertencias
- ⚠️ [error común o trampa mencionada]

## Insights Personales
- 💡 [conexión o revelación que tuviste]

## Patrones Identificados
- [[Patrón 1 encontrado en este capítulo]]
- [[Patrón 2 encontrado en este capítulo]]
```

---

## Organización en Obsidian

```
PKM/
├── Patrones/                          ← Nodos centrales (transferibles)
│   ├── Complejidad desde componentes simples.md
│   ├── Escasez aumenta valor.md
│   └── ...
├── Templates/                         ← Templates de Templater
│   ├── Patrón.md
│   └── Notas de Capítulo.md
└── Libros/
    ├── Procedimiento de Lectura.md    ← Este archivo
    └── [Nombre del Libro]/
        ├── [Nombre del Libro].md      ← Nota índice del libro
        └── Notas/
            ├── Cap 1 - [Título].md
            ├── Cap 2 - [Título].md
            └── ...
```

### Principios de Organización

1. **Patrones en `PKM/Patrones/`**: Son conocimiento transferible, no pertenecen a un libro específico
2. **Notas de capítulo**: Contienen el contexto específico del libro (datos, citas, preguntas)
3. **Links bidireccionales**: La nota del capítulo linkea a los patrones encontrados

---

## Test de Dominio

Para verificar que un patrón está *grabado*:

| Nivel | Capacidad | Pregunta de Verificación |
|-------|-----------|-------------------------|
| 1 | Puedo explicarlo | ¿Puedo definirlo sin mirar notas? |
| 2 | Puedo predecir | Dado X, ¿qué pasa con Y? |
| 3 | Puedo transferir | ¿Puedo aplicarlo a un dominio que el autor nunca mencionó? |
| 4 | Puedo enseñar | ¿Puedo responder preguntas inesperadas sobre él? |

**Objetivo mínimo**: Nivel 3.

---

## Resumen Ejecutivo

1. **Leer** buscando patrones estructurales (30 min por sesión)
2. **Marcar** en el libro con símbolos:
   - `[texto] !` patrón
   - `• nota` ejemplo
   - `• ✗ nota` contraejemplo
   - `→ nota` conexión
   - `? subrayado` pregunta
   - `d subrayado` dato
   - `" "` cita textual
   - `! subrayado` advertencia
   - `* subrayado` insight
3. **Preguntar** siempre: "¿Dónde más aplica esto?"
4. **Transferir** a Obsidian al terminar cada capítulo:
   - Crear nota de capítulo en `Libros/[Libro]/Notas/`
   - Crear notas de patrones en `PKM/Patrones/`
5. **Organizar** con el patrón como nodo central (links bidireccionales)
