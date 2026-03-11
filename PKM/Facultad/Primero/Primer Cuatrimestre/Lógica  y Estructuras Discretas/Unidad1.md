---
id: Unidad1
aliases:
  - Lógica y Estructura Discretas
tags: []
---

# Proposiciones y conectivos lógicos.

> Una **proposición** es una afirmación que puede ser verdadera o falsa, pero no ambas.

La **negación** se denota como $\neg p$ y se lee "no p". La proposición $\neg p$ tiene el valor de V cuando p tiene el valor de vardad F y tiene el valor F cuando p tiene el valor V. Su tabla de verdad es:

|  p  | $\neg p$ |
| :-: | :------: |
|  V  |    F     |
|  F  |    V     |

La **conjunción** se denota como $p \land q$ y se lee "p y q". Y es verdadera cuando tanto p como q son verdaderas, en otro caso son falsas.

|  p  |  q  | $p \land q$ |
| :-: | :-: | :---------: |
|  V  |  V  |      V      |
|  V  |  F  |      F      |
|  F  |  V  |      F      |
|  F  |  F  |      F      |

La **disyunción** se donta como $p \lor q$ y se lee "p o q". Es falsa solamente cuando p y q son falsas. Su tabla de verdad es:

|  p  |  q  | $p \lor q$ |
| :-: | :-: | :--------: |
|  V  |  V  |     V      |
|  V  |  F  |     V      |
|  F  |  V  |     V      |
|  F  |  F  |     F      |

Los símbolos $\neg, \land, \lor$ se denominan **conectivos lógicos**. Una proposición formada de la combinación de otras Proposiciones utilizando conectivos lógico es una **proposición compuesta**.

# Tautologías y contradicción.

> Una **tautología** es una proposición compuesta que es verdadera en todas las proposiciones las cuales la componen.
> Y una **contradicción** cuando es falsa en todas las propo las cuales las componen.

# Implicación y equivalencia.

La proposición compuesta $p \to q$ se la denomina **proposición condicional**. Donde p es la hipótesis y q es la conclusión, a su vez, $\to$ es el operador condicional cuya tabla de verdad es:

|  p  |  q  | $p \to q$ |
| :-: | :-: | :-------: |
|  V  |  V  |     V     |
|  V  |  F  |     F     |
|  F  |  V  |     V     |
|  F  |  F  |     V     |

> Si $p \to q$ es una **tautología** diremos que **p implica logicamente a q** y en este caso escribiremos $p \implies q$

La proposición compuesta $p \leftrightarrow q$ es verdadera sólo cuando los valores de verdad de p y q coinciden, y se denominan **proposición bicondicional**. A su vez $\leftrightarrow$ se denomina **operador bicondicional**. Su tabla de verdad es:

|  p  |  q  | $p \leftrightarrow q$ |
| :-: | :-: | :-------------------: |
|  V  |  V  |           V           |
|  V  |  F  |           F           |
|  F  |  V  |           F           |
|  F  |  F  |           F           |

> Dos proposiciones compuestas p y q son **lógicamente equivalentes** si $p \leftrightarrow q$ es una **tautología** y en este caso escribiremos $p \iff q$

# Predicados y cuantificadores.

> Predicados: Afirmaciones a las cuales se le asignan valores a las variables que intervienen en ella.

Los valores de las variables que aparecen en un predicado deben pertenecer a un conjunto llamada **universo de discurso o universo**, el cual debe ser establecido explicitamente.

> **Cuantificador universal**: $\forall $ se lee "para todo/a", "para cada" o "para cualquier"
> **Cuantificador existencial:** $\exist$ se lee "exite", "para algún" o "para al menos un".

# Conjuntos.

## Operaciones con conjuntos.

La **unión** de dos conjuntos A y B es el conjunto:
$ A \bigcup B = \lbrace x | x \in A \lor x \in B \rbrace$

# Métodos de demostración.

> **Demostación**: Sucesión de afirmaciones que representan una argumentación de la validez de un enunciado matemático.

- **Demostración directa:** Se comienza suponiendo que P es verdadera y de ahí se concluye que Q es veradera.
- **Demostración indirecta:** Se comienza suponiendo que $\neg Q$ es verdadera y de ahí se concluye que $\neg P$ es verdadera.
- **Demostración por contradicción o reducción al absurdo:** Comienza suponiendo que P es verdadera y que Q es falsa, con lo cual se llega a una contradicción, esto significa que la conclusión debe ser verdadera.

# Inducción matemática.
