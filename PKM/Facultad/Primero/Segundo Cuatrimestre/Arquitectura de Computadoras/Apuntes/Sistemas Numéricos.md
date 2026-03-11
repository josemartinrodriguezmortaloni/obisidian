## **Introduccion**

> [!info] Patron Fundamental
> Esta nota es una aplicacion de [[Complejidad desde componentes simples]]: a partir de **pocos simbolos** (ej: 0 y 1 en binario), se genera **cualquier numero** usando el algoritmo de numeracion posicional.

---

Un **Sistema de Numeración** es el conjunto de reglas y convenios que permiten la representación de todos los números mediante varios signos, o varias palabras. Un sistema de numeración muy  conocido es el Romano.

El sistema **Decimal** utilizar el principio del *valor relativo* (**posicional**), es decir, una misma cifra representa valores distintos según el lugar que ocupa, la escritura numérica se codifica por el peso que corresponde a cada posición. El Sistema Decimal, que fue inventado en la India en el siglo IV antes de Cristo, está fundado en el número fijo que llamamos **diez.**

Toda combinación de operaciones fundamentales efectuadas con números cualesquiera que da origen a un nuevo número, se llama **algoritmo de numeración**.

$$
\boxed{ N = a_{n}b^{n}+ a_{n-1}b^{n-1}+a_{n-2}+ \dots +a_{1}b^1 + a_{0}b^0+a_{-1}b^{-1}+a_{-2}b^{-2}+ \dots + a_{-k}b^{-k}}
$$

Las propiedades de los números, que se estudian en Matemáticas, son válidas cualquiera sea la base utilizada, siempre que se utilice el mismo algoritmo de numeración.

## **Confiabilidad.**

Un sistema físico es confiable cuando su correcto funcionamiento sea los más independiente posible de la **temperatura** en la que trabaja, del **envejecimiento** y de la **dispersión** de los componentes que lo forma.

Los componentes que se utilizan en la construcción de los sistemas físicos de procesamiento de información, son eléctricos y electrónicos, todos ellos cambian sus parámetros con el envejecimiento y la temperatura.

Los fabricantes especifican el **porcentaje de dispersión máximo de los componentes** que comercializa.

Si comparamos la confiabilidad de dos circuitos, uno que trabaja en sistema decimal y el otro que trabaja en binario, se concluye que el más confiable es el binario. Podemos afirmar que son más confiables los circuitos que trabajan en binario que  cualquier circuito que trabaje en cualquier base.

## **Costo.**

**El costo es proporcional a la base del sistema de numeración** utilizado. Entonces, **el costo es proporcional a la cantidad de cifras**, por ello podemos decir que: 

$$
Costo~=~k~.~b~.(n+1)
$$

Entre b y (n+1) existe una relación: 

$$
b^{(n+1)}~=~M 
$$

Donde M es el módulo del sistema, es decir el máximo número representable con n+1 cifra: 

$$
\begin{align} &(n+1).Ln~b~=~Ln~M\\ &n+1~=~(\frac{Ln~M}{Ln~b}) 
\end{align}
$$

Reemplazando: 

$$
Costo~=~k~.~Ln~M~.~(\frac{b}{Ln~b})
$$

Haciendo la derivada respecto a la base ($dCosto/db=0$) e igualando a 0 obtenemos que: 

$$
b_{óptima}~=~e
$$

Concluimos que la base óptima, desde el punto de  vista del costo, está entre 2 y 3. En realidad, la expresión del costo planteada es aproximada, pero teniendo en cuenta la confiablidad, el **Sistema Binario**, es el que se utiliza en la construcción de los Sistemas Digitales.

# **Sistema de Numeración Binario.**

El Sistema Binario utiliza los símbolos 0 y 1, llamados dígitos binarios (bits o bitios). Es posible, aplicando el algoritmo de numeración, obtener los números binarios correspondientes a las primeras (16) sub 10 (el subíndice 10 indica que el número esta en el Sistema Decimal) cantidades.

