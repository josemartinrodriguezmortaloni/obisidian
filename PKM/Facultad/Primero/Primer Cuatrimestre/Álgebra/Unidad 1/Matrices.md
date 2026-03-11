#facu
Una matriz es un arreglo rectangular de números y/o elementos dispuestos en filas y en columnas, se utilizan letras mayúsculas para nombrarlos. Un ejemplo puede ser el siguiente:$$A\ = \begin{bmatrix} 1 & 0 & 0\\ 0 & 1 & 0\\ 0&0&1 \end{bmatrix}$$

- Los números en el arreglo se llaman elementos de la matriz y están ubicados en _m_ filas y _n_ columnas.
- El tamaño u orden de la matriz esta dado por la cantidad de filas y de columnas que posee, se escribe de la forma _mxn_.

#### Notación:

- _a<sub>ij</sub>_ indica el elemento de la fila _i_ y la columna _j_ de la matriz A. Por lo tanto una matriz también se puede expresar en base a sus componentes de la siguiente manera $$A\ =\ \begin{bmatrix}\ a_{ij}\ \end{bmatrix}\ o \ A\ =\ \begin{bmatrix}\ a_{ij}\ \end{bmatrix}_{mxn}$$

## Tipos de matrices.

#### Matriz Fila:

Es una matriz que tiene exactamente una fila y n elementos, es de orden _1xn_ y se las denomina vector fila. Por ejemplo: $$A\ = \begin{bmatrix}\ 1 & 5 & 8 & 9\ \end{bmatrix}\ $$

#### Matriz Columna:

Es una matriz que tiene exactamente una columna y n elementos, es de orden _mx1_ y se las denomina vector columna. Como por ejemplo: $$B\ =\ \begin{bmatrix}\ x+2y\\ 5\\ 10 \ \end{bmatrix}$$

#### Matriz Nula:

Es una matriz cuadrada, es decir de orden _mxm_, cuyos elementos son el 0. Por ejemplo: $$\ Z_{2x2}\ =\ \begin{bmatrix}\ 0 & 0\\ \ 0 & \ 0  \ \end{bmatrix} \ o\ J_{2x1}\ = \begin{bmatrix} 0\\ \ 0\ \end{bmatrix}$$

#### Matriz Cuadrada:

La matriz cuadrada tiene la misma cantidad de filas que de columnas, y sus elementos $[\ a_{ii}\ ]$ se encuentran en la diagonal principal. $$\ A\ =\ \begin{bmatrix} a_{11} &a_{12} &\cdots &a_{1n}\\ a_{21} &a_{22} &\cdots &a_{2n}\\ a_{31} &a_{32} &\cdots &a_{3n}\\ \vdots &\vdots &\ddots &\vdots\\ a_{n1} &a_{n2} &\cdots &a_{nn}\end{bmatrix}$$

#### Matriz Triangular Superior:

Matriz cuadrada donde todos los elementos por debajo de la diagonal principal son 0. En símbolos A<sub>nxn</sub> es triangular superior si cumple que para todo $1\leq i\leq n$ y para todo $1\leq j \leq n$: $$a_{ij}\ =\ 0\ \ si\ \ i>j $$
Por ejemplo: $$L\ =\ \begin{bmatrix} 2 & 5 & 8\\ 0 & 4 & 9\\ 0 & 0 & 0  \end{bmatrix}$$

#### Matriz Triangular Inferior:

Es una matriz cuadrada cuyos elementos por encima de la diagonal principal son iguales a 0. En símbolos A <sub>nxn</sub> es triangular inferior si cumple que para todo $1\leq\ i\ \leq\ n$ y para todo $1\leq\ j\ \leq\ n$:$$a_{ij}\ =\ 0\ \ si\ \ i<j$$
En el caso que una matriz sea triangular inferior y superior la matriz se denomina triangular.

