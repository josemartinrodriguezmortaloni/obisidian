Las memorias son dispositivos de almacenamiento de datos e instrucciones en un computadora. Un sistema de memoria es un conjunto de estos dispositivos y los algoritmos de hardware y/o software de control de los mismo. 

Clasificación funcional de las memorias: 
+ **Memoria Interna:** Constituida por los registros internos de la CPU, se caracterizan por su alta velocidad.
+ **Memoria Central:** Almacena programas y datos. Es relativamente grande, rápida, y es accedida directamente por la CPU a través de un bus.  (memoria ram)
+ **Memoria Secundaria:** Se usa para almacenamiento de programas del sistema y grandes archivos. Su capacidad es mucho mayor que las anteriores, peor más lenta. El acceso a la misma por parte de la CPU en indirecto.(memoria rom)

Parámetros generales aplicables a todas las memorias.
1. **Unidad de almacenamiento:** Bit (1 biestable guarda un bit)
2. **Capacidad de almacenamiento:** Cantidad de bits que puede almacenarse. Capa palabra es un conjunto de bits a los cuales se accede simultáneamente. 
3. **Tiempo de acceso (ta):** Es el que tarda en leer o escribir una palabra en la memoria desde el momento que se direcciona.
4. **Tipo de acceso:**
	+ Acceso aleatorio: Cuando el tiempo de acceso es similar para cualquier posición.
	+ Acceso serie: Cuando el tiempo de acceso depende de la posición que ocupa la palabra dentro de la memoria. 
5. **Tiempo de ciclo (c):** Indica el mínimo tiempo entre dos accesos sucesivos a la memoria. El tiempo tc es mayor que el tiempo ta. El ancho de banda es la inversa de tc y es un indicativo de la cantidad de palabras procesables por unidad de tiempo.
6. **Medio físico:** 
	+ <u>Electrónicas:</u> Construidas con semiconductores.
	+ <u>Magnéticas:</u> Basadas en el fenómeno de histéresis de los materiales ferromagnéticos. 
	+ <u>Ópticas:</u> Utilizan tecnología láser. 
7. **Estabilidad:** 
	+ <u>Volatilidad:</u> El contenido de la memoria se pierde cuando se suspende la alimentación eléctrica. 
	+ <u>Almacenamiento dinámico:</u> El bit se almacena como carga de una capacidad parásita de un transitor MOS. LA información se pierde cuando el capacitor se descarga. 
	+ <u>Lectura Destructiva (DRO):</u> Al efectuar la lectura se pierde la información.
## Clasificación de las memorias electrónicas. 

Las memorias electrónicas se pueden considerar como un sistema digital mixto capaz de almacenar información binaria. 
En función del tipo de acceso, estas memorias se clasifican en: 

1. **Memorias de acceso aleatorio (RAM):** En las que el ta es similar para cualquier posición. Se subdividen según la operación que realizan: 
	+ <u>Memorias de lecturas/escritura:</u>También llamadas activas, se caracterizan por tener los ta de lectura y escritura similares, presentan volatilidad, pierden su contenido cuando dejan de esta alimentadas, pueden ser: 
		+ Memorias estáticas (SRAM)
		+ Memorias dinámicas (DRAM)
	+ <u>Memorias de sólo lectura (ROM):</u> Se caracterizan por tener el ta de escritura mucho mayor que el de lectura, presentan no volatilidad, no pierden su contenido, se subdividen: 
		+ ROM
		+ PROM (memoria rom programable)
		+ EPROM (memoria rom programable borrable)
		+ EEPROM
		+ FLASH
2. **Memorias de acceso serie:** El tiempo de acceso (ta) depende de la posición de la palabra dentro de la memoria. Se subdividen en:
	+ Registros de desplazamiento
	+ Memorias pilas(LIFO)
	+ Memorias cola (FIFO)
# Memorias de acceso aleatorio (RAM).

Memoria es un conjunto de posiciones, donde cada una de ellas está formada por una o más celdas. El tipo de celda depende de la clase de memoria que se trate y la tecnología utilizada. En la RAM de lectura/escritura, las celdas consisten en biestables asíncronos. En las RAM de sólo lectura (ROM) consisten en diodos o transistores.

Las memorias RAM operan de la siguiente manera: 
1. Una dirección se transfiere al bus de direcciones. 
2. El decodificador de direcciones procesa la dirección y selecciona una posición de memoria. 
3. La posición seleccionada se lee o escribe en función de las señales de control.
4. Si es una lectura el contenido de la posición seleccionada se transfiere al registro de datos de salida (de *n* bits). Si es una escritura se transfiere el registro de datos de entrada a la posición seleccionada. 

**Organización 2D:** Las celdas se organizan en una matriz de dos dimensiones, las filas son los números de palabras (N) y las columnas por la longitud de cada palabra. Cada celda binaria se accede por una sola línea de selección. 
La cantidad de líneas de salidas del decodificador de una org. en 2D es igual a $2^m$, donde m es la cantidad de direccionamientos. 

