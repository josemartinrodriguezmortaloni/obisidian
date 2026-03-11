## <mark style="background: #ADCCFFA6;">I. Derivada Primitiva.</mark>

Sea *f* una función definida en cierto intervalo. La función F es una primitiva o antiderivada de *f*, si para todo *x* del intervalo se cumple que: 
$$F´(x)=f(x)$$
#### <u>Teorema.</u> 
Si F y G son dos primitivas cualesquiera de la función *f* en el intervalo *I* entonces F y G difieren de una constante, siendo C un número real: 
$$ G = F+C $$
En consecuencia, la primitiva más general de *f* en *I* en una *familia* de funciones F(x)+C cuyas gráficas son traslaciones verticales unas de otras. Podemos elegir una primitiva en particular de la familia, asignando un valor específico a C. 

### A. <u>Notación Integral Indefinida.</u> 

El conjunto de todas las antiderivadas de *f* es la **integral indefinida** de *f* con respecto a x, denotada mediante: 
$$\int f(x)\ =\ F(x)\ +\ C$$
#### <u>Teorema 1.</u>

Sean *f* y *g* dos funciones definidas en un conjunto D. 
Si F y G son respectivamente primitivas de *f* y *g* en D, entonces F + o - G es una primitiva de f + o - g.
En símbolos:
$$\int [\ f(x)\ \pm\ g(x)\ ] \ dx = \int f(x)\ dx \ \pm\int g(x)\ dx $$
$$ \int [\ f(x)\ \pm\ g(x)\ ]\ dx = F(x)\ \pm\ G(x)\ +\ C$$
#### <u>Teorema 2</u>.

Sea *f* una función definida en un conjunto D, *k* un número real y F una primitiva de F en D. 
En símbolos: 
$$ \int k \ f(x) \ dx\ =\ k \int f(x) \ dx$$ $$\int k\ f(x)\ dx\ = k\ F(x)\ +\ C$$ 
## <mark style="background: #ADCCFFA6;">II. Métodos de Integración.</mark> 

### A. <u>Integración por Descomposición.</u>

Dada una integral, la podemos descomponer en varias integrales, aplicando las propiedades anteriores y luego integrar utilizando la tabla de integrales inmediatas. 

### B. <u>Regla de Sustitución.</u> 

Este método se aplica en aquellos casos en los que en el integrando hay una función compuesta. 

Sean *f* y *g* funciones continuas en el intervalo $[\ a,b\ ]$, suponiendo que $g\ '(x)$ es continua en $[\ a,b\ ]$, entonces: 
$$\int f(g(x))\ g\ '(x)\ dx\ = \int f(u)\ du $$
En conclusión se realiza un cambio de variable, done llamaremos *u*=*g(x)*, luego *du* = *g'(x) dx* , quedándonos: 
$$\int f(u)\ du $$

**<u>Demostración:</u>** La regla es cierta porque, de acuerdo con la regla de la cadena, $F(g(x))$ es una primitiva de $f(g(x)) \cdot g\ '(x)$ siempre que *F* sea una primitiva de *f*:  
$$
\frac {d}{dx}F(g(x))\ =\ F\ '(g(x)) \cdot g\ '(x) 
= f(g(x)) \cdot g\ '(x). 
$$

Si hacemos la sustitución $u\ =\ g(x)$ entonces
$$
\begin{align*}
\int f(g(x))g\ '(x)\, dx &= \int \frac{d}{dx}F(g(x)) \, dx\\ 
&=\ F(g(x))\ +\ C \\ 
&=\ F(u)\ +\ C \\
&=\int F\ '(u) \, du\\
&=\int f(u) \,du\\
\end{align*}
$$


La regla de sustitución proporciona el siguiente método para evaluar la integral
$$\int f(g(x))g\ 'g(x) \, dx,$$

cuando *f* y *g'* son funciones continuas:

1. Sustituir $u\ = g(x)$ y $du\ = g\ '(x)\ dx$ para obtener la integral. $$\int f(u) \, du $$
2. Integrar respecto de *u*.
3. Reemplazar *u* por *g(x)* en el resultado. 

### C. <u>Integración por Sustitución: Integrales Trigonométricas.</u> 

