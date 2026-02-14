# Tarea4



Paul Maguiña Quispe

# Problema 1


Para modelar rigurosamente el número total de iteraciones, primero analizamos el comportamiento del bucle interno mediante una inspección lógica de sus ejecuciones para valores fijos de $i$.

**Análisis de ejecuciones por iteración:**
El bucle interno inicializa $j=i$ y avanza con `j += i` (saltos de $i$) mientras $j \le n$.

* **Si $i=1$:** $j$ toma los valores $1, 2, 3, \dots, n$. (Recorre todos los enteros).
    * Total: $n/1$ iteraciones.
* **Si $i=2$:** $j$ toma los valores $2, 4, 6, 8, \dots$. (Recorre los pares).
    * Total: $\approx n/2$ iteraciones.
* **Si $i=3$:** $j$ toma los valores $3, 6, 9, 12, \dots$. (Recorre múltiplos de 3).
    * Total: $\approx n/3$ iteraciones.

Para un $i$ dado, el bucle interno se ejecuta tantas veces como $i$ quepa en $n$.

$$Iteraciones_i = \left\lfloor \frac{n}{i} \right\rfloor$$

Podemos expresar esto como una sumatoria simple:

$$T(n) = \sum_{i=1}^{n} \left\lfloor \frac{n}{i} \right\rfloor$$

**Doble Sumatoria:**
cualquier número entero $K$ puede expresarse como la suma de $1$ sumado $K$ veces ($\sum_{k=1}^{K} 1 = K$).

Sustituimos el término $\lfloor \frac{n}{i} \rfloor$ por su representación en sumatoria:

$$\left\lfloor \frac{n}{i} \right\rfloor \equiv \sum_{k=1}^{\lfloor n/i \rfloor} 1$$

Sustituyendo esto en la ecuación de $T(n)$, obtenemos el modelo final:

$$T(n) = \sum_{i=1}^{n} \sum_{k=1}^{\lfloor n/i \rfloor} 1$$



Teniendo el tiempo de ejecucion en una sumatoria.

$$T(n) = \sum_{i=1}^{n} \left\lfloor \frac{n}{i} \right\rfloor$$

Para obtener una cota superior ajustada, eliminamos la función piso (dado que $\lfloor x \rfloor \le x$):

$$T(n) \le \sum_{i=1}^{n} \frac{n}{i}$$

Factorizamos $n$ fuera de la sumatoria, ya que es una constante respecto al índice $i$:

$$T(n) = n \cdot \sum_{i=1}^{n} \frac{1}{i}$$



La expresión resultante $\sum_{i=1}^{n} \frac{1}{i}$ es, por definición, la serie arrmónica ($H_n$).

El rol de la serie armónica describe cómo disminuye la frecuencia de ejecución del bucle interno a medida que $i$ crece.  La serie armónica crece logarítmicamente:

$$H_n \approx \ln(n) $$

Sustituyendo esto en nuestra ecuación de tiempo, tenemos la cota superior ajustada:

$$T(n) \approx n \cdot \ln(n)$$

En notación Big-O:

$$T(n) = O(n \log n)$$




# Problema 2:


Analizamos el número de iteraciones del bucle interno en función del índice del bucle externo $i$.


El bucle externo va de $i=1$ hasta $n$.

El bucle interno va de $j=1$ hasta $n-i+1$.

Analicemos cuántas veces se ejecuta el cuerpo del bucle interno para los primeros valores de $i$:

* **Si $i=1$:** El límite es $n-1+1 = n$. Se ejecuta $n$ veces.
* **Si $i=2$:** El límite es $n-2+1 = n-1$. Se ejecuta $n-1$ veces.
* **Si $i=3$:** El límite es $n-3+1 = n-2$. Se ejecuta $n-2$ veces.
* ...
* **Si $i=n$:** El límite es $n-n+1 = 1$. Se ejecuta $1$ vez.

Observamos que el número de iteraciones decrece linealmente a medida que $i$ aumenta.

El número total de operaciones T(n) es la suma de las iteraciones del bucle interno para cada i.