Por ejemplo: $$\begin{align*} &A\ =\ \begin{bmatrix} 5 &0 &0\\ 6 &7 &0\\ 2 &23 &19 \end{bmatrix} & J\ = \begin{bmatrix} 6 &0 &0\\ 0 &8 &0\\ 0 &0 &1 \end{bmatrix}\end{align*}$$

#### Matriz Diagonal:

Matriz cuyos elementos se encuentran en la diagonal principal son distintos a 0 y los elementos que están por afuera de la diagonal principal son iguales a 0. En símbolos A<sub>nxn</sub> es diagonal si cumple que para todo $1\leq\ i \leq\ n$ y para todo $1 \leq\ j \leq\ n$:$$a_{ij}\ =0\ \ si\ \ i\ \neq\ j$$
Por ejemplo: $$A\ =\ \begin{bmatrix} 3 &0 &0\\ 0 &5 &0\\ 0 &0 &8  \end{bmatrix}$$

#### Matriz Escalar:

Es aquella matriz en donde todos los elementos de la diagonal principal son cualquier valor _k_, en símbolos: X<sub>nxn</sub> es una matriz escalar si para todo $1\leq\ i\ \leq\ n$ y para todo $1\leq\ j\ \leq\ n$ se cumple que: $$X_{ij}=\left\lbrace\begin{array}{c} 0~si~i\neq j~ \\ k~si~i~=j  \end{array}\right. \ ~siendo~k~un~escalar$$
Por ejemplo: $$A\ =\ \begin{bmatrix} 3 &0 &0\\ 0 &8 &0\\ 0 &0 &9 \end{bmatrix}\ ,\ D\ = \begin{bmatrix} 0 &0\\ 0 &0\end{bmatrix}$$

#### Matriz Identidad:

Matriz cuadrada de orden _nxn_ donde todos los elementos de la diagonal principal son 1 y el resto de los elementos son 0, se simboliza _I<sub>n</sub>_ o _I_. En símbolos: _I<sub>nxn</sub>_ es la matriz identidad si para todo $1\leq\ i\ \leq\ n$ y para todo $1\ \leq\ j\ \leq\ n$ se cumple que: $$X_{ij}=\left\lbrace\begin{array}{c} 1~si~i = j~ \\ 0~si~i\neq j  \end{array}\right. \ $$
Por ejemplo: $$I\ =\ \begin{bmatrix}1 &0 &0\\ 0 &1 &0\\ 0 &0 &1 \end{bmatrix}$$

## Operaciones Matriciales.

#### Suma Matricial:

La suma de dos matrices consiste en la suma de cada elemento de las matrices dadas dando como resultado una matriz del mismo tamaño. $$A\ +\ B\ =\ [a_{ij}]\ + \ [b_{ij}]\ = \ [a_{ij}\ +\ b_{ij}]$$
La diferencia de dos matrices se define como, si A y B tienen el mismo tamaño entonces: $$A-B\ =\ A\ +\ (-B)$$

#### Producto de un escalar por una matriz.

Resulta de multiplicar un escalar k por una matriz en símbolos es: dado A<sub>mxn</sub> = [a<sub>ij</sub>] y un escalar $k \in \mathbb{R}$, la matriz kA será: $$k\ .\ A\ =\ k\ .\ [a_{ij}]\ =\ [ka_{ij}]$$

##### Propiedades de la suma y la multiplicación por un escalar de matrices.

Si A, B y C son matrices del mismo tamaño, y c y d son escalares cualesquiera, entonces se cumple que:

1. A+B = B+A
2. (A+B)+B = A+(B+C)
3. A+N = A
4. A+(-A) = N
5. c.(A+B) = cA + cB
6. (c+d).A = ca + da
7. (c.d).A = c.(d.A)
8. I.A = A

$$
A = \begin{bmatrix} a*{11}&\cdots &a*{1n}\\ \vdots &\ddots &\vdots\\ a*{n1} &\cdots &a*{nn}\end{bmatrix}
$$