##### <u>1.Potencia Impar de Senos y Cosenos.</u>
Integrales de la forma: $\boxed{\int sen{^n}(x) \, dx}$ y $\boxed{\int cos{^n}(x)\ dx}$ siendo **n impar**.
Se debe descomponer la función trigonométrica en dos factores: uno de potencia n-1 y el otro de potencia 1. Luego es necesario utilizar la identidad pitagórica.

##### <u>2.Potencia Par de Senos y Cosenos.</u>  
Integrales de la forma: $\boxed{\int sen{^n}(x) \, dx}$ y $\boxed{\int cos{^n}(x)\ dx}$ siendo **n par**.
Usamos las identidades: $\boxed{sen^2(x)=\frac{1-cos(2x)}{2}}$ y $\boxed{cos^2(x)=\frac{1+cos(2x)}{2}}$ 
Luego de realizada la sustitución trigonométrica adecuada, se desarrolla el polinomio de senos y cosenos y se resuelven cada uno de los sumandos, los de potencia par por este método y los de potencia impar por el método anterior. 

### D. <u>Integración por Partes.</u>

La fórmula de derivación para el producto de dos funciones nos permite resolver integrales cuyo integrando es el producto de dos funciones de naturaleza: 
$$[f(x)\ g(x)]'\ =\ f\ '(x)\ g(x)\ +\ f(x)\ g\ '(x)$$
Sean f y g funciones sobre la misma variable x y derivables. 

<u>Pasos para su resolución:</u>
1. Se integra miembro a miembro:$$\int[f(x)g(x)]\ 'dx\ =\ \int[\ f\ '(x)\ g(x)\ +\ f(x)\ g\ '(x)\ ]dx$$
2. Aplicamos la definición de integral indefinida: **f(x)g(x) es una primitiva de f'(x)g(x) + f(x)g'(x)**$$f(x)\ g(x)\ =\ \int f\ '(x)g(x)\ dx\ +\ \int f(x)g(x)\ '(x)\ dx\ \ (1) $$
3. Luego se realizan las siguientes sustituciones: $u=\ f(x)$ y $v=g(x)\ \ (2)$,$du = f\ '(x)\ dx$ y $dv=g\ '(x)dx\ \ (3)$ 
4. Reemplazando (2) y (3) en (1): $$u\ v= \int v\ du\ + \int u\ dv$$
5. Luego $\int u\ dv\ =\ u\ v\ - \int v\ du$ esta última expresión es la fórmula de integración por partes.

Para resolver una integral por este método, se deben elegir $u$ y $dv$ en la integral, luego calcular $du$ y $v$ y finalmente aplicar la fórmula de integración por partes. 
### E. <u>Integración de funciones racionales: Descomposición en fracciones simple.</u>

Para integrar el cociente de funciones polinómicas: $$\int \frac{P(x)}{Q(x)} dx $$
1. Para aplicar  este método es necesario que el grado del polinomio del numerador *P(x)* sea menor que el grado del polinomio denominador *Q(x)*.
2. Si el grado del polinomio *P(x)* es mayor o igual que el grado del polinomio *Q(x)* antes de aplicar algún método para integrar se debe realizar la división de polinomios.
3. Al efectuar la división de dos funciones polinómicas se obtiene: $$\frac{P(x)}{Q(x)}\ = C(x)\ +\ \frac{R(x)}{Q(x)} \Longrightarrow \int[\frac{P(x)}{Q(x)}]\ dx\ =\ \int C(x)\ dx\ +\ \int [\frac{R(x)}{Q(x)}]\ dx$$
El método de descomposición en fracciones simples consiste en: 
* Hallar las raíces del polinomio denominador.
* Factorizar el denominador en base a las raíces encontradas.
* Se procede según el tipo de raíces del denominador. 

