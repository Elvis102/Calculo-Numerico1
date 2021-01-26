---
title: "Tema 2 - Errores"
author: "Juan Gabriel Gomila y Arnau Mir"
date: ''
output: 
  ioslides_presentation: 
    css: Mery_style.css
    fig_caption: yes
    keep_md: yes
    logo: Images/matrix.gif
    widescreen: yes
header-includes: \usepackage{amsmath,color,array,booktabs,algorithm2e,multirow}
---



# Análisis del Error

## Introducción

El concepto de **error** es fundamental en el cálculo numérico. 

En cada problema, es muy importante realizar un seguimiento de los **errores
cometidos**  a fin de poder estimar el **grado de aproximación** de la **solución obtenida**.

Hay que estudiar los diversos **tipos de error** que afectan sus soluciones:

* errores que provienen del **almacenamiento de los datos** llamados **errores de redondeo** y
* errores que provienen de los **métodos o algoritmos numéricos** usados.

Empezemos con los errores de redondeo.

## Errores de redondeo

Los *ordenadores* trabajan con un número **finito de dígitos**. 

Almacenar números que necesitan un número infinito de dígitos no es posible.

Los números tales como $\frac{1}{3}=0.333\ldots$ o $\sqrt{2}=1.4142136\ldots$ no pueden ser almacenados ya que
se necesitaría un número infinito de cifras para ello.

El error que cometemos al almacenar un número en el ordenador se denomina **error de redondeo**. 

<div class="example">
**Ejemplo**

Supongamos que nuestro ordenador trabaja con $6$ cifras decimales. Cuando trabajamos con $\sqrt{2}$, tendremos un error de redondeo acotado por $\frac{1}{2}\cdot 10^{-6}$:
$$
|\sqrt{2}-1.414214|\approx4.4\times 10^{-7}\leq 0.5\cdot 10^{-6}.
$$
</div>

## Introducción
Antes de proseguir con el análisis del error, hagamos una breve descripción de la manera en que los ordenadores almacenan los valores reales de cara a entender con más profundidad los **errores de redondeo** y los errores que provienen de las **operaciones realizadas**.

# Números binarios

## Introducción

La manera de almacenar los números o las aproximaciones de los mismos como hemos comentado es mediante su representación binaria ya que los ordenadores trabajan en **binario**.

Dado un número $x$, la representación binaria de $x$ será:
$$
x=\sum_{i=0}^s a_i\cdot 2^i +\sum_{i=1}^{t} b_i\cdot 2^{-i}, 
$$
donde $a_i,b_i\in\{0,1\}$.


## Ejemplo
<div class="example">

Veamos cómo escribir el número $10.2345$ en binario. 

Primero escribimos su parte entera: $10=2+2^3,$ y escribimos $10=1010_{2)}$.  

El número $1010$ nos indica que $10$ puede escribirse como $10=1\cdot 2^3+0\cdot 2^2+1\cdot 2^1+0\cdot 2^0$. Para obtener los números $1010$, hemos de realizar divisiones sucesivas por $2$ y considerar los **restos**:
$$
\begin{align*}
\color{green}{10}= & \color{blue}{5}\cdot 2+\color{red}{0},\\
\color{green}{5}= & \color{blue}{2}\cdot  2+\color{red}{1},\\
\color{green}{2}= & \color{blue}{2}\cdot \color{red}{1}+\color{red}{0}.
\end{align*}
$$

</div>

## Ejemplo (continuación)
<div class="example">


Vayamos a la parte decimal. Para hallar su representación binaria hemos de multiplicar sucesivamente por 2 y considerar la parte entera:
$$
\begin{align*}
0.2345\cdot 2= & \color{red}{0}.469,\ \Rightarrow b_1=\color{red}{0}, \\
2\cdot(0.469-b_1) = &\color{red}{0}.938, \ \Rightarrow b_2=\color{red}0,\\
2\cdot(0.938-b_2) = & \color{red}{1}.876.\ \Rightarrow b_3=\color{red}1,\\
2\cdot(1.876-b_3) = & \color{red}{1}.752, \Rightarrow b_4=\color{red}1,\\
\ldots & \ldots
\end{align*}
$$
Esto significa que $0.2345 =0\cdot 2^{-1}+0\cdot 2^{-2}+1\cdot 2^{-3}+1\cdot 2^{-4}+\cdots$ y escribimos $0.2345=0.0011\ldots_{2)}$, donde $\ldots$ significa que hay más cifras y que dicho número puede tener infinitas cifras en base $2$.

Por tanto, el número $10.2345$ en binario será de la forma $10.2345=1010.0011\ldots_{2)}$.
</div>

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/18KBWA-iT6v6UdTK4Sh_TWPpU2xZxZGpS?usp=sharing)
</div>


# Representación binaria

## Introducción

Vamos a ver cómo se almacena un número real en el ordenador en **formato binario** usando **64 bits**. 

Sea $x$ un número real que suponemos en formato **binario**.

Escribimos $x$ de la forma siguiente:
$$
x=(-1)^s\ 1.f\cdot 2^{c-1023},
$$

## Signo, mantisa y exponente
donde 

* $s$ indica el **signo** del número, indicado $s=0$ para los números **positivos** y $s=1$, para los **negativos.**
* $1.f$ es lo que se llama la **mantisa** donde $f$ es una secuencia de ceros y unos,
$$
f=f_1f_2f_3\ldots f_n,\ f_i\in\{0,1\}.
$$
* $c\geq 1$ indica el **exponente** del número donde se le resta $1023$ para poder representar números muy pequeños en valor absoluto. Escribimos $c-1023$ en binario como $c-1023=e_1e_2\ldots e_m$, con $e_i\in\{0,1\}$

## Signo, mantisa y exponente

