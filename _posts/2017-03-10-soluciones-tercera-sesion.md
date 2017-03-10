---
layout: post
title:  "Soluciones Tercera Sesión"
date: 2017-03-09 20:00:03 -0600
author: csssaz
categories: solucion editorial código
mathjax: true
---

Los resultados del segundo concurso fueron los siguientes:

![Scoreboard]({{site.url}}/assets/scoreboard3.png)

¡Felicidades a todos!

En este post explicaremos las soluciones:

### [675B](http://codeforces.com/problemset/problem/675/B) Restoring Painting

Consideremos a la siguiente matriz, donde reemplazamos los valores desconocidos con las variables $w, x, m, y$ y $z$.

$$ 
\left[
\begin{array}{ccc}
w & a & x \\
b & m & c \\ 
y & d & z
\end{array}
\right]
$$

El problema nos pide encontrar la cantidad total de tuplas $(w, x, m, y, z)$ tal que la suma de cada cuadrado de $2 x 2$ sea igual, y $ 1 \le w, x, m, y, z \le n$.

Una solución que algunos de ustedes intentaron, es probar cada posible combinación de las incógnitas mencionadas. Para esto, podríamos simplemente utilizar 5 *fors* anidados, iterando desde 1 a n y probar si la igualdad se cumple. Lastimosamente(?), las computadoras con las que contamos tienen recursos limitados, y realizar dicho cómputo nos demoraría alrededor de $n^5$ operaciones (¿por qué?). Dado que el problema nos indica que $ 1 \le n \le 100 000$, esto es claramente es muy lento (como rule of thumb, se pueden realizar aproximadamente ~$10^9$ operaciones en un segundo en un servidor como el de Codeforces).


Por lo tanto, tenemos que pensar en algo mejor que eso. La solución más sencilla es fijar el elemento $m$, y determinar si es posible cumplir con las condiciones deseadas.

Es decir, dado m (cualquier m), podemos obtener expresiones para $y, x, z$ si consideramos la igualidad en suma entre los cuadrados:

$$
a + b + w + m = b + m + y + d \Rightarrow y = a + w - d \\
a + b + w + m = a + x + m + c \Rightarrow x = b + w - c \\
a + c + x + w = c + d + z + m \Rightarrow z = a + x - d \\
$$