**<u>Primer Caso: Raíces Reales Simples</u>**
Dado el cociente: $$\frac{P(x)}{Q(x)}\ donde\ Gr[P(x)]<\ Gr[Q(x)]$$
* Hacemos $Q(x)\ =\ 0$ para hallar las raíces del denominador. 
* Si las raíces $Q(x)$ son $x_{1},x_{2},\dots,x_{n}$ reales y distintas entre sí, el denominador se puede factorizar como: $$Q(x)\ =\ a_{n}(x-x_{1})(x-x_{2})\dots(x-x_{n})$$
* Luego $$\frac{P(x)}{Q(x)}\ =\ \frac{P(x)}{a_{n}(x-x_{1})(x-x_{2})\dots(x-x_{n})}$$
* La descomposición correspondiente es:$$\frac{P(x)}{Q(x)}=\frac{1}{a_{n}}[\frac{A_{1}}{(x-x_{1})}\ +\ \frac{A_{2}}{(x-x_{2})}+\ \dots\ + \frac{A_{n}}{(x-x_{n})}]$$
* Siendo $A_{1};A_{2};\dots;A_{n}$ constantes a determinar. 
* Finalmente se procede a integrar:$$\int \left[ \frac{P(x)}{Q(x)} \right] \, dx=\ \frac{1}{a_{n}}\left[\int\frac{A_{1}}{(x-x_{1})}\ dx +\ \int \frac{A_{2}}{(x-x_{2})}\ dx+\ \dots\ + \int \frac{A_{n}}{(x-x_{n})}\ dx \right]$$ 
**<u>Segundo Caso: Raíces Reales Múltiples</u>**
* Hacemos $Q(x)\ = 0$ para hallar las raíces del denominador.
* Si las raíces de $Q(x)$ son $x_{1},x_{2},\dots,x_{n}$ reales e iguales, el denominador se puede factorizar como: $$Q(x)\ =\ a_{n}(x-x_{1}){^n}$$
* Luego $$\frac{P(x)}{Q(x)}\ =\ \frac{P(x)}{a_{n}(x-x_{1}){^n}}$$
* La descomposición correspondiente es: $$\frac{P(x)}{Q(x)}\ =\ \frac{1}{a_{n}}[\frac{A_{1}}{(x-x_{1})}\ +\ \frac{A_{2}}{(x-x_{2}){^2}}\ +\ \dots\ \frac{A_{n}}{(x-x_{n}){^n}}]$$ Siendo $A_{1},A_{2},\dots,A_{n}$ constantes a determinar. 
* Finalmente se procede a integrar: $$\int \left[ \frac{P(x)}{Q(x)} \right]dx\ =\ \frac{1}{a_{n}}\left[ \int\frac{A_{1}}{(x-x_{1})}\ dx\ +\ \int\frac{A_{2}}{(x-x_{2}){^2}}\ dx\ +\ \dots\ \int\frac{A_{n}}{(x-x_{n}){^n}}\ dx \right]$$
### F. <u>Integración de funciones irracionales.</u>

Dada la función irracional de la forma: $$\boxed{f(x)\ = \sqrt {a-bx{^2}}}$$
* Realizamos la siguiente sustitución$$x\ = \sqrt{\frac{a}{b}}sen(t) \Longrightarrow\ dx\ =\ \sqrt{\frac{a}{b}}cos(t)\ dt$$
* Luego$$\begin{align*}\\ &f(x)\ = \sqrt{a-b(\sqrt{\frac{a}{b}}\ sen(t))^2}\ \Longrightarrow f(x)\ = \sqrt{a-b\ \frac{a}{b}\ sen(t)}\\ & f(x)\ =\ \sqrt{a-a\ sen^2(t)} \Longrightarrow f(x)\ =\ \sqrt{a(1-sen^2(t))} \Longrightarrow f(x)\ =\ \sqrt{a} cos(t) \end{align*}$$
* Finalmente se procede a integrar: $$\begin{align*} &\int \sqrt{a-bx^2}\ dx\ =\ \int \sqrt{a}\ cos(t)\ \sqrt{\frac{a}{b}}\ cos(t)\ dt\\ &\Longrightarrow \boxed{\int \sqrt{a-bx^2}\ dx\ =\ \frac{a}{\sqrt{b}}\int cos^2(t)\ dt} \end{align*}$$
* Después de integrar, se deben realizar las sustituciones que correspondan para que las primitivas encontradas dependan de la variable x