La importancia de tener en cuenta este Sistema es que $2^4$ es igual a 16, y por lo tanto, cada 4 cifras binarias se corresponderá 1 cifra hexadecimal. También en el Sistema Octal se da algo parecido ya que $2^3$ es 8, y cada tres cifras binarias se corresponde una cifra octal. Esto último nos permite encontrar rápidamente las equivalencias entre binario, octal y hexadecimal.

Un **grupo de cuatro bits** recibe el nombre de **nibble** y un **grupo de ocho bits** recibe el nombre de octeto o **byte**.

Todas las operaciones aritméticas estudiadas en el álgebra, aplicadas a los números del Sistema Decimal, son aplicables a los números binarios, por cuanto ambos Sistemas responden al mismo algoritmo de numeración.

## **Conversión entre números de distintas bases.**

Para convertir las expresiones de números entre distintas bases puede usarse el siguiente procedimiento general:

### Para la parte entera del número:

$$
N_{b1}~=~a_nb_{2}^n~+~a_{n-1}b_{2}^{n-1}~+~\dots~+a_{1}b_{2}^{1}+a_{0}b_{2}^{0}
$$

Si dividimos miembro a miembro entre $b_{2}$ vemos que el último término del segundo miembro será a sub 0, que es el resto de la división y la cifra menos significativa del número base b sub 2.

Repitiendo la división entre el cociente anterior y $b_2$, obtendremos como resto a sub 1, y así sucesivamente hasta llegar a a sub n.

### Para la parte fraccionaria del número:

$$
N_{b1}~=~a_{-1}b_{2}^{-1}~+~a_{-2}b_{2}^{-2}~+~\dots~+a_{k+1}b_{2}^{k+1}+a_{-k}b_{2}^{-k}
$$

Si multiplicamos miembro a miembro por $b_{2}$ observamos que el primer término del segundo miembro es $a_{-1}$, es decir la primera cifra significativa de la parte fraccionaria. Reiterando la multiplicación sólo sobre la parte fraccionaria del resultado, vamos obteniendo las sucesivas cifras menos significativas.

## **Complementos Binarios.**

### **Complemento a la Base:**

El complemento a la base de un número N que posee m cifras enteras, se define como: 

$$
C_{b-1}(N)~=~b^{m}-N

$$

### **Complemento a la base disminuida:**

El complemento a la base disminuida de un número N que posee m cifras enteras, se entiende como: 

$$
C_{b-1}(N)~=~b^{m}-N-2^{-k}
$$

donde k es la cantidad de cifras fraccionarias.

Los complementos se aplican a Sistemas de Numeración de cualquier base, pero especialmente se aplica en el Sistema de Numeración Binario.

El complemento a 1 se puede obtener combinando ceros por unos y viceversa. Esto es consecuencia de restar m 1s un número de m bits. El complemento a 2 de un número binario puede obtener combinando unos por ceros y viceversa, y sumando uno.

# **Representación de números negativos en binario.**

Los números se pueden representar de distintas formas, a saber:

1. **Valor Absoluto y Signo:**
    
    El bit más significativo se reserva para el signo (bits de signo Bits) y los restantes para el valor absoluto (Mantisa M), si el número es negativo, son el complemento a 2.
    
    Bit de signo:
    
    **0 = +** (positivo), **1 = -** (negativo)
    
2. **Mediante el convenio de complemento a 2:**
    
    El bit más significativo es el bit de signo. Los restantes, si el número es negativo, son el complemento a 2.
    
3. **Mediante el convenio de complemento a 1:**
    
    El bit más significativo es el bit de signo, los restantes, si el número es negativo, son el complemento a 1.
    
4. Mediante el Convenio Exceso 2 elevado a la n-1:
    
    El bit más significativo es el bit signo, los restantes, si el número es negativo, son el complemento a 2.
    
    **0 = -** (negativo), **1 = +** (positivo)
