En 1938 Claude Shannon adopta el álgebra de Boole para la aplicación en sistemas digitales. El álgebra de Boole es bivalente, es decir, que está compuesta por dos elementos. 
Álgebra: Conjunto de elementos binarios relacionados entre sí mediante las operaciones lógicas producto $(.)$ y suma $(+)$, que cumple con los siguientes postulados:
1. **Existencia del elemento identidad:** $$\begin{align*}&a~+~0~=~a \\ &a~.~1~=~a \end{align*}$$
2. **Propiedad conmutativa:** $$\begin{align*}&a~+~b~=~b~+~a \\ &a~.~b~=~b~.~a \end{align*}$$
3. **Propiedad distributiva:** $$\begin{align*} &a~.~(b~+~c)~=~(a~.~b)~+~(a~.~c) \\ &a~+~(b~.~c)~=~(a~+~b)~.~(a~.~c) \end{align*}$$
4. **Complemento o inversión lógica:** $$\begin{align*}&a~+~\bar{a}~=~1 \\ &a~.~\bar{a}~=~0 \end{align*}$$
Algunos teoremas importantes son: 

+ <u>Dualidad:</u> Toda igualdad lógica sigue siendo válida si se intercambian los operadores y los 0s por 1s. 
+ El álgebra es un conjunto cerrado, es decir, los resultados de aplicar las operaciones lógicas pertenecen al álgebra. 
+ <u>En el álgebra se cumple que:</u> $$\begin{align*}&a~+~1~=~1\\ &a~.~0~=~0 \end{align*}$$
+ <u>Ley de idempotencia:</u> $$\begin{align*}&a~+~a~=~a \\ &a~.~a~=~a \end{align*}$$
+ <u>Ley de involución:</u> $$\begin{align*}&\bar{\bar{a}}~=~a \end{align*}$$
+ <u>Las operaciones lógicas son asociativas:</u> $$\begin{align*} &a~+~(b~+~c)~=~(a~+~b)~+~c \\ &a~.~(b~.~c)~=~(a~.~b)~.~c \end{align*}$$
+ <u>Absorción:</u>  $$\begin{align*} &a~+~(a~.~b)=a \\ &a~.~(a~+~b)~=a \end{align*}$$
+ <u>Leyes de Morgan:</u> $$\begin{align*}&\overline{a+b+c+\dots+n}~=~\bar{a}~.~\bar{b}~.~\bar{c}~.~\dots~\bar{n}\\ &\overline{a~.~b~.~c~.~\dots~.~n}~=~\bar{a}~+~\bar{b}~+~\bar{c}~+~\dots~\bar{n} \end{align*}$$
## Funciones lógicas.

Una función lógica es una variable binaria que depende de otras variables binarias relacionadas entre sí por las operaciones lógicas. 

La función adoptará el valor 0 o 1 de acuerdo a la expresión y al valor determinado de las variables. 

Se pueden escribir de dos formas: 
+ Por extensión: $f(a,b,c)= a\bar{b}~+~ac$ 
+ Por compresión: $f(a,b,c)~=~\sum_{5}{1,3}$ , esta forma nos permite disminuir costos.
+ Utilizando una tabla de verdad: 
![[Pasted image 20231003164751.png]]
### Teoremas de las funciones lógicas. 

En álgebra de Boole se cumple que: 
+ $f(a,b,c,\dots,n)=af(1,b,c,\dots,n)~+~\bar{a}f(0,b,c,\dots,n)$
+ $f(a,b,c,\dots,n) = [a~+~f(0,b,c,\dots,n)][\bar{a}~+~f(1,b,c,\dots,n)]$ 

Sus corolarios que surgen a la hora de simplificar funciones lógicas: 
1. $af(a,b,c,~\dots~,b)~=~af(1,b,c,~\dots~,n)$
2. $\bar{a}f(a,b,c,~\dots~,n)~=~\bar{a}f(0,b,c,~\dots~,n)$
3. $a~+~f(a,b,c,~\dots~,n)~=~a~+~f(0,b,c,~\dots~,n)$
4. $\bar{a}~+~f(a,b,c,~\dots~,n)~=~\bar{a}~+~f(1,b,c,~\dots~,n)$

Todas las funciones lógicas pueden expresar en forma canónica, es decir: 

+ Como una **sumatoria** de términos en los cuales aparecen todas sus variables en forma de **producto lógico**, estos términos se llaman <u>minterms</u>
+ Como una **productoria** de términos en los cuales aparecen todas sus variables en forma de **suma lógica**, estos términos se denominan <u>maxterms</u>

En ambos casos, la función se dice expresada en forma canónica y sus términos se llaman términos canónicos. 