Dada la función irracional de la forma: $$\boxed{f(x)\ = \sqrt {abx{^2}}}$$
* Realizamos la siguiente sustitución$$x\ = \sqrt{\frac{a}{b}}tg(t) \Longrightarrow\ dx\ =\ \sqrt{\frac{a}{b}}sec^2(t)\ dt$$
* Luego$$\begin{align*}\\ &f(x)\ = \sqrt{a-b(\sqrt{\frac{a}{b}}\ tg(t))^2}\ \Longrightarrow f(x)\ = \sqrt{a-b\ \frac{a}{b}\ tg^2(t)}\\ & f(x)\ =\ \sqrt{a-a\ tg^2(t)} \Longrightarrow f(x)\ =\ \sqrt{a(1-tg^2(t))} \Longrightarrow f(x)\ =\ \sqrt{a}\ sec(t) \end{align*}$$
* Finalmente se procede a integrar:  $$\begin{align*} &\int \sqrt{a-bx^2}\ dx\ =\ \int \sqrt{a}\ sec(t)\ \sqrt{\frac{a}{b}}\ sex^2(t)\ dt\\ & \boxed{\int \sqrt{a-bx^2}\ dx\ =\ \frac{a}{\sqrt{b}}\int sec^3(t)\ dt} \end{align*}$$
* Después de integrar, se deben realizar las sustituciones que correspondan para que las primitivas encontradas dependan de la variable x

## <mark style="background: #ADCCFFA6;">III_INTEGRALES DEFINIDAS.</mark> 

## A-Conceptos previos a la definición de Integral Definida

Sea *f* una función definida y acotada en $[a,b]$.

* Realizamos una partición P del intervalo $[a,b]$ en **n** subintervalos de longitudes iguales o distintas, seleccionando abscisas *$x_{0},~x_{1},~\dots~,~x_{n-1},~x_{n}$* de tal forma que $x_{0}$ coincida con *a* y $x_{n}$ con *b* 
* La longitud del subintervalo $[x_{i-1},~x_{i}]$ se calcula: $$\Delta~X_i~=~x_i~-~x_{i-1}~~con~~i= 1,~2,~3,~\dots,~n$$
* Se llama **norma de la partición P** al máximo del conjunto formado por todas las longitudes de los subintervalos $$||P||~=~ máx~\{\Delta~X_i\}$$
* Los valores mínimos de *f* en cada uno de los intervalos los simbolizaremos con $f(m_1),~f(m_2),~\dots,~f(m_n)$ y los valores máximos de *f* en cada uno de los intervalos los simbolizaremos como$f(M_1),~f(M_2),~\dots,~f(M_n)$

#### Aproximación por defecto:

##### Rectángulos Inscriptos en el recinto R

La altura de cada sub-intervalo es el **mínimo absoluto** alcanzado por la función *f*: $f~(m_i)$
* El área de cada rectángulo será: $f(m_i)~.~\Delta X_i$
* El área de la región sombreada será: $\sum\limits_{i~=~1}^{n}{f(m_i)~.~\Delta X_i}$ 
* La suma de las áreas de estos rectángulos se trataría de una aproximación por defecto. $\sum\limits_{i~=~1}^{n}{f(m_i)~.~\Delta X_i}~\leq~Área~de~R$

#### Aproximación por exceso:

##### Rectángulos Circunscriptos en el recinto R

La altura de cada sub-intervalo es el **máximo absoluto** alcanzado por la función *f*: $f~(M_i)$
* El área de cada rectángulo será: $f(M_i)~.~\Delta X_i$
* El área de la región sombreada será: $\sum\limits_{i~=~1}^{n}{f(M_i)~.~\Delta X_i}$ 
* La suma de las áreas de estos rectángulos se trataría de una aproximación por exceso. $\sum\limits_{i~=~1}^{n}{f(M_i)~.~\Delta X_i}~\geq~Área~de~R$

Si las subdivisiones son cada vez más finas: 
* La **sumas superiores** decrecen.
* Las **sumas inferiores** crecen.
* El valor del área buscada es el valor al que tienden las sumas superiores e inferiores cuando ||P|| tiende a cero o infinito. $$A~=~ \lim_{||P||~\to~0}{\overline{S_{p}(f)}}~~=~\lim_{||P||\to~0}{\underline{S_{p}(f)}}$$
* A este valor que tienden ambos límites se lo llama integral definida de la función *f* en el intervalo $[a,b]$.

#### Teorema.
Sea *f* una función continua y no negativa en el intervalo $[a,b]$ y considerando que todos los subintervalos tienen la misma longitud se cumple que: 

Los límites cuando *n* tiende a infinito o ||P|| tiende a 0, de las sumas inferiores o sumas superiores son idénticas.$$\lim_{||P||~\to~0}{\overline{S}}~~=~\lim_{||P||\to~0}{\underline{S}}$$ Cuando $\Delta X~=\frac{~(b-a)}{n}$ y donde$f~(m_i)$ y $f~(M_i)$, son los valores mínimos y máximos de *f* en el *i-ésimo* subintervalo.

