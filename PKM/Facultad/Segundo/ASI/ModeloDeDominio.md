---
id: Modelo de dominio
aliases: []
tags: []
---

# Introducción

Un modelo de dominio muestra clases conceptuales significativas en un dominio del problema, es el artefacto más importante que se crea durante el análisis orientado a objetos.

> ![Notes]
> El **modelo de dominio** es una representación de las clases conceptuales del mundo real, no de componentes de software. No se trata de un conjunto de diagramas que describen clases de software u objetos software con responsabilidades.

## Modelos de dominio

> ![Important]
> Representación _visual_ de las clases conceptuales u objetos del mundo real en un dominio de interés.

En la notación **UML** un modelo de dominio se representa con un conjunto de **diagrama de clases** en los que no se define ninguna operación. Los diagramas de clases pueden mostrar:

- Objetos del dominio o clases conceptuales.
- Asociaciones entre las clases conceptuales.
- Atributos de las clases conceptuales.

La idea clave es que el modelo de dominio es un diccionario visual de abstracciones, donde el modelo muestra una vista parcial, o abstracción, e ignora detalles sin interés.

Los **modelos de dominio no son modelos de componentes de software**, donde los siguientes elementos no son adecuados en un modelo de dominio:

- **Artefacto de software** ( a menos que el dominio que se esté modelando sea de conceptos de software)
- **Responsabilidades o métodos**.

### Clases conceptuales

Definición: Conjunto de objetos de iguales características que solo poseen atributos.

En cambio una **clase** es un conjunto de objetos de iguales características, poseen operaciones conceptuales y atributos característicos.

Una clase conceptual puede considerarse en términos de símbolo, intensión, y extensión.

- **Símbolo:** Palabras o imágenes que representan una clase conceptual.
- **Intensión:** La definición de una clase conceptual.
- **Extensión:** El conjunto de ejemplos a los que se aplica la clase conceptual.

Cuando creamos un modelo de dominio, normalmente, el símbolo y la vista intensional de la clase conceptual son los que tienen un mayor interés práctico.

### Modelos y descomposición del dominio

En el **análisis estructurado**, la dimensión de la descomposición es por procesos o por funciones. Sin embargo, en el **análisis orientado a objetos,** la dimensión de la descomposición es fundamentalmente por cosas o entidades del dominio.

Una diferencia esencial entre el análisis orientado a objetos y el estructurado es: la división por clases conceptuales (objetos) en lugar de la división por funciones.