Ahora, si $1 \le x, y, z \le n$, sumamos $n$ a nuestra respuesta, ya que encontramos una solución posible dada cualquier $m$ (i.e. *n posibles m's*).

Finalmente, notar que para encontrar $y, x, z$ sólo necesitamos el valor de w (a y d los obtenemos como entrada en el problema). Por lo tanto, podemos iterar sobre los posibles valores de $w$.

{% highlight c++ %}

int ok(int x, int n) {
  return x >= 1 && x <= n;
}

int main() {
  int n, a, b, c, d;
  scanf("%d %d %d %d %d", &n, &a, &b, &c, &d);
  int w;
  long long ans = 0;
  for (w = 1; w <= n; w++) {
    int y = a + w - d;
    int x = b + w - c;
    int z = a + x - d;
    if (ok(y, n) && ok(x, n) && ok(z, n)) {
      ans += n;
    }
  }
  printf("%lld\n", ans);
  return 0;
}
{% endhighlight %}

### [673A](http://codeforces.com/problemset/problem/673/A) Bear and Game

En este problema, basta con iterar sobre los *minutos interesantes*, siendo cuidadosos de mantener la diferencia entre el último minuto interesante (inicialmente en $t=0$), y el minuto actual. Si la diferencia es $> 15$, sabemos que la respuesta es $min(90, t + 15)$ donde $t$ es el último minuto interesante que encontramos:

{% highlight c++ %}
int min(int a, int b) {
  return a < b ? a : b;
}

int main() {
  int n;
  scanf("%d", &n);
  int prev_time = 0, curr_time = 0, i;
  int ans = 0;
  for (i = 0; i < n; i++) {
    scanf("%d", &curr_time);
    if (curr_time - prev_time > 15) {
      break;
    }
    prev_time = curr_time;
  }
  printf("%d\n", min(90, prev_time + 15));
  return 0;
}
{% endhighlight %}


### [667A](http://codeforces.com/problemset/problem/667/A) Infinite Sequence

El problema nos da una progresión aritmética, cuyo primer elemento es $a$ y la diferencia entre elementos consecutivos es $c$, y nos pregunta si el número $b$ es parte de dicha secuencia.

Basta con revisar si $(b - a) \mod c = 0$, ya que la diferencia entre cualquier par de elementos de la progresión tiene que ser divisible entre el incremento.

Lo único complicado es que hay casos *tricky* como cuando $c \le 0$ o $c = 0$.

{% highlight c++ %}
int main() {
  int a, b, c;
  scanf("%d %d %d", &a, &b, &c);
  if (c == 0) {
    if (a == b) puts("YES");
    else puts("NO");
  }
  else if ((b-a) % c == 0 && (b-a) / c >= 0) {
    puts("YES");
  }
  else {
    puts("NO");
  }
  return 0;
}
{% endhighlight %}

### [667A](http://codeforces.com/problemset/problem/667/A) Pouring Rain

Para saber si la respuesta es no, debemos comparar la velocidad a la cual la lluvia llena el vaso, con la velocidad a la cuál tomamos líquido del mismo. 

Para esto, notar que la velocidad a la que tomamos está en $\frac{ml}{s}$ y la velocidad a la cuál la lluvia llena el vaso en $\frac{cm}{s}$. Por lo tanto, convertimos la velocidad de la lluvia a $\frac{ml}{s}$, multiplicándola por el área de la base (ya que por cada cm que se aumente, el volúmen se incrementa en esa proporción).

Finalmente, calculamos el tiempo total dividiendo el volúmen entre la diferencia de velocidades.

{% highlight c++ %}
double sq(double x) {
  return x * x;
}

int main() {
  const double PI = 4*atan(1);
  double d, h, v, e;
  scanf("%lf %lf %lf %lf", &d, &h, &v, &e);
  e = PI * sq(d/2.0) * e;
  if (e > v) {
    puts("NO");
  }
  else {
    double volume = PI * sq(d/2.0) * h;
    double ans = volume / (v-e);
    printf("YES\n%.10lf\n", ans);
  }
  return 0;
}
{% endhighlight %}


### [678A](http://codeforces.com/problemset/problem/678/A) Johny Likes Numbers

Este era probablemente el problema más sencillo. Sólo tenemos que hacer lo que nos dice. Para ello, notar que si n es divible entre k, la respuesta es $n + k$ (ya que nos pide el entero $x$ *mas grande que n*). Y si no es divisible, el primer múltiplo de k más grande que n.

{% highlight c++ %}
int main() {
  int n, k, ans;
  scanf("%d %d", &n, &k);
  if (n % k == 0) {
    ans = n + k;
  }
  else {
    ans = (n/k + 1) * k;
  }
  printf("%d\n", ans);
  return 0;
}
{% endhighlight %}


### [667B](http://codeforces.com/problemset/problem/667/B) Coat of Anticubism

Hay dos formas de hacer este problema. Una, es *adivinar* (o confiar en la intuición) la respuesta viendo los dos casos de entrada(!), y la otra es saber de la existencia de la [inequidad del triángulo](https://en.wikipedia.org/wiki/Triangle_inequality) (que es algo fundamental, y no está demás saberlo).

De forma general, esta dice que el lado más largo de un triángulo, es menor a la suma de los otros dos lados. Por inducción, se puede extender este argumento a polígonos convexos de n lados y obtener el resultado que el lado más largo debe ser menor a la suma de los otros lados.

El problema nos garantiza que no se puede construir un polígono convexo con los lados que nos da, y por lo tanto, existe un lado que es mayor o igual a la suma de todos los demás. Entonces, basta con encontrar el lado más grande, y añadir un lado cuyo tamaño es igual a $k + 1$, donde $k$ es la diferencia entre el lado más grande, y la suma de todos los demás:

{% highlight c++ %}
long long max(long long a, long long b) {
  return a > b ? a : b;
}

int main() {
  int n, i;
  scanf("%d", &n);
  long long longest = 0;
  long total_sum = 0;
  for (i = 0; i < n; i++) {
    long long l;
    scanf("%lld", &l);
    longest = max(l, longest);
    total_sum += l;
  }
  long long ans = longest - (total_sum - longest) + 1;
  printf("%lld\n", ans);
  return 0;
}
{% endhighlight %}


### [641A](http://codeforces.com/problemset/problem/641/A) Little Artem and Grasshopper

Simulemos los saltos del saltamontes marcando en un arreglo las posiciones que visitamos después de cada salto. Si en algún momento nos "salimos" del arreglo (i.e. estamos en una posición $ p \ge n $ o $p < 0$), sabemos que la respuesta es finita, ya que no podemos regresar a nuestro arreglo! Por otro lado, si llegamos a una posición que ya visitamos (i.e. está marcada), significa que la respuesta es infinita, ya que nos encontramos en un *ciclo*.

De forma más general, este problema nos da un [grafo](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)). Es decir, un conjunto de nodos (las posiciones en el arreglo), y arcos (los saltos entre posiciones) que conectan a los nodos. Empezando en un nodo inicial, queremos detectar si existe un ciclo, es decir, una secuencia de nodos conectados (camino) tal que empecemos y terminemos en el mismo nodo.

Por ejemplo, el segundo caso de entrada representa el siguiente grafo:

![Grafo]({{site.url}}/assets/graph.png)

Es claro ver que nos quedamos en el ciclo (0 -> 2 -> 1 -> 2 -> ....).
{% highlight c++ %}

int main() {
  const int MAX = (int)1e5;
  int n, i;
  scanf("%d", &n);
  char direction[MAX];
  scanf("%s", direction);
  int step[MAX];
  int visited[MAX];
  for (i = 0; i < n; i++) {
    visited[i] = 0; // 0 significa que no está visitado
    scanf("%d", &step[i]);
  }
  int pos = 0;
  visited[pos] = 1;
  while (1) {
    // realizamos el salto
    if (direction[pos] == '>') {
      pos = pos + step[pos];
    }
    else {
      pos = pos - step[pos];
    }
    if (pos < 0 || pos >= n) { // nos salimos!
      puts("FINITE");
      break;
    }
    if (visited[pos] == 1) { // encontramos el ciclo
      puts("INFINITE");
      break;
    }
    visited[pos] = 1; // marcamos posicion como visitada
  }
  return 0;
}
{% endhighlight %}

### [677B](http://codeforces.com/problemset/problem/677/B) Vanya and Food Processor

Debemos simular el proceso que hace Vanya para moler todas las papas. Sin embargo, simular el proceso que se describe en el problema es demasiado lento: en el peor de los casos nos dan $10^5$ papas, cada una de tamaño $10^9$, y la *velocidad* de procesamiento $k = 1$. Entonces, si lo hacemos de forma ingenua, terminaríamos haciendo $10^{14}$ operaciones, que son claramente demasiadas.

Por lo tanto, notar que tardaremos $\lfloor \frac{p[i]}{k} \rfloor$ segundos para procesar la iésima papa, y nos quedará $p[i]\mod k$ restantes. A esto restante, le podemos añadir la siguiente papa si es que la suma es menor o igual a $h$. Sino, utilizamos un segundo más. Este proceso es el mismo que el anterior, pero sólo realizamos n pasos.

{% highlight c++ %}

int main () {
  int n, i;
  long long k, h;
  scanf("%d %lld %lld", &n, &h, &k);
  long long ans = 0;
  long long curr_height = 0;
  for (i = 0; i < n; i++) {
    long long p;
    scanf("%lld", &p);
    if (curr_height + p <= h) {
      curr_height += p;
    }
    else {
      ans++;
      curr_height = p;
    }
    ans += curr_height / k;
    curr_height = curr_height % k;
  }
  ans += curr_height / k;
  if (curr_height % k != 0) ans++;
  printf("%lld\n", ans);
  return 0;
}

{% endhighlight %}

