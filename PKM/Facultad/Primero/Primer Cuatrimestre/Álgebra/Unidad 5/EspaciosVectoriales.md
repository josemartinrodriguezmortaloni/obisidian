# Resumen: Espacios Vectoriales

> [!info] Patron Fundamental
> Esta nota es una aplicacion de [[Complejidad desde componentes simples]]: a partir de una **base** (vectores LI), se genera **todo el espacio vectorial** mediante combinaciones lineales.

---

### **1. Definición y Axiomas**

Un **espacio vectorial** $V$ es un conjunto no vacío con dos operaciones (suma y multiplicación por escalar) que cumplen 10 axiomas:

- **Suma**: Cierre, conmutatividad, asociatividad, elemento neutro ($\mathbf{0}$), y elemento opuesto.
- **Multiplicación por escalar**: Cierre, asociatividad mixta, distributividad respecto a suma de vectores y escalares, y elemento neutro ($1$).

**Ejemplos**:

- $\mathbb{R}^n$, matrices $M_{m \times n}$, funciones polinómicas de grado $\leq 2$.
- Contraejemplo: $\mathbb{R}^2$ con producto $k \cdot (u_1, u_2) = (k u_1, 0)$ no es espacio vectorial.

---

### **2. Subespacios**

Un **subespacio** $S$ de $V$ es un subconjunto no vacío que cumple:

1. Cerrado bajo suma: $u, v \in S \Rightarrow u + v \in S$.
2. Cerrado bajo multiplicación por escalar: $u \in S, k \in \mathbb{R} \Rightarrow k \cdot u \in S$.

**Propiedades**:

- Contiene al vector nulo.
- Subespacios triviales: $\{\mathbf{0}\}$ y $V$.

**Ejemplos**:

- Rectas o planos que pasan por el origen en $\mathbb{R}^2$ o $\mathbb{R}^3$.
- Soluciones de sistemas homogéneos $A\mathbf{X} = \mathbf{0}$.

---

### **3. Combinación Lineal y Espacio Generado**

- **Combinación lineal (CL)**: Un vector $\mathbf{w}$ es CL de $\mathbf{v}_1, \dots, \mathbf{v}_n$ si existen escalares $c_1, \dots, c_n$ tales que:
  $$
  \mathbf{w} = c_1 \mathbf{v}_1 + \dots + c_n \mathbf{v}_n.
  $$
- **Espacio generado**: Conjunto de todas las CL de $\{\mathbf{v}_1, \dots, \mathbf{v}_n\}$. Se denota $\text{gen}(\mathbf{v}_1, \dots, \mathbf{v}_n)$.

**Ejemplos**:

- $\text{gen}((1,0), (0,1)) = \mathbb{R}^2$.
- En $\mathbb{R}^3$, $\text{gen}((-1,1,1), (3,4,4))$ es un plano.

---

### **4. Independencia Lineal**

- **Linealmente independiente (LI)**: La única solución para $c_1 \mathbf{v}_1 + \dots + c_n \mathbf{v}_n = \mathbf{0}$ es $c_1 = \dots = c_n = 0$.
- **Linealmente dependiente (LD)**: Existen soluciones no triviales.

**Propiedades**:

- Todo conjunto con el vector nulo es LD.
- En $\mathbb{R}^n$, más de $n$ vectores son LD.

**Ejemplo**:

- $\{(1,2), (0,3)\}$ en $\mathbb{R}^2$ es LI.

---

### **5. Bases y Dimensión**

- **Base**: Conjunto LI y generador de $V$.
- **Dimensión**: Número de vectores en una base.

**Ejemplos**:

- Base canónica de $\mathbb{R}^n$: $\{(1,0,\dots,0), \dots, (0,\dots,0,1)\}$.
- $\dim(\mathbb{R}^n) = n$; $\dim(M_{2 \times 2}) = 4$.

**Aplicación**:

- Para $S = \{(x,y,z) \in \mathbb{R}^3 \mid 2x - y + z = 0\}$, una base es $\{(1,2,0), (0,1,1)\}$ y $\dim(S) = 2$.

---

### **6. Coordenadas**

- **Coordenadas en una base $B$**: Escalares únicos $c_1, \dots, c_n$ tales que $\mathbf{u} = c_1 \mathbf{v}_1 + \dots + c_n \mathbf{v}_n$.

**Ejemplo**:

- $\mathbf{u} = (1,2)$ en base $B = \{(1,2), (-1,1)\}$ tiene coordenadas:
  $$
  [\mathbf{u}]_B = \begin{bmatrix} 1 \\ 1 \end{bmatrix}.
  $$

---

### **7. Producto Interior**

- **Definición**: Función $\langle \mathbf{u}, \mathbf{v} \rangle$ que cumple:
  1. Simetría: $\langle \mathbf{u}, \mathbf{v} \rangle = \langle \mathbf{v}, \mathbf{u} \rangle$.
  2. Linealidad: $\langle \mathbf{u} + \mathbf{v}, \mathbf{w} \rangle = \langle \mathbf{u}, \mathbf{w} \rangle + \langle \mathbf{v}, \mathbf{w} \rangle$.
  3. Homogeneidad: $\langle k \mathbf{u}, \mathbf{v} \rangle = k \langle \mathbf{u}, \mathbf{v} \rangle$.
  4. Positividad: $\langle \mathbf{u}, \mathbf{u} \rangle \geq 0$, con igualdad solo si $\mathbf{u} = \mathbf{0}$.

**Ejemplos**:

- Producto escalar usual en $\mathbb{R}^n$.
- $\langle (u_1, u_2), (v_1, v_2) \rangle = 2u_1v_1 - 3u_2v_2$ no es producto interior (no cumple positividad).

---

**Notas Adicionales**:

- **Bases ortogonales**: Vectores ortogonales entre sí.
- **Bases ortonormales**: Bases ortogonales con vectores de norma 1 (ej: base canónica de $\mathbb{R}^n$).
- La dimensión es invariante para todas las bases de $V$.