Representamos $x$ por tres cajas: el signo $s$, la mantisa $f$ y el exponente $c-1023$ en binario:
$$
|s|\ f_1f_2\ldots f_n|\ e_1e_2\ldots e_m|
$$

<div class="example">
**Ejemplo**

Sea $x=31.54068088531494$. Vamos a escribirlo en binario en el formato anterior.
$$
\begin{align*}
31.54068088531494_{10)}= & 11111.10001010011010100001_{2)}\\
=& +1.11110001010011010100001_{2)}\cdot 2^{4}
\end{align*}
$$
En este caso $s=0$, $f=11110001010011010100001$ y $c=100_{2)}+1023 =100_{2)}+1111111111_{2)}=10000000011_{2)}$.

Entonces escribiremos:
$$
31.41593_{10)}=|0|\ 111110001010011010100001|\ 10000000011|.
$$

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/18KBWA-iT6v6UdTK4Sh_TWPpU2xZxZGpS#scrollTo=cb7QCbX7MshF)
</div>
</div>



## Signo, mantisa y exponente

Como sólo tenemos $64$ **bits** para representar el número, la cantidad de bits usados para su representación no puede superar $64$. En caso que los superase, tendremos que considerar una **aproximación** del mismo. 

En el ejemplo anterior hemos usado el número siguiente de bits:

* signo: 1 bit,
* mantisa: 20 bits,
* exponente: 11 bits,

en total, 32 bits, por tanto, sí sería posible su representación exacta y no haría falta considerar una aproximación del mismo.

## Rango de valores
En general, para representar un número en $64$ bits se guardan

* 1 bit para su signo,
* 52 bits para su mantisa y
* 11 bits para su exponente.

Como sólo tenemos 52 bits para su mantisa, esto significa que sólo podemos representar números en base 10 con 16 cifras decimales como máximo ya que $2^{52} =4.5035996\times 10^{15}$.


El exponente estaría entre los valores $-1022$ ($c=1$, por tanto, $c-1023=-1022$) y $1024$ ($c=2^{11}-1=2047$, por tanto $c-1023=1024$).

## Ejemplo
<div class="example">

Hagamos la conversión contraria.

Imaginemos que nos dan el número siguiente:
$$
|1|\ 101101110011|\ 1111|.
$$
Vamos a ver a qué número $x$ corresponde.

El signo es negativo, por tanto $x<0$. 

La mantisa será:
$$
1.f=1+\frac{1}{2}+\frac{1}{2^3}+\frac{1}{2^4}+\frac{1}{2^6}+\frac{1}{2^7}+\frac{1}{2^8}+\frac{1}{2^{11}}+\frac{1}{2^{12}}=1.7155762.
$$
El exponente $c$ vale $1111_{2)}-1023=1+2+2^2+2^3-1023=-1008$. 

El número será:
$$
x=-1.7155762\cdot 2^{-1008}\approx -6.25424\cdot 10^{-304}.
$$


</div>

## Número más pequeño
Con la representación anterior, el número más pequeño $x_{\mathrm{min}}$ en valor absoluto que se podría representar sería:
$$
|0|0000000000000000000000000000000000000000000000000000|1|
$$
cuyo valor es:
$$
x_{\mathrm{min}}=1\cdot 2^{1-1023}=2^{-1022}\approx 2.22507\cdot 10^{-308},
$$

## Número más grande
y el número más grande $x_{\mathrm{máx}}$ en valor absoluto que se podría representar sería:
$$
\begin{align*}
 & |0|1111111111111111111111111111111111111111111111111111\\ & |11111111111|
\end{align*}
$$
cuyo valor es:
$$
\begin{align*}
x_{\mathrm{máx}}  = & \left(1+\sum_{i=1}^{52}\frac{1}{2^i}\right)\cdot 2^{2047-1023}\\  = & 1.9999999999999997779553950749686919152736663818359375\\ & \cdot 2^{1024}\approx 3.59539\cdot 10^{308}
\end{align*}
$$

# Números en formato decimal

## Introducción

El uso del **formato binario** cuando realizamos operaciones es incómodo y muy tedioso ya que se necesitan muchas cifras para representar valores numéricos.

Por dicho motivo, usualmente el formato usado es el decimal o la **base 10**.

La representación de un número $x$ en **formato decimal** o en **base 10** en el formato llamado **punto flotante** es la siguiente:
$$
x=\pm 0.d_1d_2\ldots d_k\cdot 10^n,\ d_i\in\{0,\ldots,9\},\ d_1\neq 0.
$$

El valor $k$ es el número de **cifras significativas** del número $x$ y  $n$ es el exponente del número.

Vimos anteriormente que si usamos la representación binaria de $64$ bits, el valor máximo de cifras significativas es $k_{\mathrm{máx}}=16$.

## Ejemplos
<div class="example">

La representación **punto-flotante** de los números siguientes es la siguente:

* $x=31.41593,$ $fl(x)=0.3141593\cdot 10^2$.
* $x=-0.00004356,$ $fl(x)=-0.4356\cdot 10^{-4}$.
* $x=19875.24,$ $fl(x)=0.1987524\cdot 10^{5}$.



</div>

## Representación punto-flotante
Si el número de **cifras significativas** del número $x$ supera el valor $k$ (número máximo de **cifras significativas** con las que trabajamos), la representación **punto-flotante** de $x$, $fl(x)$ será una **aproximación** de $x$. 

Para hallar dicha aproximación, podemos hacer dos cosas:

* **cortar**. En este caso si $x=\pm 0.d_1\ldots d_kd_{k+1}d_{k+2}\ldots\cdot 10^n,$ $fl(x)=\pm 0.d_1\ldots d_k\cdot 10^n$.
* **redondear**. En este caso si $x=\pm 0.d_1\ldots d_kd_{k+1}d_{k+2}\ldots\cdot 10^n,$
$$
fl(x)=\begin{cases}
\pm 0.d_1\ldots (d_k+1)\cdot 10^n, & \mbox{si }d_{k+1}\geq 5,\\
\pm 0.d_1\ldots d_k\cdot 10^n, & \mbox{si }d_{k+1}< 5.
\end{cases}
$$

