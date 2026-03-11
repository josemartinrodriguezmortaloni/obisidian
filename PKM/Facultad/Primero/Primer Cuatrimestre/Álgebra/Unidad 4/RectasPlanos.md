# Resumen Completo: Rectas y Planos en el Espacio Tridimensional

---

## 1. Definiciones Básicas

- **Espacio tridimensional**: Coordenadas $(x, y, z)$.
- **Vectores clave**:
  - **Vector director** ($\mathbf{v}$): Dirección de una recta.
  - **Vector normal** ($\mathbf{n}$): Perpendicular a un plano.

---

## 2. Ecuaciones de Rectas

**Definición**: Punto $P_0(x_0, y_0, z_0)$ + vector director $\mathbf{v} = (a, b, c)$.

### Formas de representación:

1. **Paramétricas**:

   $$
   \begin{cases}
   x = x_0 + a t \\
   y = y_0 + b t \\
   z = z_0 + c t
   \end{cases} \quad (t \in \mathbb{R})
   $$

2. **Simétrica/Continua** (si $a, b, c \neq 0$):

   $$
   \frac{x - x_0}{a} = \frac{y - y_0}{b} = \frac{z - z_0}{c}
   $$

3. **Vectorial**:
   $$
   \mathbf{r}(t) = \mathbf{P_0} + t \mathbf{v}
   $$

---

## 3. Ecuaciones de Planos

**Definición**: Punto $P_0(x_0, y_0, z_0)$ + vector normal $\mathbf{n} = (a, b, c)$.

### Formas de representación:

1. **General/Cartesiana**:

   $$
   a(x - x_0) + b(y - y_0) + c(z - z_0) = 0 \quad \text{o} \quad ax + by + cz + d = 0
   $$

2. **Segmentaria** (corta ejes en $(a, 0, 0)$, $(0, b, 0)$, $(0, 0, c)$):

   $$
   \frac{x}{a} + \frac{y}{b} + \frac{z}{c} = 1
   $$

3. **Vectorial** (con vectores $\mathbf{u}$, $\mathbf{v}$ en el plano):
   $$
   \mathbf{r}(s, t) = \mathbf{P_0} + s \mathbf{u} + t \mathbf{v} \quad (s, t \in \mathbb{R})
   $$

---

## 4. Relaciones entre Rectas y Planos

### Intersección:

- **Dos planos no paralelos**: Sistema de ecuaciones lineales.
- **Recta y plano**: Sustituir ecuaciones paramétricas en el plano.

### Paralelismo:

- **Planos**: $\mathbf{n_1} = k \mathbf{n_2}$.
- **Recta y plano**: $\mathbf{v} \cdot \mathbf{n} = 0$.

### Perpendicularidad:

- **Planos**: $\mathbf{n_1} \cdot \mathbf{n_2} = 0$.
- **Recta y plano**: $\mathbf{v} \parallel \mathbf{n}$.

---

## 5. Distancias y Ángulos

### Distancias:

1. **Punto $Q(x_1, y_1, z_1)$ a plano $ax + by + cz + d = 0$**:

   $$
   D = \frac{|a x_1 + b y_1 + c z_1 + d|}{\sqrt{a^2 + b^2 + c^2}}
   $$

2. **Punto $Q$ a recta**:

   $$
   D = \frac{\| \mathbf{P_0Q} \times \mathbf{v} \|}{\| \mathbf{v} \|}
   $$

3. **Rectas que se cruzan**:
   $$
   D = \frac{| (\mathbf{P_2} - \mathbf{P_1}) \cdot (\mathbf{v_1} \times \mathbf{v_2}) |}{\| \mathbf{v_1} \times \mathbf{v_2} \|}
   $$

### Ángulos:

1. **Entre planos**:

   $$
   \cos \theta = \frac{|\mathbf{n_1} \cdot \mathbf{n_2}|}{\| \mathbf{n_1} \| \| \mathbf{n_2} \|}
   $$

2. **Recta y plano**:
   $$
   \theta = 90^\circ - \phi, \quad \cos \phi = \frac{|\mathbf{v} \cdot \mathbf{n}|}{\| \mathbf{v} \| \| \mathbf{n} \|}
   $$
