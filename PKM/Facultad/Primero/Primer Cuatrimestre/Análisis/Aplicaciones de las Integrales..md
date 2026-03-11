---
id: Aplicaciones de las Integrales.
aliases: []
tags: []
---

## Área bajo la curva

- Cuando $f(x)~\geq~0$ para todo x en $[a,b]$. El área delimitada por la gráfica de _f_, el eje de las abscisas y las rectas $x~=~a$ y $x~=~b$, esta dada por la siguiente integral: $$A~=~\int_{a}^{b}{f(x)~dx}$$
- Cuando $f(x)~\leq~0$ para todo x en $[a,b]$. El área delimitada por la gráfica de _f_, el eje de las abscisas y las rectas $x~=~a$ y $x~=~b$, esta dada por la siguiente integral: $$A~=~\bigg|\int_{a}^{b}{f(x)~dx}\bigg|$$
- Cuando los valores de _f(x)_ cambian de signo en $[a,b]$:

  Para determinar el área comprendida entre la gráfica de _f_, el eje de las abscisas y las rectas $x~=~a$ y $x~=~b$ se procede de la siguiente manera:

  1. Determinamos las intersecciones del gráfico de _f_ con el eje x en el intervalo $[a,b]$.
  2. Subdividimos $[a,b]$ usando los puntos hallados en el inciso anterior.
  3. Integramos _f_ sobre cada sub-intervalo.
  4. Sumamos los valores absolutos de las integrales calculadas en el apartado anterior. $$A~=~\int_{a}^{x_1}{f(x)~dx}~+~\bigg|\int_{x_1}^{x_2}{f(x)~dx}~\bigg|~+~\int_{x_2}^{x_3}{f(x)~dx}~+~\bigg|\int_{x_3}^{b}{f(x)dx}~\bigg|$$

- Cuando $ f(y)\geq0$ para todo x del $[a,b]$ el área de la region delimitada por la gráfica de _f_, el eje y, las rectas $y~=~a$ y $y~=~b$ esta dada por la siguiente integral:$$ A~=~\int_{a}^{b}{f(y)~dy} $$

## Áreas entre Curvas

![[Pasted image 20230620114948.png]]
El área de la región entre ambas curvas está dada por: $$\begin{align*} &A~=~\int_{a}^{b}{[f(x)~-~g(x)]~dx~}~~~siendo~f(x)\geq g(x)\\ &A~=~\Bigg|\int_{a}^{b}{[f(x)~-~g(x)]~dx}\Bigg|~~~siendo~f(x)\leq g(x)\end{align*}$$
Para verificar el resultado obtenido, se puede dividir el $[a,b]$ entre _n-subintervalos_, cada uno de base $\Delta x$ y altura $f(x_i)~-~g(x_i)$ siendo $x_i$ punto en el _i-ésimo_ sub-intervalo.

- El área del rectángulo esta dada por la siguiente fórmula:$$A_i~=~[f(x_i)~-~g(x_i)]~\Delta x$$
- Por adición de las áreas de los n rectángulos: $$\lim_{n~\to~+\infty}{\sum_{i~=~1}^{n}{[f(x_i)~-~g(x_i)]}~\Delta x}$$
- Como f y g son continuas en $[a,b]$, f-g también es continua en ese intervalo y el límite existe.
- Así que el área de la region dada es: $$A~=~\lim_{n~\to~+\infty}{\sum_{i~=~1}^{n}{[f(x_i)~-~g(x_i)]}~\Delta x}~~~=~~~\int_{a}^{b}{[f(x)~-~g(x)]~dx}$$
  Para calcular el área entre las gráficas de _f_ y _g_, cuyas gráficas son verticales y continuas en el intervalo $[c,d]$:

- Se buscan las ordenadas $y_1,y_2,~\dots~,y_n$ pertenecientes al $[c,d]$ de los puntos donde las gráficas se intersecan. Para ello se resuelve la siguiente ecuación:$$f(y)~=~g(y)$$
- Se encuentra el área de la siguiente manera:$$A~=~\Bigg|~\int_{c}^{y_1}{[f(y)~-~g(y)]~dy}~\Bigg|~+~\dots~+~\Bigg|\int_{y_n}^{d}{[f(y)~-~g(y)]~dy}~\Bigg|$$

## <mark style="background: #ADCCFFA6;">Longitud de un arco de Curva.</mark>

