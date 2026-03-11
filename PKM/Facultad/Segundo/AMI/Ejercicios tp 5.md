# Ejercicio 1 - c

Para resolver la integral doble $\int_{0}^{1} \int_{0}^{x} \sqrt{1 - x^2} , dy , dx$, primero integramos con respecto a (y).

La integral interna es:

$$
\int_{0}^{x} \sqrt{1 - x^2} , dy
$$

Dado que $\sqrt{1 - x^2}$ es constante con respecto a (y), podemos sacarlo fuera de la integral:

$$
\sqrt{1 - x^2} \int_{0}^{x} dy
$$

La integral de (dy) es simplemente (y), evaluada desde 0 hasta (x):

$$
\sqrt{1 - x^2} \left[ y \right]_{0}^{x} = \sqrt{1 - x^2} \cdot (x - 0) = x \sqrt{1 - x^2}
$$

Ahora, integramos esta expresión con respecto a (x):

$$
\int_{0}^{1} x \sqrt{1 - x^2} , dx
$$

Para resolver esta integral, utilizamos la sustitución $u = 1 - x^2$. Entonces, $du = -2x , dx$ o $dx = -\frac{du}{2x}$.

Cuando (x = 0), (u = 1). Cuando (x = 1), (u = 0).

Reescribimos la integral en términos de (u):  
$$\int_{1}^{0} x \sqrt{u} \left(-\frac{du}{2x}\right) = \int_{1}^{0} -\frac{1}{2} \sqrt{u} , du $$

Cambiamos los límites de integración para que vayan de menor a mayor:

$$
\int_{0}^{1} \frac{1}{2} \sqrt{u} , du
$$

La integral de $\sqrt{u}$ es $\frac{2}{3} u^{3/2}$:

$$
\frac{1}{2} \int_{0}^{1} u^{1/2} , du = \frac{1}{2} \left[ \frac{2}{3} u^{3/2} \right]_{0}^{1} = \frac{1}{2} \cdot \frac{2}{3} \left[ u^{3/2} \right]_{0}^{1} = \frac{1}{3} \left[ u^{3/2} \right]_{0}^{1}
$$

Evaluamos en los límites:

$$
\frac{1}{3} \left( 1^{3/2} - 0^{3/2} \right) = \frac{1}{3} (1 - 0) = \frac{1}{3}
$$

Por lo tanto, el resultado de la integral es:

$$
\boxed{\frac{1}{3}}
$$

# Ejercicio 1 - d

Para resolver la integral doble

$$
\int_{0}^{2} \int_{3y^2 - 6y}^{2y - y^2} 3y , dx , dy
$$

primero integramos con respecto a (x).

La integral interna es:

$$
\int_{3y^2 - 6y}^{2y - y^2} 3y , dx
$$

Dado que (3y) es constante con respecto a (x), podemos sacarlo fuera de la integral:

$$
3y \int_{3y^2 - 6y}^{2y - y^2} dx
$$

La integral de (dx) es simplemente (x), evaluada desde $3y^2 - 6y$ hasta $2y - y^2$:

$$
3y \left[ x \right]_{3y^2 - 6y}^{2y - y^2} = 3y \left( (2y - y^2) - (3y^2 - 6y) \right)
$$

Simplificamos la expresión dentro del paréntesis:

$$
3y \left( 2y - y^2 - 3y^2 + 6y \right) = 3y \left( 8y - 4y^2 \right) = 3y \cdot 4y (2 - y) = 12y^2 (2 - y)
$$

Ahora, integramos esta expresión con respecto a (y):

$$
\int_{0}^{2} 12y^2 (2 - y) , dy
$$

Distribuimos $12y^2$ dentro del paréntesis:

$$
\int_{0}^{2} (24y^2 - 12y^3) , dy
$$

Integramos término a término:

$$
\int_{0}^{2} 24y^2 , dy - \int_{0}^{2} 12y^3 , dy
$$

La integral de $24y^2$ es $24 \cdot \frac{y^3}{3} = 8y^3$, y la integral de (12y^3) es $12 \cdot \frac{y^4}{4} = 3y^4$:

$$
8y^3 \bigg|_{0}^{2} - 3y^4 \bigg|_{0}^{2}
$$

Evaluamos en los límites:

$$
8(2)^3 - 3(2)^4 - (8(0)^3 - 3(0)^4)
$$

Simplificamos:

$$
8 \cdot 8 - 3 \cdot 16 = 64 - 48 = 16
$$

Por lo tanto, el resultado de la integral es:

$$
\boxed{16}
$$

# Ejercicio 2

## A)\_

Para graficar la región de integración y expresar la integral invirtiendo el orden de integración, primero identificamos los límites de integración originales:

$$
\int_{0}^{2} \int_{3y^2 - 6y}^{2y - y^2} 3y , dx , dy
$$

La región de integración está definida por las curvas $x = 3y^2 - 6y$ y $x = 2y - y^2$ para $y$ en el intervalo $[0, 2]$.

### Paso 1: Graficar la región de integración