Entonces toda función puede expresarse como: 
+ Una sumatoria de todos sus minterms afectados por un coeficiente que consiste en el valor de la función, se calcular reemplazando las variables por 0 o por 1 si, el minterm que acompaña se encuentra directa o negada respectivamente.
+ Una productoria de todos sus maxterms afectados por un coeficiente que consiste en el valor de la función, se calcular reemplazando las variables por 0 o por 1 si, el maxterm que acompaña se encuentra directa o negada respectivamente.

A cada término canónica se suele asignar un número decimal, que son pesos de acuerdos a si se encuentran expresadas en forma directa o negada. 
![[Pasted image 20231003171713.png]]
Si la variable aparece negada el peso asignado es 0. 

Para expresar una función en forma compacta o por compresión, se utiliza una regla que permite pasar de una función canónica en minterms a una en maxterms y viceversa, esta establece que:
- Se deben buscar los términos canónicos que no están en la expresión de la función y se los complemente a $2^{n}-1$, estos serán los términos de la función buscada. 

**Expresión en minsterms:** $f(a,b,c)~=~\sum_{4}{(0,1,3,5,7)}$

**Expresión en maxterms:** $f(a,b,c)~=~\prod_{4}{(1,3,6,7)}$

## Minimización de funciones lógicas. 

La mínima expresión de una función es la menor cantidad de variables y operaciones involucradas, se utilizan métodos que están basados en los postulados del álgebra. Para aplicar estos métodos es necesario que la función esté expresada en forma canónica.

Método gráfico para la identificación de los términos adyacentes de una función, resulta práctico para funciones de hasta 5 variables. Por ejemplo: 
![[Pasted image 20231003173146.png]]
Los dos números binarios en las columnas y las filas , que siguen un código de Gray de dos variables, son las variables directas o negadas de cada cuadro, y los asignados a cada término canónico, cada celda se marca con 1 para indicar los términos canónicos que son parte de la función, luego se crea un grupo entre 1s adyacentes.
Es recomendable agrupar de a varios términos para minimizar más la función.

## Compuertas lógicas. 
Las compuertas lógicas son la base constructiva de la electrónica digital. Puertas lógicas comunes: 
![[Pasted image 20231003174827.png]]
En función de la cantidad de compuertas por chip, se clasifica a los CI en escalas de integración: 
+ SSI: escala de integración prequeña, hasta 10 compuertas.
+ MSI: de 10 a 100 compuertas.
+ LSI: de 100 a 1000 compuertas.
+ VLSI: más de 1000 compuertas.
A la hora de implementar una función lógica es importante minimizarla para así usar la menor cantidad de compuertas posibles. 

# Sistemas Digitales. 

Un sistema digital es un conjunto de elementos binarios relacionados entre sí. Se distinguen dos tipos de variables: 

+ **Variables de salida:** Estas dependen de las entradas.
+ **Variables de Entrada:**

	+ <u>Variables de procesos.</u>

	+ <u>Variables de control</u>.

**Sistema Combinacional:** Cuando cada combinación de las variables de entrada (vector de entrada) se corresponde con una y sólo una combinación de las variables de salida.

**Sistema Secuencial:** Cuando a un mismo vector de entrada puede corresponder más de uno de salida. Los sistemas secuenciales deben poseer memoria interna, ya que sus salidas son consecuencia de la evolución anterior de sus entradas. 

## Sistemas Combinacionales.

Las salidas de los sistemas combinacionales son funciones lógicas de las entradas. Para diseñar un circuito combinacional es importante minimizar las funciones requeridas y finalmente implementar con compuertas lógicas. 
### Circuitos Combinacionales. 

+ **Codificadores:** Permiten codificar las líneas de entrada. Generalmente codifican en binario o BCD. Estos son de lógica invertida. 
	Si en la tabla de verdad se reemplazan con x los ceros a la izquierda de los unos de las entradas, se obtiene un codificador con prioridad. La entrada de mayor prioridad es la que define la salida. 
	Si las salidas correspondientes a las entradas activadas independientemente se deben calcular como una función OR bit a bit, estos decodificadores se denominan sin prioridad. 
	Lo codificadores cuentan con una entrada de habilitación. 

+ **Decodificadores:** Son combinaciones que posee *n* entradas y *m* salidas. Una salida se activa cuando la codificación correspondiente se inyecta a la entrada.
	Un decodificador binario , si tiene *n* entradas poseerá *m = $2^n$* salidas. Un decodificador realiza lo opuesto a un codificador. 
	Los decodificadores son útiles para implementar funciones lógicas. Cada una de sus salidas es un minterm de una función de *n* variables. Aprovechando la entrada de habilitación que suelen tener, es posible aumentar el número de vairables.
	
	Salidas activas en alto: Cuando la salida es 1.
	Salidas activas en bajo: Cuando la salida es 0. 
	
	Dependiendo de la salidas pueden ser: 
	 - Octal.
	 - Decimal.
	 - Hexadecimal.
	 - BCD - 7 segmentos.