### Definición de el área bajo la curva. 

Sea *f* continua y no negativa en el intervalo $[a,b]$. El área de la región limitada por la gráfica de *f*, el eje x y las rectas verticales $x~=~a$ y $x~=~b$ $$\lim_{||P||~\to~0}{\sum\limits_{i~=~ 1}^{n}{f(c_i)~\Delta X} }$$
Con $x~_{i-1}\le~c_{i}\le~x~_{i}$ y $\Delta x~=\frac{~(b-a)}{n}$. Además que ${\sum\limits_{i~=~ 1}^{n}{f(c_i)~\Delta X_i}}$ es la suma de RIEMANN de *f* asociada a la partición P 

### Definición de Integral Definida

Dada una función de variable real continua en el intervalo cerrado $[a,b]$ y existe el límite $$\lim_{n~\to~\infty}{\sum\limits_{i~=~ 1}^{n}{\Delta X~.~f(x)} }$$
entonces *f* es integrable $[a,b]$ y el límite se denota: $$\lim_{||P||~\to~0}{\sum\limits_{i~=~ 1}^{n}{f(c_i)~\Delta X_i}}~=~\int_{a}^{b}{f(x)~dx}$$
Este límite se llama integral definida de *f* entre $a$ y $b$. 
![[Pasted image 20230619120524.png]]

#### Propiedades de la Integral Definida. 

1. Si f es una función continua en el intervalo cerrado $[a,b]$, es integrable en dicho intervalo. 
2. Si $f(x)~\geq~0$ para todo x de $[a,b]$ entonces $\int_{a}^{b}~f(x)~dx~\geq~0$
3. Si $f(x)~\leq~0$ para todo x de $[a,b]$ entonces $\int_{a}^{b}~{f(x)~dx~\leq~0}$

##### Importante. 
* Para $f(X)~\leq~0$: En este caso el valor de la integral es negativa, debido a que la función es negativa y por estar en el cuarto cuadrante, el área a obtener va a ser negativa por lo tanto: 
	* Para todo x perteneciente a $[a,b]$ se cumple:$$f(x)~\leq~0~\to~A=|\int_{a}^{b}{f(x)~dx}|$$

4. Si *f* es una función integrable en $[a,b]$, entonces $\int_{a}^{a}{f(x)~dx~=~0}$

5. Si *f* es una función integrable en $[a,b]$, entonces $\int_{a}^{b}~{f(x)~dx~}=~-\int_{b}^{a}{f(x)~dx}$

6. Si *f* y *g* son funciones integrables sobre $[a,b]$, entonces:$$\int_{a}^{b}{[~f(x)\pm g(x)~]}~dx~=~\int_{a}^{b}{f(x)~dx}~\pm~\int_{a}^{b}{g(x)~dx}$$

7. Si *f* es una función integrable sobre $[a,b]$, entonces $\int_{a}^{b}~{k~.~f(x)~dx}~=~k~\int_{a}^{b}{f(x)~dx}$

8. Sea *f* una función integrable sobre $[a,b]$. Si b pertenece a $[a,c]$, entonces: $$\int_{a}^{c}{~f(x)~dx}=~\int_{a}^{b}{f(x)~dx}~+~\int_{b}^{c}{f(x)~dx}$$

9. Si para todo x perteneciente a $[a,b]$, se verifica que $f(x)~\geq~g(x)$ entonces: $$\int_{a}^{b}{f(x)~dx}~\geq~\int_{a}^{b}{g(x)~dx}$$
10. Si *f* es una función continua $[a,b]$, con máximo *M* y mínimo *m* absolutos, entonces: $$m~.~(b-a)~\leq~\int_{a}^{b}{f(x)~dx}~\leq~M~.~(b-a)$$
11. Si *f* es una función constante $f(x)~=~c$ entonces $\int_{a}^{b}c~dx~=~c~(b-a)$

## B-Teorema Fundamental del Valor Medio del Cálculo Integral. 