$$T(n) = \sum_{i=1}^{n} (n - i + 1)$$

Podemos ver que esta sumatoria representa la suma de los enteros desde $n$ descendiendo hasta $1$. Podemos reordenar los términos de la suma para verla como una suma desde $1$ hasta $n$:

$$T(n) =  \sum_{i=1}^{n} (n - i + 1) = n + (n-1) + (n-2) + \dots + 1 = \sum_{k=1}^{n} k$$


Reconocemos que $\sum_{k=1}^{n} k$ es la suma aritmética:

$$T(n) = \frac{n(n+1)}{2}$$

Expandiendo la expresión para identificar el término dominante:

$$T(n) = \frac{n^2 + n}{2} = \frac{1}{2}n^2 + \frac{1}{2}n$$

Por lo tanto, la cota superior del comportamiento del algoritmo está dominada por el término cuadrático $n^2$.

Dado que la función de tiempo $T(n)$ es un polinomio de segundo grado, eliminamos las constantes y los términos de menor orden para la notacion Big O.

$$T(n) = O(n^2)$$



# Problema 3:


El bucle while divide i entre 2 al inicio de cada iteración. El bucle interno for se ejecuta i veces con este nuevo valor.
Esto genera la secuencia de iteraciones: $\frac{n}{2}, \frac{n}{4}, \frac{n}{8}, \dots, 1$.

El número total de operaciones es la suma de estos términos desde $k=1$ hasta que i llega a 1 aprox $\log_2 n$ veces:

$$T(n) = \sum_{k=1}^{\lfloor \log_2 n \rfloor} \frac{n}{2^k}$$

**Inducción**

Para probar $$\sum_{k=0}^{\lfloor \log n \rfloor} \frac{n}{2^k} \le 2n$$, demostraremos por inducción la fórmula exacta de la suma geométrica finita para cualquier entero $m \ge 0$.

**Proposición $P(m)$:**
$$\sum_{k=0}^{m} \frac{n}{2^k} = 2n - \frac{n}{2^m}$$

* **Paso Base ($m=0$):**
    * Lado izquierdo: $\frac{n}{2^0} = \frac{n}{1} = n$.
    * Lado derecho: $2n - \frac{n}{2^0} = 2n - n = n$.
    * **Se cumple ($n=n$).**

* **Hipótesis Inductiva:**
    Asumimos que la proposición es verdadera para un entero $m$:
    $$\sum_{k=0}^{m} \frac{n}{2^k} = 2n - \frac{n}{2^m}$$

* **Paso Inductivo ($m+1$):**
    Queremos demostrar que $\sum_{k=0}^{m+1} \frac{n}{2^k} = 2n - \frac{n}{2^{m+1}}$.

    Descomponemos la sumatoria separando el último término:
    $$\sum_{k=0}^{m+1} \frac{n}{2^k} = \left( \sum_{k=0}^{m} \frac{n}{2^k} \right) + \frac{n}{2^{m+1}}$$

    Sustituimos la sumatoria entre paréntesis usando la Hipótesis Inductiva:
    $$= \left( 2n - \frac{n}{2^m} \right) + \frac{n}{2^{m+1}}$$

    Para operar las fracciones, expresamos $\frac{n}{2^m}$ con denominador $2^{m+1}$ (multiplicando numerador y denominador por 2):
    $$= 2n - \frac{2n}{2^{m+1}} + \frac{n}{2^{m+1}}$$
    $$= 2n - \left( \frac{2n - n}{2^{m+1}} \right)$$
    $$= 2n - \frac{n}{2^{m+1}}$$
  
    **demostrado**


Se prueba que la suma exacta es $S_m = 2n - \frac{n}{2^m}$.
Dado que el término $\frac{n}{2^m}$ es siempre positivo, restarlo a $2n$ implica que el resultado es siempre estrictamente menor que $2n$:

$$2n - \frac{n}{2^m} < 2n$$

Por lo tanto, se cumple la desigualdad solicitada:
$$\sum_{k=0}^{\lfloor \log n \rfloor} \frac{n}{2^k} \le 2n$$