Para graficar la región de integración, primero encontramos los puntos de intersección de las curvas$x = 3y^2 - 6y$ y $x = 2y - y^2$.

Igualamos las dos ecuaciones:

$$
3y^2 - 6y = 2y - y^2
$$

Reorganizamos la ecuación:

$$
3y^2 + y^2 - 6y - 2y = 0
$$

Simplificamos:

$$
4y^2 - 8y = 0
$$

Factorizamos:

$$
4y(y - 2) = 0
$$

Entonces, $y = 0$ o $y = 2$.

Ahora, evaluamos las curvas en estos puntos para encontrar los límites en (x):

Para $y = 0$:

$$
x = 3(0)^2 - 6(0) = 0
$$

$$
x = 2(0) - (0)^2 = 0
$$

Para (y = 2):

$$
x = 3(2)^2 - 6(2) = 12 - 12 = 0
$$

$$
x = 2(2) - (2)^2 = 4 - 4 = 0
$$

Entonces, los puntos de intersección son ((0, 0)) y ((0, 2)).

### Paso 2: Invertir el orden de integración

Para invertir el orden de integración, necesitamos expresar los límites en términos de (x) primero y luego en términos de (y).

La región de integración en términos de (x) y (y) es:

$$
0 \leq y \leq 2
$$

$$
3y^2 - 6y \leq x \leq 2y - y^2
$$

Para invertir el orden de integración, encontramos los límites en (y) para un (x) dado.

La región de integración se puede dividir en dos partes:

1. Para (0 \leq x \leq 2), (y) varía de (0) a (\sqrt{\frac{x}{3} + 2}).
2. Para (2 \leq x \leq 4), (y) varía de (0) a (\frac{4 - x}{2}).

Entonces, la integral invertida es:

$$
\int_{0}^{2} \int_{0}^{\sqrt{\frac{x}{3} + 2}} 3y , dy , dx + \int_{2}^{4} \int_{0}^{\frac{4 - x}{2}} 3y , dy , dx
$$

### Paso 3: Graficar la región de integración

Aquí está la gráfica de la región de integración:

```python
import numpy as np
import matplotlib.pyplot as plt

# Definir las funciones
def f1(y):
    return 3*y**2 - 6*y

def f2(y):
    return 2*y - y**2

# Crear un rango de valores para y
y = np.linspace(0, 2, 400)

# Crear la gráfica
plt.figure(figsize=(8, 6))
plt.plot(f1(y), y, label='$x = 3y^2 - 6y$')
plt.plot(f2(y), y, label='$x = 2y - y^2$')
plt.fill_betweenx(y, f1(y), f2(y), where=(f1(y) <= f2(y)), color='gray', alpha=0.5)

# Etiquetas y leyenda
plt.xlabel('$x$')
plt.ylabel('$y$')
plt.title('Región de Integración')
plt.legend()
plt.grid(True)
plt.show()

```

### Integral invertida

La integral invertida es:

$$
\int_{0}^{2} \int_{0}^{\sqrt{\frac{x}{3} + 2}} 3y , dy , dx + \int_{2}^{4} \int_{0}^{\frac{4 - x}{2}} 3y , dy , dx
$$

## B)\_

Para invertir el orden de integración de la integral

$$
\int_{0}^{1} \int_{x^4}^{x} f(x, y) \, dy \, dx
$$

primero identificamos la región de integración en el plano \(xy\).

### Paso 1: Identificar la región de integración

La región de integración está definida por:

- $0 \leq x \leq 1$
- $x^4 \leq y \leq x$

### Paso 2: Graficar la región de integración

Para graficar la región de integración, consideramos las curvas \(y = x^4\) y \(y = x\).

### Paso 3: Invertir el orden de integración

Para invertir el orden de integración, necesitamos expresar los límites en términos de \(y\) primero y luego en términos de \(x\).

La región de integración en términos de \(y\) y \(x\) es:

$0 \leq y \leq 1$
$y^{1/4} \leq x \leq y$

Entonces, la integral invertida es:

$$
\int_{0}^{1} \int_{y^{1/4}}^{y} f(x, y) \, dx \, dy
$$

### Paso 4: Graficar la región de integración

Aquí está la gráfica de la región de integración:

```python
import numpy as np
import matplotlib.pyplot as plt

# Definir las funciones
def f1(x):
    return x**4

def f2(x):
    return x

# Crear un rango de valores para x
x = np.linspace(0, 1, 400)

# Crear la gráfica
plt.figure(figsize=(8, 6))
plt.plot(x, f1(x), label='$y = x^4$')
plt.plot(x, f2(x), label='$y = x$')
plt.fill_between(x, f1(x), f2(x), where=(f1(x) <= f2(x)), color='gray', alpha=0.5)

# Etiquetas y leyenda
plt.xlabel('$x$')
plt.ylabel('$y$')
plt.title('Región de Integración')
plt.legend()
plt.grid(True)
plt.show()
```

### Integral invertida

La integral invertida es:

$$
\int_{0}^{1} \int_{y^{1/4}}^{y} f(x, y) \, dx \, dy
$$