Si una función *f* es continua en $[a,b]$ entonces existe por lo menos un valor de *c* de (a,b) tal que: $$\int_{a}^{b}~{f(x)~dx}~=~(b-a)~.~f(c)$$
Si $f(x)~\geq~0$ en $[a,b]$: El área de la región comprendida por *f(x)*, el eje x y la rectas de ecuaciones x=a y x=b es igual al área de un rectángulo cuya base tiene una longitud (b-a) y altura *f(c)* siendo c un valor de (a,b).

Si $f(x)~<~0$ en $[a,b]$: El área de la región será numéricamente igual al área de un rectángulo cuya base tiene una longitud (b-a) y altura igual al valor absoluto de *f(c)*.

* *f(c)* recibe el nombre de valor medio o promedio de la función *f* en el intervalo $[a,b]$:$$f(c)~=\frac{~1}{b-a}\int_{a}^{b}{f(x)~dx}$$
##### Demostración. 

+ Si *f* es continua en $[a,b]$ por el Teorema de Weirstrass, *f* alcanza un máximo absoluto *f(M)* y un mínimo absoluto *f(m)* en dicho intervalo. 
+ Por propiedad de integrales definidas de funciones continuas se tiene: $$f(m)~(b-a)~\leq~\int_{a}^{b}{^f(x)~dx}~\leq~f(M)~(b-a)$$
+ Dividiendo miembro a miembro por $(b-a)$ $$f(m)~\leq~\frac{{1}}{(b-a)}\int_{a}^{b}{f(x)~dx}~\leq~f(M)$$
+ Llamando $$k~=~\frac{{1}}{(b-a)}\int_{a}^{b}{f(x)~dx}~$$
+ Luego resulta que: $$f(m)~\leq~\frac{{1}}{(b-a)}\int_{a}^{b}{f(x)~dx}~\leq~f(M)~\Longrightarrow~f(m)~\leq~k~\leq~f(M)$$
+ Si aplicamos el teorema del valor intermedio de una función continua en un intervalo cerrado y por el Teorema de Weirtrass, entonces existe, por lo menos un *c* de (a,b), tal que: $$f(c)~=~k$$
+ Entonces: $$f(c)~=~\frac{1}{(b-a)}\int_{a}^{b}{f(x)~dx}$$
+ Finalmente: $$\int_{a}^{b}{f(x)~dx~}~=~(b-a)~.~f(c)$$
#### Función Integral. 

Sea *f* una función integrable en $[a,b]$ y sea $x$ un elemento de $[a,b]$.Se define función integral de *f* sobre el intervalo $[a,b]$ a: $$F:[a,b]\to~R~/~F(x)~=~\int_{a}^{x}{f(t)~dx~} $$
## C-Teorema Fundamental del Cálculo Integral. 

#### Primera Parte: Existencia de la Antiderivada.
Si *f* es una función continua en $[a,b]$ y *x* es un elemento de $[a,b]$ entonces la función integral: $$F(x)=\int_{a}^{x}{f(t)~dt}$$ es derivable en ese intervalo y su derivada en cualquier punto x de $[a,b]$ es *f(x)*, es decir $$F´(x)~=~f(x)$$
##### Demostración:

