La Notación en Punto Fijo resuelve el problema de las representaciones binarias con uno, dos, tres o más dígitos (bits) fraccionarios, según la necesidad. Por ejemplo, un formato podría ser: 
<center>1101001,101</center>

La Notación en Punto Flotante divide al número en tres campos: 

- **Signo Mantisa:** Es un campo de un bit.
<center>0 positivo (+)</center>
<center>1 negativo (-) </center>

- **Exponente (E):**  Es un campo de varios bits con la **Convención Exceso $2^{2n-1}$** . El Exceso 64 usa 7 bits y el Exceso 128 requiere 8 bits. 
- **Mantisa (M):** Es un campo de varios bits fraccionarios. Para que el número este *normalizado*, el bit más significativo deber ser 1, es decir, la mantisas deber mayor a 1/2 y menor que 1. 

# Operaciones Aritméticas. 

Las operaciones aritméticas con números en punto flotante tienen ciertas particularidades. Sean $S_1E_1M_1$ y $S_2E_2M_2$ dos números en punto flotante normalizado y en Exceso 64.

1.  **Multiplicación:** El producto de los dos números será $S_pE_pM_p$ donde:

	- <u>El signo de la mantisa:</u>
			<center>	S<sub>p</sub> = 0 si S<sub>1</sub> = S<sub>2</sub> </center>
				 <center>S<sub>p</sub> = 1 si S<sub>1</sub> $\not=$ S<sub>2</sub></center>

	- <u>Exponente:</u> 
	<center>E<sub>p</sub> = E<sub>1</sub> + E<sub>2</sub> - 64- K </center> 
	- <u>Mantisa:</u>$$M_{1}\times M_{2} \times 2^{K} $$

2.  **División:** La división de los números será $S_{d}E_{d}M_{d}$ donde:

	- <u>El signo de la mantisa:</u>
			<center> S<sub>p</sub> = 0 si S<sub>1</sub> = S<sub>2</sub></center>
			 <center>S<sub>p</sub> = 1 si S<sub>1</sub> != S<sub>2</sub></center>

	- <u>Exponente:</u>
			 <center>E<sub>p</sub> = E<sub>1</sub> + E<sub>2</sub> - 64- K </center> 

	- <u>Mantisa:</u> $$M_{d}~=~(\frac{M_{1}}{M_{2}})~2^{k}$$ 

3.  **Suma y Resta:** Para sumar o restar los dos números debemos igualar sus exponente. Debido a que los exponentes están en Exceso, y en esta convención menores números se corresponden con menores combinaciones binarias absolutas.
	
	- Suponiendo que:  $$E_{1}> E_{2}$$ 
	- Entonces encontramos j, tal que:  $$j = E_{1}~-~E_2$$			
	- Modificamos $S_2E_2M_2$ a fin de igualar los exponentes: $$S_2(E_2~+~j)~\frac{M_2}{2^{j}}$$
	- Luego, procedemos a sumar o restar las mantisas según sea el caso. El resultado deber ser normalizado como en la multiplicación y en la división. 

## Base de Exponenciación. 

Para aumentar el rango de representación, la base de exponenciación en vez de 2 puede ser 4, 8 o 16. La normalización en estos casos se obtiene cuando el dígito más significativo (grupo de tres bits) de la mantisa no es cero.

## Norma IEEE 754

- En 1985 el IEE dictó la Norma 754 con el fin de estandarizar la representación en punto flotante, esta norma establece tres tipos de representaciones: 
![[Pasted image 20230820195656.png]]
- Los exponentes mínimo y máximo se utilizan para casos especiales.

Las mantisas normalizadas siempre empiezan con 1, y no es necesaria almacenarla y la coma implícita se coloca a la derecha del 1. La mantisa, ahora llamada **significado**, es mayor o igual a 1 y menor que 2. 

- Se consideran 5 clases de números: 
![[Pasted image 20230820200026.png]]

 	