Si _f'_ es continua en $[a,b]$ entonces la longitud del arco de curva de _f_ desde el punto A(a, f(a)) hasta B(b, f(b)) está dada por: $$L~=~\int_{a}^{b}{\sqrt{1+[~f'(x)~]^{2}}~dx}$$

#### <mark style="background: #FFF3A3A6;">Demostración:</mark>

- Sea P una partición de $[a,b]$.
- Suponiendo que P divide al $[a,b]$ en n subintervalos de igual longitud: $$\Delta x~=~\frac{b-a}{n}$$
- Por cada valor de $x_i$ obtenemos un punto P<sub>i</sub> que si unimos estos puntos tendremos una poligonal cuya longitud sera una aproximación de la longitud L de la curva AB.
- Para calcular la longitud de dicha poligonal, se toma uno de los n segmentos, y se calcula su longitud:
  - $P_{i~-~1}(~x_{i~-~1},f(x_{i-1})~)$
  - $P_i(~x_i,f(x_i)~)$
- Como queda determinado un triángulo rectángulo, nos queda:
  - <u>Cateto 1:</u> $\Delta x~=~x_1~-~x_{i-1}$
  - <u>Cateto 2:</u> $f(x_i)~-~f(x_{i-1})$
- Aplicando Teorema de Pitágoras: $$l_i~=~\sqrt{(x_{i}~-~x_{i-1})^2~+~(f(x_i)~-~f(x_{i-1}))^2}$$
- Por hipótesis _f'_ es continua en $[a,b]$, por lo tanto _f_ es derivable en $(a,b)$ y también en el intervalo $(x_{i-1}~,~x_{i})\subset(a,b)$
- Vemos que _f_ es continua en $[x_{i-1},x_i]$ y derivable en $(x_{i-1},x_i)$ y por el Teorema de Lagrange(Teorema del valor medio del cálculo diferencial) existe al menos un valor $c_i~\in~(x_{i-1},~x_i)$ tal que: $$ \begin{align*} &f'(c*i)~=~\frac{f(x_i)~-~f(x*{i-1})}{x*i~-~x*{i-1}}\\ &f(x*i)-f(x*{i-1})~=~f'(c*i)~.~(x_i~-~x*{i-1})\end{align*} $$
- Reemplazando en $l_i$:$$l_i~=~\sqrt{(x_{i}~-~x_{i-1})^2~+~(f'(c_i))^2~.~(x_i~-~x_{i-1})^2}$$
- Trabajando algebraicamente: $$\begin{align*} &l_i~=~\sqrt{(x_{i}~-~x_{i-1})^2~.~[1~+~(f'(c_i))^2]}\\ &l_i~=~(x_i~-~x_{i-1})\sqrt{[1~+~(f'(c))^2]}\\ &l_i~=~\Delta x~.~\sqrt{[1~+~(f'(c))^2]}\end{align*}$$
- La medida de la poligonal se obtendrá de sumar las medidas de los n segmentos: $$l_{poligonal}~=~\sum_{i=1}^{n}\Delta x~.~\sqrt{[1~+~(f'(c))^2]}$$
- La longitud de la poligonal es una aproximación de la medida de la longitud de la curva: $$L \approx l_{poligonal}~=~\sum_{i=1}^{n}\Delta x~.~\sqrt{[1~+~(f'(c))^2]}$$
- Esta aproximación será mejor si se particiona en mayor cantidad de subintervalos el segmento $[a,b]$, por lo tanto L será: $$L~=~\lim_{n~\to~\infty}{\sum_{i=1}^{n}~\sqrt{[1~+~(f'(c))^2]}~.~\Delta x~}$$
  ![[Pasted image 20230620161027.png]]

- Al ser $f'$ continua en $[a,b]$, se cumple que $\sqrt{1+[f´(x)]^2}$ es continua, lo que implica que la longitud del arco L de _f_ entre $a$ y $b$ puede expresarse como:$$L~=~\lim_{n~\to~\infty}{\sum_{i=1}^{n}~\sqrt{[1~+~(f'(c))^2]}~.~\Delta x~}~=~\int_{a}^{b}{\sqrt{[1~+~(f'(c))^2]}~dx}$$ ![[Pasted image 20230620160441.png]]

## <mark style="background: #ADCCFFA6;">Sólidos en Revolución.</mark>

Es aquel sólido que resulta de hacer girar una región plana alrededor de una recta, la cual se denomina eje de revolución.

#### <u>Sección Transversal</u>

Una sección transversal de un sólido S es la región plana formada por la intersección de S con un plano.

#### <u>Área de la superficie de un sólido de Revolución.</u>

El área de un sólido de revolución se deriva de la fórmula para el área de la superficie lateral de un cono recto truncado, cuando el segmento gira alrededor de su eje de revolución su área lateral es: $$A~=~2.\pi.r.L$$Siendo:

- r el radio medio entre $r_1$(radio del extremo izquierdo del segmento de la recta) y $r_2$(radio del extremo derecho del segmento de la recta)
- L a la longitud del segmento de la recta

Entonces siendo _f_ una función continua en el intervalo $[a,b]$ y cuya derivada también es una función continua. El área de la superficie de un sólido de revolución al girar _f_ alrededor de un eje horizontal es: $$A~=~2~\pi~\int_{a}^{b}f(x)\sqrt{1+[f'(x)]^2}~dx$$
Ahora si $x~=~g(y)$ una función continua definida en el intervalo $[c,d]$ y su derivada también es continua en dicho intervalo, entonces la fórmula del área del sólido al girar g alrededor de un eje vertical es: $$A~=~2~\pi~\int_{a}^{b}g(y)\sqrt{1+[g'(y)]^2}~dx$$

#### <u>Volumen de un sólido de Revolución: Método de los discos. </u>

Sea _f_ una función continua en los intervalos $[a,b]$. El
volumen de un sólido de revolución generado al girar _f_ alrededor de un eje horizontal es: $$V~=~\pi~\int_{a}^{b}{[f(x)]^2~dx}$$

##### <mark style="background: #FFF3A3A6;">Demostración:</mark>

1. Para calcular el volumen del sólido debemos particionar el intervalo $[a,b]$ deforma que el intervalo nos quede divido en _n sub-intervalos_ de igual longitud $\Delta x$, con $||P||~=~máx~\{\Delta x_i\}~=~\Delta x$
2. Elegimos en cada sub-intervalo un $x_{i}^*$, de manera tal que cada subintervalo tenga por altura $f(x_{i}^*)$, la cual será el radio de la base de los cilindros originados.
3. De esta manera el volumen de uno de esos cilindros será:$$\Delta V_i~=~\pi~[f(x_{i}^*)]^2~.~\Delta x$$
4. La sumatorio de cada uno de esos volúmenes, nos queda la suma de Riemann: $$\sum_{i=1}^{n}{~\pi~[f(x_{i}^*)]^2~.~\Delta x}$$
5. Si aproximamos el volumen del sólido por el de los discos nos quedaría:$$Volumen~del~Sólido~\approx~\sum_{i=1}^{n}{~\pi~[f(x_{i}^*)]^2~.~\Delta x_i}$$
6. La aproximación mejora si $||P||~\to~0$ o $n~\to~\infty$:$$V~=~\lim_{n~\to~\infty}{\Bigg(\sum_{i=1}^{n}{~\pi~[f(x_{i}^*)]^2~.~\Delta x_i}\Bigg)}$$
7. Al ser _f_ una función continua en $[a,b]$, _f_ es integrable entonces por [[Integrales.|definición de la Integral de Riemann:]] $$V~=~\lim_{n~\to~\infty}{\Bigg(\sum_{i=1}^{n}{~\pi~[f(x_{i}^*)]^2~.~\Delta x_i}\Bigg)}~=~\pi~\int_{a}^{b}{[f(x)]^2~dx}$$

Ahora si tenemos _g_ que es continua y esta definida en el intervalo $[c,d]$ la hacemos girar sobre un eje vertical el volumen será: $$V~=~\pi~\int_{a}^{b}{[g(y)]^2~dy}$$

#### <u>Método de las arandelas o de los anillos.</u>

Sea _f_ y _g_ dos funciones continuas definidas en el intervalo $[a,b]$ y sea $f(x)\geq g(x)$ en dicho intervalo. El volumen de un sólido en revolución generado al girar _f_ y _g_ alrededor de un eje horizontal es: $$V~=~\pi~\int_{a}^{b}{\Bigg([f(x)]^2~-~[g(x)]^2\Bigg)}$$
Este método se puede utilizar en sólido de revolución huecos reemplazando el disco con una arandela o anillo, la cual se forma al girar un rectángulo alrededor del eje.
Siendo r y R los radios interiores y exteriores de la arandela y w es su ancho, el volumen de la arandela se calcula: $$V~=~\pi~(R^2~-~r ^2)~w$$

##### <mark style="background: #FFF3A3A6;">Demostración:</mark>

1. Si al girar dos funciones _f_ y _g_ alrededor del eje x, siendo $f(x)\geq g(x)$ en $[a,b]$, origina en cada $[x_{i-1},~x_{i}]$ una arandela de radio interno $g(x_{i}^*)$ y radio externo $f(x_{i}^*)$, de ancho $\Delta x_i$ $$V~\approx~\pi~\sum_{i=1}^{n}{\Bigg([f(x_{i}^*)]^2~-~[g(x_{i}^*)]^2\Bigg)~.~\Delta x_i}$$
2. Para mejorar la aproximación, aplicamos: $$V~=~\lim_{n~\to~\infty}\Bigg({\pi~\sum_{i=1}^{n}{\Bigg([f(x_{i}^*)]^2~-~[g(x_{i}^*)]^2\Bigg)~.~\Delta x_i}}\Bigg)$$
3. Al ser _f_ y _g_ funciones continua en el intervalo $[a,b]$, entonces _f_ y _g_ son integrables, entonces por definición de Integral de Riemann:$$V~=~\pi~\int_{a}^{b}{\Bigg([f(x)]^2~-~[g(x)]^2\Bigg)~dx}$$

## <mark style="background: #ADCCFFA6;">Integrales Impropias.</mark>

Son aquella integrales las cuales no están acotas o sus funciones no están acotadas, si este ultimo caso ocurre significa que la función presenta una discontinuidad del tipo esencial.
Las Integrales Impropias se dividen en dos especies.

##### <u>Integrales Impropias de Primera Especie:</u>

Son aquellas en las que la función se encuentra definida en un intervalo no acotado, y alguno de los extremos de integración es infinito.

- Cuando _f_ es continua en $[a,+\infty]$: 
$$
\boxed{\int_{a}^{\infty}f(x)~dx~=~\lim_{b~\to~+\infty}\int_{a}^{b}{f(x)~dx}}
$$
  Esta integral <u>converge</u> cuando el límite existe.

- Cuando _f_ es continua en $[\infty,~b]$: $$\boxed{\int_{-\infty}^{b}f(x)~dx~=~\lim_{a~\to~-\infty}\int_{a}^{b}{f(x)~dx}}$$
  Esta integral <u>converge</u> cuando el límite existe.

- Cuando _f_ es continua en $(-\infty,~+\infty)$: $$\boxed{\int_{-\infty}^{+\infty}h(x)~dx~=~\int_{-\infty}^{c}{h(x)~dx}~+~\int_{c}^{+\infty}{h(x)~dx}}$$
  La integral de la izquierda <u>diverge</u> si cualquiera de las integrales impropias de la derecha <u>diverge</u>.

##### <u>Integrales Impropias de Segunda Especie.</u>

En este caso la función se encuentra definida en un intervalo acotado, pero presentan una discontinuidad esencial ya sea en los extremos del intervalo de integración o en algún punto interior o en ambos.

- Cuando _f_ es continua en $[a,b)$ y tiene una discontinuidad infinita en b: $$\boxed{\int_{a}^{b}{f(x)~dx}~=~\lim_{c~\to~b^-}{\int_{a}^{c}{f(x)~dx}}}$$
  Este integral <u>converge</u> si el límite existe.

- Cuando _f_ es continua en $[a,b)$ y tiene una discontinuidad infinita en a: $$\boxed{\int_{a}^{b}{f(x)~dx}~=~\lim_{c~\to~a^+}{\int_{c}^{b}{f(x)~dx}}}$$
  Esta integral <u>converge</u> si el límite existe.

- Cuando _f_ es continua en $[a,b]$, excepto en algún punto _c_ en $(a,b)$:$$\boxed{\int_{a}^{b}{h(x)~dx}~=~\int_{a}^{c}{h(x)~dx}~+~\int_{c}^{b}{h(x)~dx}}$$
  La integral impropia de la izquierda <u>diverge</u> si cualquiera de las integrales de la derecha <u>diverge</u>.