<u>Hipótesis:</u> *f* es continua en $[a,b]$
<u>Tesis:</u> $F(x)~=~\int_{a}^{x}f(t)~dt~$ es derivable y para x perteneciente a $[a,b]$ es: $F'(x)~=~f(x)$
<u>Demostración:</u> Debemos probar que la función integral es derivable en $[a,b]$. Se calculará la derivada de *F*.
1. Sean *x* y *x+h* de (a,b) y por definición de derivada una función: $$F'(x)~=~\lim_{h~\to~0}\frac{F(x+h-F(x)}{h}$$
2. Por definición de función Integral: $F(x)~=~\int_{a}^{x}{f(t)~dt}~ \longrightarrow F'(x)~=~\int_{a}^{x+h}{f(t)~dt}$ $$F'(x)~=~\lim_{h~\to~0}{\frac{\int_{a}^{x+h}{f(t)~dt~-~\int_{a}^{x}{f(t)~dt}}}{h}}$$
3. Por la 5° prop. de integrales definidas.$$F'(x)~=~\lim_{h~\to~0}{\frac{\int_{a}^{x+h}{f(t)~dt~+~\int_{x}^{a}{f(t)~dt}}}{h}}$$
4. Por la 8° prop. de integrales definidas.$$\begin{align*}&F'(x)~=~\lim_{h~\to~0}{\frac{\int_{a}^{x+h}{f(t)~dt}}{h}}\\ \\ &F'(x)=~\lim_{h~\to~0}(~{\frac{1}{h}~\int_{x}^{x+h}{f(t)~dt}}~) \end{align*}$$
5. Por hipótesis sabemos que *f* es continua en $[a,b]$ y como $[x,x+h]~\subset~[a,b]$, podemos afirmar que *f* es continua en $[x, x+h]$, es decir que *f* cumple con la hipótesis del Teorema del Valor Medio del Cálculo integral: 
6. Entonces existe un c perteneciente a $(x, x+h)$ tal que:$$f(h).h~=~\int_{x}^{x+h}f(t)~dt$$
7. De donde: $$f(x)~=~\frac{1}{h}\int_{x}^{x+h}f(t)~dt$$
8. Reemplazando en la derivada de F: $$F'(x)~=~\lim_{h~\to~0}{f(c)}$$
9. Como *c* está comprendido entre *x* y *x+h*, cuando $h~\to~0 \Longrightarrow~c~\to~x$ $$F'(x)~=~\lim_{c~\to~x}{f(c)}$$
10. Por ser *f* continua por hipótesis: $$F'(x)~=~f(x)$$
11. Trabajando de manera similar se prueba que:$$\begin{align*} &Para~~x=a &&Para x=b\\ &F'(a^{+})=f(a) &&F'(b^{-})=f(b)  \end{align*}$$
12. Por lo tanto podemos afirmar que para todo x perteneciente a $[a,b]$:$$F'(x)~=~f(x)$$
##### Observaciones: 
* *f* puede ser integrable incluso no siendo continua. 
* Si *f* es continua sobre $[a,b]$ entonces la función integral *F* es derivable en ese intervalo. 

#### Segunda Parte: Regla de Barrow.

Si *f* es una función continua $[a,b]$ y G es una primitiva de *f* sobre $[a,b]$, entonces: $$\int_{a}^{b}f(t)~dt~=~G(b)~-~G(a)$$
##### Demostración: 
1. Por hipótesis G es una primitiva de *f* sobre $[a,b]$ y para un valor x cualquiera se cumple: $$G'(x)~=~f(x)~~~(1)$$
2. Al ser f continua en ese intervalo y usando la primera parte del teorema fundamental del cálculo integral, la función integral: $$F(X)~=~\int_{a}^{x}f(t)~dt~~~(2)$$ es derivable en dicho intervalo y su derivada es: $F'(x)~=~f(x)~~~(3)$
3. Luego F también es una primitiva de *f* 
4. Teniendo en cuenta (1) y (3) y utilizando la segunda consecuencia del Teorema de Lagrange, las funciones F y G difieren de una constante ya que ambas poseen la misma derivada:$F'(x)~=~G'(x)$$$F(x)~+~C~=~G(x)~~~(4)$$
5. Si x toma el valor b, la expresión (4) queda: $$F(b)~+~C~=~G(b)~~~(5)$$
6. Si x toma el valor de a, la expresión (4) queda:$$F(a)~+~C~=~G(a)~~~(6)$$
7. Restando miembro a miembro las expresiones (5) y (6): $$F(b)~-~F(a)~=~G(b)~-~G(a)~~~(7)$$
8. Teniendo cuenta (2), evaluamos la función F en $x=a$ y en $x=b$ $$\begin{align*} &F(b)~=~\int_{a}^{b}{f(t)~dt}~~~(8) &F(a)~=~\int_{a}^{a}{f(t)~dt}~=~0~~~(9) \end{align*}$$
9. Reemplazamos (8) y (9) en (7), queda: $$\int_{a}^{b}{f(t)~dt~-~0}~=~G(b)~-~G(a)$$
10. Finalmente: $$\boxed{\int_{a}^{b}{f(t)~dt}~=~G(b)~-~G(a)}$$
##### Forma de Cálculo: 
* Para la integral definida de *f* en un intervalo $[a,b]$, sólo necesitamos dos cosas:    
	1. Determinar una antiderivada (G) de *f*
	2. Calcular el número G(b) - G(a), que es igual a $\int_{a}^{b}f(t)~dt$
* La notación usual para la diferencia G(b) - G(a) es: $$\boxed{ F(x)\bigg|^{b}_{a}~~~o~~~\bigg[F(x)\bigg]^{b}_{a}}$$