## Corte y redondeo
En el caso de **redondear**, si $d_{k+1}\geq 5$ y $d_k+1=10$, la representación **punto-flotante** de $x$ será $fl(x)=\pm 0.d_1\ldots (d_{k-1}+1)0$. Si  $d_{k-1}+1=10$, la representación **punto-flotante** de $x$ será $fl(x)=\pm 0.d_1\ldots (d_{k-2}+1)00$ y así sucesivamente.

<div class="example">
**Ejemplo**

Supongamos que trabajamos con $k=4$ **cifras significativas**. La representación punto flotante de los números siguientes vale:

* $x=1234.5678$, $fl(x)=0.1234\cdot 10^4$ si cortamos y $fl(x)=0.1235\cdot 10^4$ si redondeamos.
* $x=-0.00004599881234$, $fl(x)=-0.4599\cdot 10^{-4}$ si cortamos y $fl(x)=-0.4600\cdot 10^{-4}$ si redondeamos.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/18KBWA-iT6v6UdTK4Sh_TWPpU2xZxZGpS#scrollTo=DzwOrFTNNChU)
</div>

</div>

# Errores absoluto y relativo

## Introducción

<l class="definition">Definición de error absoluto y relativo. </l>
Sea $x$ un valor real y $\hat{x}$ una aproximación del mismo. Definiremos **error absoluto** de $x$ a la cantidad $e_a(x)=|x-\hat{x}|$ y **error relativo** a la cantidad $e_r(x)=\frac{|x-\hat{x}|}{|x|}$.

<div class="example">
**Ejemplo**

Calculemos los errores absolutos y relativos para los valores numéricos del ejemplo anterior cuando cortamos o redondeamos:

* $x=1234.5678$, 
    * corte: $fl_c(x)=0.1234\cdot 10^4$, $e_a(x)=|x-fl_c(x)|=|1234.5678-1234|=0.5678,$ $e_r(x)=\frac{e_a(x)}{x}=\frac{0.5678}{1234.5678}\approx 5\times 10^{-4}.$
    * redondeo: $fl_r(x)=0.1235\cdot 10^4$, $e_a(x)=|x-fl_r(x)|=|1234.5678-1235|=0.4322,$ $e_r(x)=\frac{e_a(x)}{x}=\frac{0.4322}{1234.5678}\approx 4\times 10^{-4}.$
</div>

## Ejemplo (continuación)
<div class="example">


* $x=-0.00004599881234$, 
    * corte: $fl_c(x)=-0.4599\cdot 10^{-4}$, $e_a(x)=|x-fl_c(x)|=|-0.00004599881234+0.00004599|=8.81234\times 10^{-9},$ $e_r(x)=\frac{e_a(x)}{x}=\frac{8.81234\times 10^{-9}}{0.00004599881234}\approx 2\times 10^{-4}.$
    * redondeo: $fl_r(x)=fl(x)=-0.4600\cdot 10^{-4}$, $e_a(x)=|x-fl_r(x)|=|-0.00004599881234+0.000046|=1.18766\times 10^{-9},$ $e_r(x)=\frac{e_a(x)}{x}=\frac{1.18766\times 10^{-9}}{0.00004599881234}\approx 2.5819\times 10^{-5}.$
    
<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/18KBWA-iT6v6UdTK4Sh_TWPpU2xZxZGpS#scrollTo=sd9wjAZlN0w4)
</div>

</div>

## Errores absoluto y relativo
Observaciones:

* Mirando los ejemplos anteriores, vemos que los **errores absolutos** dependen de las magnitudes de los valores $x$: en el primer ejemplo los errores absolutos son de orden de $10^{-1}$; en cambio, en el segundo, son del orden de $10^{-9}$.

* En cambio, los **errores relativos** no se ven afectados por dichas magnitudes. Por dicho motivo, si queremos estudiar los errores sin tener en cuenta el orden de los valores $x$, hay que usar los **errores relativos**. Vemos que en los dos ejemplos los errores relativos son del orden de $10^{-4}$ ya que recordemos que trabajamos con 4 **cifras decimales significativas**. 

## Errores absoluto y relativo

* Los **errores absolutos y relativos** cometidos al **redondear** son menores que los **errores absolutos y relativos** cometivos al **cortar**. Por tanto, siempre es mejor **redondear** que **cortar**.

* Nunca o casi nunca podremos calcular los errores **absoluto y relativo** ya que en la mayoría de los casos no conoceremos el valor exacto $x$. Por dicho motivo siempre se trabaja con cotas de los errores **absoluto** y **relativo**. Es decir, sabremos qué error cometemos en el peor de los casos.
Por dicho motivo, siempre trabajaremos con cotas de **errores absolutos** y **errores relativos**.

# Aritmética de dígitos finitos

## Cifras significativas

Vamos a formalizar la definición de aproximación de $\color{red}{k}$ cifras significativas:

<l class="definition">Definición.</l>
Diremos que la aproximación $\hat{x}$ del valor $x$ tiene $\color{red}{k}$ **cifras significativas** si el **error relativo** de la aproximación está acotado por:
$$
e_r(x)=\frac{|x-\hat{x}|}{|x|}\leq 5\cdot 10^{-\color{red}{k}}.
$$

Observamos que en los dos ejemplos anteriores, las aproximaciones por **corte y redondeo** tenían $\color{red}{4}$ **cifras significativas** ya que en todos los casos los **errores relativos** estaban acotados por $5\cdot 10^{-\color{red}{4}}.$

## Operaciones básicas