El tiempo de ejecución del algoritmo $T(n)$ corresponde a la sumatoria desde $k=1$, la cual es estrictamente menor que la sumatoria desde $k=0$ que acabamos de acotar.

$$T(n) < \sum_{k=0}^{\lfloor \log n \rfloor} \frac{n}{2^k} \le 2n$$

Al haber demostrado que $T(n) \le 2n$, aplicamos la notacion de Big-O:

$$T(n) = O(n)$$




# Problema 4:


El bucle interno inicializa $k=1$ y en cada iteración actualiza $k$ multiplicándolo por 2 (k = k * 2). El bucle continúa mientras $k < i$.

Analicemos la progresión de $k$:
$$k \in \{ 1, 2, 4, 8, \dots, 2^m \}$$
El bucle se detiene cuando $2^m \ge i$.

Despejando $m$ (número de iteraciones):
$$2^m \approx i \implies m \approx \log_2(i)$$

El número de veces que se ejecuta el cuerpo del bucle para un $i$ dado es el techo del logaritmo:
$$Iteraciones_i = \lceil \log_2 i \rceil$$

El tiempo total de ejecución $T(n)$ es la suma de las iteraciones del bucle interno para todos los valores de $i$ desde 1 hasta $n$:

$$T(n) = \sum_{i=1}^{n} \lceil \log_2 i \rceil$$

Trabajamos con la aproximación continua:

$$T(n) \approx \sum_{i=1}^{n} \log_2 i$$


Sabemos que para todo valor de $i$ en el rango del bucle ($1 \le i \le n$), se cumple que:
$$\log_2 i \le \log_2 n$$

Por lo tanto, podemos reemplazar cada término de la sumatoria por el valor máximo $\log_2 n$ para obtener una cota superior segura:

$$T(n) = \sum_{i=1}^{n} \log_2 i \le \sum_{i=1}^{n} \log_2 n$$

Como el término $\log_2 n$ es constante respecto al índice de la sumatoria $i$, podemos sacarlo como factor común (sumar el mismo valor $n$ veces es multiplicar por $n$):

$$T(n) \le n \cdot \log_2 n$$

Al haber demostrado que $T(n)$ es menor o igual a $n \log_2 n$, con notacion Big O:

$$T(n) = O(n \log n)$$



# Problema 6: 

El algoritmo R(n) realiza una llamada recursiva con el tamaño de la entrada dividido por 3 (R(n/3)) y agrega una operación constante (+1).

La relación de recurrencia es:
$$T(n) = T(n/3) + c$$

Para resolver la recurrencia, generamos un sistema de ecuaciones iterando el argumento recursivamente $k$ veces:

$$
\begin{aligned}
T(n) &= T\left(\frac{n}{3}\right) + c \\
T\left(\frac{n}{3}\right) &= T\left(\frac{n}{3^2}\right) + c \\
T\left(\frac{n}{3^2}\right) &= T\left(\frac{n}{3^3}\right) + c \\
&\vdots \\
T\left(\frac{n}{3^{k-1}}\right) &= T\left(\frac{n}{3^k}\right) + c
\end{aligned}
$$

**Suma y Cancelación:**
Sumamos todas las ecuaciones. Los términos que aparecen a ambos lados de la igualdad (en diagonal) se cancelan, dejando solo el primer término, el último término y la suma de las constantes.

$$T(n) = T\left(\frac{n}{3^k}\right) + k \cdot c$$



Para detener la recursión y resolver la ecuación, necesitamos llegar al **caso base**. Sabemos por el código que el algoritmo termina cuando el tamaño de la entrada es 1.

Por lo tanto, igualamos el argumento de la función recursiva final a 1:

$$\frac{n}{3^k} = 1$$

Despejamos $k$ (profundidad):
$$n = 3^k$$
$$k = \log_3(n)$$


Sustituimos el valor de $k$ y el caso base ($T(1)=1$) en la ecuación general obtenida.

$$T(n) = T(1) + c \cdot \log_3(n)$$
$$T(n) = 1 + c \cdot \log_3(n)$$

En notación Big-O:

$$T(n) = O(\log n)$$