**Organización 3D:** Cada celda binaria se accede por dos líneas de selección. La activación simultánea de ambas determina la selección de la celda. 
La cantidad de líneas de salidas del decodificador de una org. en 3D es igual a $2~.~2^{m/2}$, donde *m* es la cantidad de direccionamientos y considerando que los dos codificadores son iguales. 

La reducción de la complejidad del decodificador y del retardo que introduce se logra a costa de agregar un decodificador y una compuerta AND por cada palabra. 

## Memorias Ram de Lectura/Escritura. 
### Memorias de lectura/escritura estática.
El elemento básico de estas memorias consiste en un biestable asíncrono. Donde la línea de selección está activa con un 1 lógico, se habilita la celda para lectura/escritura. Si L/E' = 1 se trata de una operación de lectura y las entradas al biestable se bloquean y se habilitan la compuerta AND de salida. Si L/E' = 0 se bloquea la compuerta AND de salida y se habilita la entrada al biestable. 

Estructura básica: 
![[Pasted image 20231006203250.png]]

Las líneas de acceso externas a la memoria son: 
1. **Líneas de direccionamiento A0:A3 (Bus de direcciones):** Se trata de 4 líneas unidireccionales (externas) para la selección de la palabra a acceder. Se usa una de las 16 combinaciones posibles de los 4 bits asociadas a las 4 líneas. 
2. **Líneas de datos I/O0:I/O3 (Bus de datos):** Se trata de líneas de 4 líneas bidireccionales que pueden actuar como entradas o salidas. Para lograr líneas bidireccionales se usan los buffer triestados
3. **Señal de control de lectura escritura (L/E'):** 
	+ Si L/E' = 1 se lee la memoria.
	+ Si L/E' = 0 se escribe la memoria.
4. **Señales de control:**  
	+ Si C/S’ = 1 las líneas de datos se colocan en alta impedancia.
	+ Si C/S’ = 0 y L/E’ = 0 las líneas de datos externas se conectan a las líneas de datos de entrada interiores, y las líneas de datos de salida interiores se colocan en alta impedancia.
	+ Si C/S’ = 0 y L/E’ = 1 las líneas de datos externas se conectan a las líneas de datos de salida interiores, y las líneas de datos de entrada interiores se colocan en alta impedancia.
	+ Si O/E’ = 1 se deshabilitan los circuitos de salida de la memoria sin tener en cuenta el estado de las señales C/S’ y L/E’.
#### Ciclo de lectura y ciclo de escritura. 
Para una correcta operación de la memoria se necesita una temporización adecuada de las señales aplicadas a sus líneas. 
![[Pasted image 20231006203615.png]]
Un ciclo de lectura o escritura comienza con la aplicación de una dirección en las líneas de direccionamiento. 
+ Si es una lectura R/W' debe colocarse en 1. Los datos a leer aparecerán en las líneas de salida de datos al cabo del tiempo TA. 
+ Si es una escritura R/W' debe colocarse en 1 un tiempo mínimo tAW, después del cual debe pasar a 0 para indicar una operación de escritura. Este valor debe mantenerse al menos el tiempo tWP. 
+ El tiempo tC es el tiempo de ciclo, indicativo de la cantidad de operaciones sucesivas por unidad de tiempo, tC es siempre mayor que ta. 
### Memorias de lectura/escritura dinámicas.
La celda básica cosiste en la capacidad parásita de una compuerta de un transitor MOS y los circuitos asociados de control, debido a pérdidas inevitables el capacitor se descargar, por lo tanto para restaurar periódicamente la información se utiliza un proceso llamado **refresco**, que consiste en una lectura seguida de una escritura automática cada aproximadamente 2 ms. Cuando está actuando el proceso de refresco, no es posible acceder a la memoria.

La escritura interna es generalmente 3D, con el mismo números de filas y columnas. Los m bits de dirección están divididos en $m/2$ filas y $m/2$ para las columnas. Las líneas de tiempo están multiplexadas en el tiempo. De esta manera se reduce el bus de direcciones a la mitad.

![[Pasted image 20231006204620.png]]
La aplicación de una dirección de fila provoca que todas las celdas de la fila se lean y restauren, el refresco se realiza por un controlador de RAM dinámica.

## Memorias Ram de sólo Lectura (ROM). 
Estas memorias una vez programadas sólo realizan operaciones de lectura. La organización interna de estas memorias es similar a las RAM de lectura/escritura. 

### Memorias ROM. 
Para su construcción se utilizan diodos y transistores, las conexiones se indica con un 1 y sin conexiones se indica con un 0. 

![[Pasted image 20231006205111.png]]
Los diodos se utilizan como elementos acopladores. La conexión de varios diodos a una misma línea implementa la función OR de las señales de entrada. Una ROM de $2n~x~m$ bits, podría realizar cualquier combinacional de *n* variables de entrada y *m* funciones. 
Las salidas del bus de datos son triestados para permitir la conexión de más de una memoria a un bus común. 
### Memorias PROM.
Los elementos de conexión son diodos o transistores con un fusible en serie. La programación consiste en destruir el fusible en aquellos lugares donde quiere almacenarse un 0. 

Internamente estas memorias son similares a las ROM. Para la grabación de estas memorias es necesario utilizar equipos de grabación especiales. 

### Memorias RPROM. 
Estas memorias pueden ser reescritas por el usuario. La grabación se realiza con la memoria fuera del circuito en el cual está conectada.

Tipos de RPROM:
1. **EPROM:** Las celdas están constituidas por puertas flotantes de transistores MOS. La descarga de datos se realiza con luz ultravioleta. 
2. **EEPROM:** El borrado es posición a posición, eléctricamente y en algunos casos, puede realizarse con la memoria insertada en el circuito.
3. **FLASH:** El borrado se realiza simultáneamente a todas las posiciones.![[Pasted image 20231006210104.png]]
## Extensión de longitud de palabra y capacidad.
Es posible aumentar la capacidad de una memoria partiendo de circuitos integrados de menor capacidad. Esto puede lograrse aumentado la longitud de palabra o la cantidad de las mismas. 

### Extensión de la longitud de palabra.
Se parte de un CI de N palabras de *m* bits. Las líneas de dirección y de control son compartidas por todos los CI. Las líneas de datos se amplían de *m* a *k.m* bits. 
![[Pasted image 20231006210652.png]]
### Extensión del número de palabra.
De las  $p~+~k$ líneas de dirección necesarias, *p* se interconectan a todos los CL a fin de seleccionar una de las N palabras en cada CI. El resto de las *k* líneas de dirección se inyectan a un decodificador cuyas salidas se conectan a las líneas de selección (CS) de cada CI. La señal W/R'  y el bus de datos son común para todos los circuitos.
![[Pasted image 20231006210951.png]]
Si disponemos de memorias de RAM de N palabras de m bits y pretendemos una memoria de N' palabras de *h x m* bits, se procede como sigue: 
+ Se calcula el número de Chips, donde:
	Número de Cls = Entero $N’/N$ x h
+ Se calcula el decodificador de k entradas, donde:
	2k >= Número de CIs
+ La parte baja de la dirección se conecta a las líneas de dirección de los CIs (p líneas, siendo 2p=N). La parte alta de la dirección (k bits), se conecta a las entradas del decodificador.
+ Las salidas del decodificador se conectan a las entradas de selección (CS) de cada CI.
+ Las líneas de datos se conectan a un bus común de m + h bits.
# Memorias de acceso serie.
Son aquellas en las que el tiempo de escritura o lectura de una posición depende de la situación física de la misma en el interior de la memoria. Para escribir o leer en una de estas memorias es preciso pasar primero por todas las posiciones anteriores. 

La información puede organizarse de dos formas: 
1. **Organización bit a bit:** Se disponen en serie tanto las palabras como los bits que las conforman, posee una sola línea de entrada y una solo de salida. Dispone de líneas de control de lectura/escritura. 
2. **Organización posición a posición:** Se disponen las palabras en serie pero los bits que las conforman en paralelo, donde exiten *n* entradas de información y *n* salidas de información. Estas memorias pueden clasificarse en tres tipos: 
	+ <u>Registros de desplazamientos:</u> Aquellos donde la información se desplaza una posición en un sentido, con cada orden de lectura o escritura. La orden externa de desplazamiento está constituida por los impulsos de un generador ( reloj o clock). 
			1. Registros de desplazamientos estáticos: Los impulsos de desplazamiento pueden anularse por tiempo indefinido sin que la información almacenada se pierda. Están constituidos por biestables síncronos activados por flancos.
			![[Pasted image 20231006212415.png]]
			2. Registros de desplazamiento dinámico: Los impulsos de desplazamiento no pueden anularse porque la información se perdería. La celda elemental en este caso es la capacidad parásita de la puerta de un transistor MOS. Para restaurar la información se conectan a las entradas y de esta manera la información circula permanentemente en todo el registro sincrónicamente a los impulsos del reloj, es importante agregar circuitos que permitan las operaciones de lectura y escritura.![[Pasted image 20231006212451.png]]
	+ <u>Memorias FIFO:</u> Son memorias en serie en las que la primera información que entra es la primera que sale, estas pueden implementarse con registros de desplazamiento estáticos y una unidad de control. Estas memorias poseen las siguientes características: 
		+ La lectura es destructiva.
		+ Cada operación de lectura/escritura debe producir un desplazamiento del resto de la memoria. 
		+ Cuando la memoria está llena no podrá escribirse. 
		+ Generar señales de control necesarias para que el primer dato escrito esté disponible para la primera lectura. 
		+ Deberá aceptar al menos tres entradas exteriores: señal de lectura/escritura, inicio de ciclo y de sincronismo.![[Pasted image 20231006213419.png]]
	+ <u>Memorias LIFO:</u> La última información que entra es la primera que sale. Está formada por biestables síncronos y multiplexores. La salida de la memoria es la salida del primer biestable y la entrada es el segundo canal del primer multiplexor. ![[Pasted image 20231006213343.png]]