Aparte de los **errores de redondeo** que tenemos en la representación de los números, las operaciones básicas como la **suma, resta, multiplicación o división** realizadas en los ordenadores no son exactas ya que se cometen errores.

Las operaciones anteriores se realizan en **formato binario** en los ordenadores y son básicamente operaciones lógicas o de desplazamiento de bits.

A dicho tipo de **aritmética** se le denomina **aritmética de dígitos finitos**.


## Operaciones básicas

Sean $\oplus$, $\ominus$, $\odot$ y $\oslash$ las operaciones de la suma, resta, multiplicación y división, respectivamente, que realiza el ordenador.

Dados dos valores $x$ e $y$, cuando nos planteamos el resultado de una operación entre ellos, $x + y$, $x-y$, $x\cdot y$ o $x/y$ en realidad obtenemos lo siguiente: $fl(x)\oplus fl(y)$, $fl(x)\ominus fl(y)$, $fl(x)\odot fl(y)$ o $fl(x)\oslash fl(y)$ y los resultados se indican de la forma siguiente para poner de manifiesto los errores cometidos en las operaciones:
$$
\begin{align*}
fl(x)\oplus fl(y) & =fl(fl(x)+fl(y)),\\
fl(x)\ominus fl(y) & =fl(fl(x)-fl(y)),\\
fl(x)\odot fl(y) & =fl(fl(x)\cdot fl(y)),\\
fl(x)\oslash fl(y) & =fl(fl(x)/fl(y)).
\end{align*}
$$

## Ejemplo
<div class="example">

Supongamos que trabajamos con $\color{red}{4}$ **cifras significativas**. 

Nos planteamos la operación siguiente: $fl(x)\oplus fl(y)$ con $x=\frac{1}{3}$ e $y=\frac{345}{7}$.

Como trabajamos con $4$ cifras significativas, el valor de $fl(x)$ será: $fl(x)=0.3333$.

Como trabajamos con $4$ cifras significativas, el valor de $fl(y)$ será: $fl(y)=49.29$.

En las aproximaciones anteriores, hemos redondeado ya que vimos que **redondear** es mejor que **cortar**.

El valor de $fl(x)\oplus fl(y)$ será:


$$
fl(x)\oplus fl(y) = fl(fl(x)+fl(y))=fl(0.3333+49.29)=fl(49.6233)=49.62.
$$
Fijémonos que tenemos los errores siguientes:

* las aproximaciones de $x$ e $y$ a $\color{red}{4}$ cifras significativas.
* el error que hemos cometido en la operación **suma**.

</div>

## Ejemplo (continuación)
<div class="example">

Seguidamente, nos planteamos la operación siguiente: $fl(x)\odot fl(y)$.

El valor de $fl(x)\odot fl(y)$ será:
$$
fl(x)\odot fl(y) = fl(fl(x)\cdot fl(y))=fl(0.3333\cdot 49.29)=fl(16.428357)=16.43.
$$
En este caso, los errores cometidos son:

* las aproximaciones de $x$ e $y$ a $\color{red}{4}$ cifras significativas.
* el error que hemos cometido en la operación **producto**.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/18KBWA-iT6v6UdTK4Sh_TWPpU2xZxZGpS#scrollTo=jAdjQbeKOK8A)
</div>

</div>

# Fórmula de propagación del error

## Introducción

En la sección anterior hemos visto cómo se propaga el error cuando realizamos **operaciones aritméticas básicas** como **sumar, restar, multiplicar o dividir**.

Nos podemos plantear el caso general, cómo se propaga el error cuando aplicamos una función $f$ a una cantidad $x$ con error, es decir, dada una función cualquiera $f$ y una cantidad $x$, ¿podemos tener una idea de cuánto nos equivocamos cuando hacemos $fl(f(fl(x)))$? O sea, ¿cuánto vale:
$|fl(f(fl(x)))-f(x)|$? 

La **fórmula de propagación del error** intenta responder a la cuestión anterior.


## Fórmula de propagación del error
<l class="prop">Fórmula de propagación del error.</l>
Sea $f$ una función de clase ${\cal C}^1$ y $x$ un valor con error absoluto $e_a(x)$. En este caso,  tenemos:
\[
|f(x+e_a(x))-f(x)|\approx |f'(x)|\cdot e_a(x), 
\]
es decir, el error en la función $f(x)$ es aproximadamente el valor de la **derivada** en $x$ multiplicado por $e_a(x)$
De hecho,
\[
|f(x+e_a(x))-f(x)|\leq \max_{t\in (x-e_a(x),x+e_a(x))} |f'(t)|\cdot e_a(x).
\]

Por tanto, el comportamiento de la **función derivada** es clave para ver cómo se propaga el error cuando aplicamos una función $f$.

## Fórmula de propagación del error
<div class="dem">
**Demostración**

Si aplicamos el **Teorema del valor medio** a la función $f$ en el intervalo $(x-e_a(x),x+e_a(x))$, tenemos que existe un punto $c\in (x-e_a(x),x+e_a(x))$ tal que:
$$
f(x+e_a(x))-f(x) = f'(c)\cdot e_a(x).
$$
Si aproximamos $f'(c)$ por $f'(c)\approx f'(x)$ y considerando valores absolutos a la expresión anterior tenemos la fórmula de propagación del error.
</div>

## Ejemplo

<div class="example">


Queremos computar $(\sqrt{2}-1)^6$ utilizando $x=1.4$ valor tan aproximado de $\sqrt{2}$.

¿Cuál de las expresiones siguientes es la más apropiada desde el punto de vista numérico:
\[
\frac{1}{(\sqrt{2}+1)^6},\quad (3-2\sqrt{2})^3,\quad \frac{1}{(3+2\sqrt{2})^3}?
\]