- **Multiplexores:** Disponen de $m = 2^n$ líneas de entradas, una línea de salida y *n* líneas de selección. En función de las líneas de selección, se determina qué entrada aparece en la salida.
	Los multiplexores se pueden utilizar para implementar funciones lógicas.
- **Comparadores:** Realizan la comparación entre dos números binarios de *n* bits. Los comparadores poseen entras por =, <, y >. Esto permite realizar comparadores de elevado número de bits, partiendo de comparadores menores.
- **Detectores/Generadores de paridad:** Son CI capaces de generar/detectar la paridad de un conjunto de bits.
- **Sumadores:** Son CI que realizan sumas aritméticas de dos números de *n* bits.
- **Unidad Aritmética y Lógica (ALU)**: Permiten realizar operaciones lógicas y aritméticas sobre números binarios (generalmente de 4 bits). La operación a realizar se selecciona colocando los valores adecuados en las líneas de selección. 
	Estos bloques se pueden conectar en cascada para realizar operaciones sobre números de mayor número de bits. Las salidas P y G son las funciones Propagadora y Generadora de acarreos.
## Sistemas Secuenciales. 

Son aquellos cuyas salidas no sólo dependen de sus entradas, sino también de como han evolucionado estas anteriormente. Estos tienen que se capaces de memorizar la mencionada evolución. 
El sistema evolucionará entre distintos estados internos hasta arribar a un estado estable. Este proceso, llamado evolución automática del sistema, se repetirá hasta tanto el valor de las variables internas conocidas con el anterior, este es el **estado estable.**
El diseño de estos sistemas consiste en plantear una tabla de verdad en la cual se tenga en cuenta el concepto **tiempo**. Una función lógica no sólo depende de ciertas variables independientes, sino que también depende de sí misma. 

1. La compuertas lógicas reales se diferencian de las ideales en: 
	+ Poseen un tiempo de retardo, la señal lógica tarda un tiempo no nulo para atravesar la compuerta.
	+ Disipan calor. 
2. En un señal lógica se pueden indicar los siguiente componentes: 
![[Pasted image 20231003185655.png]]
El mismo tipo de gráfico se aplica para las señales del reloj.
### Biestables.

Los biestables son secuenciales que poseen dos estados estables, es decir, las variables internas pueden adoptar dos estados en los cuales permanecerán indefinidamente a menos que cambien las variables de entrada.
Clasificación de biestables: 
+ **Biestables Asincrónicos:** Son aquellos en los cuales las entradas actúan directamente sobre el biestable. Aproximadamente cada 20 ns cambia, no están sincronizados con alguna señal externa. 
+ **Biestables Síncronos:** Estos biestables cuentan con una entrada de sincronismo o reloj.
	+ **Biestables Síncronos activados por nivel**: Son aquellos donde su tabla de verdad es válida sólo en presencia de un nivel activo en la entrada de sincronismo, es decir, cuando la señal de reloj es 1 respeta la entrada, cuando es 0 congela el último valor almacenado.
	+ **Biestables Síncronos activados por flancos:** En estos biestables las entradas actúan sólo en presencia de un flanco en la entrada de sincronismo. Se trata de un síncrono por nivel al cual se le agrega un circuito detector de flancos
		1. <u>Flanco de Bajada:</u> Cuando pasa de 1 a 0.
		2. <u>Flanco de Subida:</u> Cuando pasa de 0 a 1.

Tipos de Biestables:
+ **Biestable JK:** Es aquel donde la salida adopta el valor opuesto al anterior. 
+ **Biestable T:** Se obtiene de un biestable JK haciendo J=K=T. También pueden obtenerse de un biestable D por flancos.
+ **Biestable D:** Pueden obtenerse a partir de un SR síncrono, haciendo S = R'= D, también de un JK haciendo J = K' = D.
#### Aplicaciones de los Biestables.

1. **Registros de desplazamiento:** Sistema secuencial síncrono que almacena varios bits de información. El formato de la información puede ser de dos tipos: serie o en paralelo. 
2. **Registros propiamente dichos:** Conjunto de biestables sincronizados por nivel o por flancos, cuyas entradas de sincronismo se encuentran unidas.

### Contadores. 

Sistema secuencial formado por biestables y lógica combinacional, capaz de almacenar en binario u otro código, la cantidad de impulsos recibidos por su entrada de cuenta.
+ **Contadores Asíncronos:** Secuencias formadas por un conjunto de biestables síncronos por flancos, las entradas de sincronismo de sus biestables no están unidas entre sí. Por lo general, la salida de un biestable sirve como entrada de sincronismo del siguiente.
+ **Contadores Síncrono:** Comparten la misma señal de reloj. Son más rápidos y complejos que los asíncronos y también tienen más compuertas que el anterior.