Sean $f_1$, $f_2$ y ser $f_3$ las funciones siguientes:
\[
f_1(x)=\frac{1}{(x+1)^6},\ f_2(x)= (3-2x)^3,\ f_3(x)= \frac{1}{(3+2x)^3}.
\]
La derivada de las funciones anteriores en el punto $1.4$ valen:
$$
\begin{align*}
f_1'(x)= & -\frac{6}{(x+1)^7},\Rightarrow |f_1'(1.4)|=0.0131,\\
f_2'(x)= & -6 (3-2 x)^2,\Rightarrow |f_2'(1.4)|=0.24,\\
\color{red}{f_3'}(x)= & -\frac{6}{(2 x+3)^4},\Rightarrow |f_3'(1.4)|=0.0053.
\end{align*}
$$
</div>

## Ejemplo (continuación)

<div class="example">

Como $e_r(x)\leq 5\cdot 10^{-2}$ ya que tiene $2$ cifras significativas, tenemos que $e_a(x)\approx |x|\cdot e_r(x)\leq 0.07$.

Los errores cometidos aplicando las funciones anteriores serán aproximadamente:
$$
\begin{align*}
|f_1(\sqrt{2})-f_1(1.4)|\approx & |f_1'(1.4)|\cdot e_a(x)\leq 0.0131 \cdot 0.07 = 9.157\times 10^{-4},\\
|f_2(\sqrt{2})-f_2(1.4)|\approx & |f_2'(1.4)|\cdot e_a(x)\leq 0.24 \cdot 0.07 = 0.0168,\\
\color{red}{|f_3(\sqrt{2})-f_3(1.4)|}\approx & |f_3'(1.4)|\cdot e_a(x)\leq 0.0053 \cdot 0.07 = 3.711\times 10^{-4}.
\end{align*}
$$
Observamos que la mejor función es la función $f_3$.
</div>

## Caso de $n$ variables
Vamos a generalizar la **fórmula de propagación del error** para el caso en que tengamos $n$ valores $x_1,\ldots, x_n$:

<l class="prop">Fórmula de propagación del error.</l>
Sea $f$ una función de $n$ variables de clase ${\cal C}^1$ y $x_1,\ldots, x_n$ $n$ valores con errores absolutos $e_a(x_1),\ldots, e_a(x_n)$, respectivamente. En este caso,  tenemos:
\[
\begin{align*}
& |f(x_1+e_a(x_1),\ldots,x_n+e_a(x_n))-f(x_1,\ldots, x_n)|\approx \\ & \sum_{i=1}^n \left|\frac{\partial 
f}{\partial x_i}(x_1,\ldots, x_n)\right|\cdot e_a(x_i).
\end{align*}
\]
es decir, el error en la función $f(x)$ es aproximadamente la suma de las **derivadas parciales** respecto cada variable $x_i$ multiplicado por $e_a(x_i)$, para $i=1,\ldots, n$.


## Ejemplo: computación del área de un círculo
<div class="example">

Queremos calcular el área de un círculo donde $e_a(\pi)$ y $e_a(r)$ son los errores absolutos de los valores $\pi$ y el radio $r$.

El área del círculo vale:
\[
A(\pi,r)=\pi\cdot r^2.
\]

Utilizando la fórmula de propagación del error para dos variables,  tenemos:
$$
|A(\pi +e_a(\pi),r+e_a(r))-A(\pi,r)|\approx   \left|\frac{\partial A}{\partial \pi}\right|\cdot e_a(\pi) +\left|\frac{\partial A}{\partial 
r}\right|\cdot e_a(r)  =  r^2\cdot e_a(\pi) + 2 \pi r \cdot e_a(r).
$$

Por ejemplo, si $r=1$ cm, con $e_a(r)\leq 0.001$ y $\pi =3.142$, $e_a(\pi)\leq 0.001$, obtenemos:
$$
|A(\pi +e_a(\pi),r+e_a(r))-A(\pi,r)|\leq  1.001^2\cdot 0.001 + 2\cdot 3.143 \cdot 1.001\cdot 0.001  
=  0.0073.
$$
</div>

# Algoritmos y convergencia

## Introducción

Los métodos numéricos están basados en **algoritmos**:

<l class="definition">Definición de algoritmo.</l>
Un **algoritmo** es un procedimiento que describe de forma no ambigua una secuencia finita de pasos en un orden específico.

Para describir los **algoritmos** usamos lo que denominamos **pseudocódigo**, que sería una forma de describir el algoritmo de una manera *taquigráfica*, es decir, escueta y entendible.

## Pseudocódigo
El **pseudocódigo** usa los símbolos siguientes:

* Punto `.` indica que un paso ha finalizado. 
* Un punto y coma `;` separa las tareas dentro de un mismo paso.
* Bucles: se indican con `For`, por ejemplo `For i=1 hasta n`.
* Asignación de variables: se indican con `Set`, por ejemplo `Set x=2`.
* Condiciones de control: 
    * `While`: para realizar unos pasos mientras se verifique una condición, por ejemplo `While n<10 do Steps 2-5`.
    * `If condición then`: para hacer tareas condicionales, si se verifica la condición, realizar lo siguiente. También existe `If condición then tarea 1 else tarea 2` que significa que si se verifica la condición hacer la tarea 1, en caso contrario, hacer la tarea 2.
    
## Ejemplo: ver si un número es primo
<div class="example">

El pseudocódigo siguiente nos permite identificar si un número es primo o no:

* `INPUT n.` (entramos el entero)  
* `Set i=2.` (inicializamos el divisor)  
* `While (i< sqrt(n)) do`  (mientras el divisor sea menor que la raíz de `n`, vamos dividiendo...)  
    * `If (resto(n/i)==0) do` (si encontramos un divisor para el cual el resto es cero, significa que  $n$ no es primo)  
      * `print (n no es primo);`  
      * `salir del While y fin.`
    * `Else  i++.` (en caso contrario, aumentamos el divisor en una unidad)
    
Observar cómo se han indentado los pasos.
</div>

## Ejemplo: Cálculo de $\mathrm{e}$
<div class="example">

En el capítulo de preliminares vimos como calcular una aproximación del número $\mathrm{e}$ dada una tolerancia a partir del **polinomio de MacLaurin** de la función $\mathrm{e}^x$ de grado $n$:
$$
P_n(x)=\sum_{k=0}^n \frac{f^{(k)}(0)}{k!}\cdot x^k = \sum_{k=0}^n \frac{x^k}{k!},
$$

El pseudocódigo para hallar la aproximación es el siguiente:


</div>


## Ejemplo: Cálculo de $\mathrm{e}$ (continuación)
<div class="example">
* `INPUT TOL, N.` (damos el valor de la tolerancia con que queremos hallar la aproximación de $\mathrm{e}$ y el grado máximo del polinomio $N$)
* `Set n=2` (empezamos con el polinomio de grado $2$)
* `While ((3/n!)>= TOL and n<=N)` (mientras se cumpla esta condición, no tenemos asegurada que el error de la aproximación sea menor que la tolerancia. También nos aseguramos que el grado no supere el grado máximo $N$)
    * `n++.` (aumentamos el grado del polinomio)
* `If n>=N` (si hemos llegado al grado máximo, damos un mensaje de error)
    * `Print 'error, no se puede alcanzar la aproximación pedida';`
    * `fin.`
* `Else` (en caso contrario hallamos la aproximación)
    * `Set x=1.` (damos a la aproximación $x$ el valor $1$ que es el primer sumando del polinomio de grado $n$)
    * `For i=1 hasta n` (en este bucle vamos añadiendo todos los demás sumandos del polinomio)
        * `Set x=x+1/i!.`
* `Print x.` (damos la aproximación)

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/18KBWA-iT6v6UdTK4Sh_TWPpU2xZxZGpS#scrollTo=r_Hna7O9Oxur)
</div>
                                       

</div>

## Tipos de algoritmos
Los algoritmos se dividen en dos clases bien diferenciadas:

* **Estables**: un algoritmo en el que pequeños cambios en los valores iniciales provoca pequeños cambios en los resultados finales, se dice que es **estable**.
* **Inestables**: un algoritmo que en cambio, cambios pequeños en las condiciones iniciales provocan grandes diferencias en los resultados finales, se dice que es **inestable**.

## Ejemplo
<div class="example">

Nos planteamos calcular la integral siguiente:
$$
I_n = \int_0^1 x^{2n}\sin(\pi x)\, dx,\ n=0,2,\ldots N,
$$
donde $N$ es un valor que nos dan.

Para ello usamos la recurrencia siguiente:
$$
I_{n}=\frac{1}{\pi}-\frac{2n(2n-1)}{\pi^2}I_{n-1}.
$$
Para probar la recurrencia anterior, basta aplicar la técnica de integración por partes dos veces a $I_n$:
$$
\begin{align*}
I_n = & \int_0^1 x^{2n}\sin(\pi x)\, dx =\left[-\frac{1}{\pi} x^{2n}\cos (\pi x)\right]_0^1+\frac{2n}{\pi}\int_0^1 x^{2n-1}\cos (\pi x)\, dx\\
= & \frac{1}{\pi}+\frac{2n}{\pi}\left(\left[x^{2n-1}\frac{1}{\pi}\sin(\pi x)\right]_0^1-\frac{2n-1}{\pi}\int_0^1 x^{2n-2}\sin(\pi x)\, dx\right)= \frac{1}{\pi}-\frac{2n (2n-1)}{\pi^2}I_{n-1}.
\end{align*}
$$
</div>

## Ejemplo (continuación)
<div class="example">


El valor de $I_0$ será:
$$
I_0 = \int_0^1 \sin(\pi x)\, dx=-\frac{1}{\pi} [\cos(\pi x)]_0^1 =\frac{2}{\pi}\approx 0.63662.
$$
Suponemos que aplicamos la recurrencia anterior para hallar los valores aproximados de $I_n$. Nos planteamos averiguar si el algoritmo planteado es estable o inestable.

Como tenemos un valor aproximado de $I_0$, ($I_0 \approx 0.63662$) nuestras condiciones iniciales están perturbadas, es decir, tenemos un pequeño cambio respecto con el valor exacto de $I_0$. ¿Cómo afectará este pequeño cambio a los valores futuros $I_n$?



Veámoslo:


$$
\begin{align*}
I_1= & \frac{1}{\pi}-\frac{2}{\pi^2}\cdot I_0 =\frac{1}{\pi}-\frac{2}{\pi^2}\cdot \frac{2}{\pi}\approx 0.189304. 
\end{align*}
$$
</div>


## Ejemplo (continuación)
<div class="example">

Los demás valores se observan en la tabla siguiente:

<div class="center">
|$n$| $I_n$|
|:---:|:---:|
|$1$|$0.1893037$
|$2$|$0.0881441$
|$3$|$0.0503839$
|$4$|$0.0324325$
|$5$|$0.0225607$
</div>
</div>

## Ejemplo (continuación)
<div class="example">

Los demás valores se observan en la tabla siguiente:

<div class="center">
|$n$| $I_n$|
|:---:|:---:|
|$6$|$0.016574$
|$7$|$0.0126785$
|$8$|$0.0100056$
|$9$|$0.0080938$
|$10$|$0.0066803$
</div>
</div>



## Ejemplo (continuación)
<div class="example">

<div class="center">
|$n$| $I_n$|
|:---:|:---:|
|$11$|$0.0056046$
|$12$|$0.0048512$
|$13$|$-0.001186$
|$14$|$0.409155$
|$15$|$-35.7484695$
</div>
</div>

## Ejemplo (continuación)
<div class="example">

Vemos que los valores $I_{13}$ e $I_{15}$ son negativos pero la función que integramos, $x^{2n}\sin(\pi x)$, es positiva para todo valor de $x\in (0,1)$ en el intervalo de integración. Por tanto, no puede dar valores negativos.

¿Nos hemos equivocado? ¡No! El algoritmo aplicado es inestable. Veamos el porqué.

Sea $\Delta I_{n-1}$ el error que tenemos del valor de $I_{n-1}$, como
$$
I_{n}=\frac{1}{\pi}-\frac{2n(2n-1)}{\pi^2}I_{n-1},
$$
lo que calculamos en la práctica es:
$$
I_{n}+\Delta I_n =\frac{1}{\pi}-\frac{2n(2n-1)}{\pi^2} (I_{n-1}+\Delta I_{n-1}).
$$
Para simplificar nuestro análisis, vamos a suponer que el número $\pi$ no tiene error, entonces de la igualdad anterior, tenemos que los errores $\Delta I_n$ verifican:
$$
\Delta I_{n}=-\frac{2n(2n-1)}{\pi^2}\Delta I_{n-1}.
$$
</div>

## Ejemplo (continuación)
<div class="example">

Es decir, en el paso $n$-ésimo el error de $I_{n-1}$ se multiplica por $\frac{2n(2n-1)}{\pi^2}$. Dicha sucesión tiende a infinito a medida que $n$ crece, de lo que deducimos que $\displaystyle\lim_{n\to\infty} \Delta I_n =\infty$, y, por tanto, el algoritmo es inestable.

¿Cómo podemos calcular de forma eficiente los valores $I_n$?

Fijémonos que si escribimos la recurrencia "al revés", obtenemos:
$$
I_{n-1}=\frac{\pi}{2n(2n-1)}-\frac{\pi^2}{2n(2n-1)}I_{n}.
$$
Si realizamos el análisis del error que hemos hecho antes, tenemos que si aplicamos la recurrencia anterior, los errores $\Delta I_n$ verifican:
$$
\Delta I_{n-1}=-\frac{\pi^2}{2n(2n-1)}\Delta I_n,
$$
es decir, se van multiplicando por $-\frac{\pi^2}{2n(2n-1)}$ y como dicha sucesión tiende a cero, los errores van disminuyendo y el algoritmo sería estable.
</div>

## Ejemplo (continuación)
<div class="example">

Ahora bien, aplicar la recurrencia anterior no tiene mucho sentido ya que tendríamos que calcular $I_{n-1}$ en función de $I_n$.

Recordemos la definición de los valores $I_n$: $\displaystyle I_n = \int_0^1 x^{2n}\sin(\pi x)\, dx$. Veamos que $\displaystyle\lim_{n\to\infty} I_n=0$:
$$
|I_n|=\left|\int_0^1 x^{2n}\sin(\pi x)\, dx\right|\leq \int_0^1 x^{2n}\, dx =\left[\frac{x^{2n+1}}{2n+1}\right]_0^1 =\frac{1}{2n+1}.
$$
Aplicando el criterio del "sandwich" para límites de sucesiones tenemos que como $\displaystyle\lim_{n\to\infty} \frac{1}{2n+1}=0$, entonces $\displaystyle\lim_{n\to\infty} I_n=0$.

Entonces para aplicar el algoritmo anterior estable, escojamos un valor de $n$ tal que $|I_n|\leq \epsilon$, donde $\epsilon$ sería la tolerancia.
Por ejemplo si $\epsilon =0.001$, el valor de $n$ sería:
$$
|I_n|\leq \frac{1}{2n+1}\leq 0.001,\ \Rightarrow 2n+1\geq\frac{1}{0.001}=1000,\ \Rightarrow n\geq 999/2,\ \Rightarrow n\geq 500.
$$

Entonces empezaríamos con $n=500$, usando que $I_{500}\approx 0$ yendo hacia atrás usando la recurrencia anterior y calcularíamos los valores $I_n$, para $n=499.\ldots, 0$.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/18KBWA-iT6v6UdTK4Sh_TWPpU2xZxZGpS#scrollTo=MCuusLnwPKPl)
</div>
                                       
</div>


# Velocidad de convergencia

## Introducción
Muchos de los **algoritmos** basados en métodos numéricos consisten en hallar una sucesión de números reales convergente. 
Por tanto, es importante de cara a estudiar la **eficiencia** del algoritmo en cuestión saber a qué velocidad converge la sucesión.

En este apartado, vamos a definir formalmente la **velocidad de convergencia** de una sucesión.

<l class="definition">Definición de equivalencia de velocidad de convergencia.</l>
Sea $(x_n)_n$ una sucesión de números reales que converge a $L$ e $(y_n)_n$ otra sucesión convergente a cero. Diremos que $(x_n)_n$ tiene **orden de convergencia** $(y_n)_n$ si existe una constante $K>0$ y un natural $n_0$ tal que:
$$
|x_n-L|\leq K |y_n|,
$$
para todo $n\geq n_0$. En este caso, se escribe que $x_n =L +O(y_n)$.

## Introducción
La definición anterior nos permite reducir la convergencia de una sucesión a un valor cualquiera a sucesiones que convergen a cero.

Un tipo de sucesiones $(y_n)_n$ que convergen a cero que se suelen usar son $y_n=\frac{1}{n^k}$, con $k$ natural. 

## Ejemplo
<div class="example">

La sucesión $x_n =\frac{P_p(n)}{Q_p(n)}$, donde $P_p(n)$ y $Q_q(n)$ son polinomios de grados $p$ y $q$, respectivamente, con $q>p$ tiene orden de convergencia $\frac{1}{n^{q-p}}$. Veamos por qué.

En primer lugar, escribimos 
$$
\begin{align*}
P_p(n) = & a_p n^p +a_{p-1} n^{p-1}+\cdots + a_0,\\
Q_q(n) = & b_q n^q + b_{q-1} n^{q-1}+\cdots + b_0.
\end{align*}
$$
Entonces $\displaystyle\lim_{n\to\infty} x_n =\lim_{n\to\infty}  \frac{P_p(n)}{Q_q(n)}=0.$ 
</div>

## Ejemplo (continuación)
<div class="example">

Ahora bien,
$$
\begin{align*}
\frac{P_p(n)}{Q_q(n)} = & \frac{a_p n^p +a_{p-1} n^{p-1}+\cdots + a_0}{b_q n^q + b_{q-1} n^{q-1}+\cdots + b_0}=\frac{a_p n^p}{b_q n^q} \frac{\left(1+\frac{a_{p-1}}{a_p n}+\cdots +\frac{a_0}{a_p n^p}\right)}{\left(1+\frac{b_{q-1}}{b_q n}+\cdots +\frac{b_0}{b_q n^p}\right)} \\ = &  \frac{a_p}{b_q}\frac{1}{n^{q-p}}\left(1+\frac{a_{p-1}}{a_p n}+\cdots +\frac{a_0}{a_p n^p}\right)\cdot \left(1-O\left(\frac{1}{n}\right)\right)
\\ = & \frac{a_p}{b_q}\frac{1}{n^{q-p}}\left(1+O\left(\frac{1}{n}\right)\right)\cdot \left(1-O\left(\frac{1}{n}\right)\right) = \frac{a_p}{b_q}\frac{1}{n^{q-p}} \left(1+O\left(\frac{1}{n}\right)\right).
\end{align*}
$$
Entonces, podemos escribir que existen valores $K=\left|\frac{a_p}{b_q}\right|$ y $n_0$ tal que:
$$
\left|\frac{P_p(n)}{Q_q(n)} \right|\leq K\frac{1}{n^{q-p}},
$$
para $n\geq n_0$, tal como queríamos ver.
</div>

## Error en los algoritmos
Existen algoritmos que, en lugar de darnos una sucesión como resultado, nos dan una expresión cuyo error depende de un parámetro $h$, supuestamente pequeño. Interesa que el error de los algoritmos tenga la forma $K\cdot h^k$, con $K$ y $k>0$ constantes. Cuanto mayor sea la constante $k$, mejor es el algoritmo o más rápido converge a la solución.

Por dicho motivo, vamos a definir el orden de convergencia para funciones numéricas que dependan de un valor continuo $h$:

## Definición de velocidad de convergencia
<l class="definition">Definición de equivalencia de velocidad de convergencia.</l>
Sean $F$ y $G$ dos funciones reales de variable real. Supongamos que existe un valor $L$ tal que $\displaystyle\lim_{h\to 0}F(h)=L$ y $\displaystyle\lim_{h\to 0}G(h)=0$.  Diremos que $F(h)$ tiene **orden de convergencia** $G(h)$ si existen constante $K>0$ y $c>0$ tal que:
$$
|F(h)-L|\leq K |G(h)|,
$$
si $|h|\leq c$ para todo $n\geq n_0$. En este caso, se escribe que $F(h) =L +O(G(h))$.

Un tipo de funciones $G(h)$ que convergen a cero que se suelen usar son $G(h)=h^k$, con $k>0$.

## Ejemplo
<div class="example">

Veamos que la función $F(h)=\mathrm{e}^h$ tiene orden de convergencia $G(h)=h$.

En este caso, el valor $L$ sería $\displaystyle\lim_{h\to 0} F(h)=\lim_{h\to 0}\mathrm{e}^h=1$. Entonces, usando la expresión del error del desarrollo de MacLaurin de la función $\mathrm{e}^h$ tenemos que:
$$
F(h)=\mathrm{e}^h =1+\mathrm{e}^\xi h,
$$
con $\xi\in <0,h>$. Por tanto, existe un valor $c>0$ tal que si $|h|\leq c$,
$$
|F(h)-1| =|\mathrm{e}^h-1| =\mathrm{e}^\xi |h|\leq \max\{\mathrm{e}^h,1\} |h|\leq \mathrm{e}^c |h| = K |h|. 
$$
donde hemos usado que $\mathrm{e}^\xi \leq \mathrm{e}^h$ si $h>0$ y $\mathrm{e}^\xi \leq 1$, si $h<0$.
</div>


## Ejemplo (continuación)
<div class="example">

En general, si consideramos $F_k(h)=\mathrm{e}^h-\left(1+h+\frac{h^2}{2}+\cdots +\frac{h^k}{k!}\right)$ tenemos que $F_k(h)$ tiene orden de convergencia $h^{k+1}$.

Usando la expresión del error del desarrollo de MacLaurin de la función $\mathrm{e}^h$ de orden $k+1$, tenemos que:
$$
F_k(h)=\mathrm{e}^h-\left(1+h+\frac{h^2}{2}+\cdots +\frac{h^k}{k!}\right) =\frac{e^\xi}{(k+1)!}h^{k+1},
$$
con $\xi\in <0,h>$. Por tanto, existe un valor $c>0$ tal que si $|h|\leq c$,
$$
\begin{align*}
|F_k(h)| = &   \left|\mathrm{e}^h-\left(1+h+\frac{h^2}{2}+\cdots +\frac{h^k}{k!}\right)\right| =\frac{e^\xi}{(k+1)!}\left|h^{k+1}\right|\leq\frac{\max\{\mathrm{e}^h,1\}}{(k+1)!}\left|h^{k+1}\right|\\ \leq &  \frac{\mathrm{e}^c}{(k+1)!}\left|h^{k+1}\right|=K\left|h^{k+1}\right|,
\end{align*}
$$
tal como queríamos ver.
</div>

