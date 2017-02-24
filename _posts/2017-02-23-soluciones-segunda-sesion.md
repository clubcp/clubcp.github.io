---
layout: post
title:  "Soluciones Segunda Sesión"
date: 2017-02-23 20:00:03 -0600
author: csssaz
categories: solucion editorial código
mathjax: true
---

Los resultados del segundo concurso fueron los siguientes:

![Scoreboard]({{site.url}}/assets/scoreboard2.png)

¡Felicidades a los que completaron todos los problemas!

En este post explicaremos brevemente las soluciones:

### [620A](http://codeforces.com/problemset/problem/620/A) Professor GukiZ's Robot

Al principio puede parecer que hay que calcular la distancia entre ambos puntos, pero el problema nos pide el *número de pasos*, no la distancia. 

De esta forma, notar que un paso nos permite movernos en el eje x por 1 unidad, eje y por 1 unidad, o 1 unidad en ambos ejes a la vez(!). Por lo tanto, es fácil notar que la solución es $max\(\|x_{1} - x_{2}\|, \|y_{1} - y_{2}\|\)$

{% highlight c++ %}

int max(int a, int b) {
  return a > b ? a : b;
}

int main () {
  int x1, y1, x2, y2;
  scanf("%d %d %d %d", &x1, &y1, &x2, &y2);
  printf("%d\n", max(abs(x1-x2), abs(y1-y2)));
  return 0;
}
{% endhighlight %}

### [669A](http://codeforces.com/contest/669/problem/A) Little Artem and Presents

La clave es darse cuenta que no nos conviene darle más de 2 piedras en una "misma vez" a Masha. Esto es fácil de probar:

Por contradicción, asumamos que si nos conviene. Esto significa que en algún "turno" le dimos a Masha $n > 2$ piedras. Asumamos que $n = 3$ (para $n > 3$ el argumento es similar). Sin embargo podríamos, en vez, darle primero 1 piedra, y en el siguiente turno 2. Esta es una secuencia válida (no se repite la cantidad de piedras), y terminamos dándole un regalo más a Masha, lo cuál contradice nuestra asumpción inicial que nos convenía darle más de 2.

Por lo tanto, la secuencia de regalos óptima tiene que ser $1, 2, 1, 2, ....$

Entonces, le podemos dar $2 * \frac{n}{3}$ regalos, y después nos quedamos con cero, una o dos piedras más (i.e. el residuo de la división entre 3). Si tenemos al menos una extra, le podemos dar un regalo más.

{% highlight c++ %}

int main () {
  int n;
  scanf("%d", &n);
  printf("%d\n", 2*(n/3) + (n%3 != 0));
  return 0;
}
{% endhighlight %}

### [615A](http://codeforces.com/problemset/problem/615/A) Bulbs

Basta con crear un arreglo que representa las luces y probar si podemos prenderlas todas apretando todos los botones.

{% highlight c++ %}

int main () {
  int n, m, i;
  int lights[110];
  scanf("%d %d", &n, &m);
  for (i = 0; i < m; i++) {
    lights[i] = 0;
  }
  for (i = 0; i < n; i++) {
    int num, j;
    scanf("%d", &num);
    for (j = 0; j < num; j++) {
      int l;
      scanf("%d", &l);
      lights[l - 1] = 1;
    }
  }
  int can = 1;
  for (i = 0; i < m; i++) {
    if (lights[i] == 0) {
      can = 0;
      break;
    }
  }
  if (can) puts("YES");
  else puts("NO");
  return 0;
}

{% endhighlight %}

### [602A](http://codeforces.com/problemset/problem/602/A) Two Bases

Sólo tenemos que convertir ambos números a base 10 y comparar los valores numéricos. Es probable que muchos hayan tenido problemas por el uso de la función pow. [Aquí](http://codeforces.com/blog/entry/21844) hay una buena explicación de por qué dicha función puede dar resultados inesperados. En general, [nunca](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html) es recomendado utilizar funciones que utilizan *doubles* o *floats* cuando queremos manipular enteros. Por lo tanto, debemos utilizar nuestra propia implementación de pow.

En este código, se hace uso de un [algoritmo](https://en.wikipedia.org/wiki/Exponentiation_by_squaring) muy sencillo y poderoso para realizar la exponenciación. Si normalmente realizamos $e$ pasos para calcular $b^e$ (i.e. $e$ multiplicaciones), con el algoritmo mencionado realizamos sólamente $log_{2}(e)$.

{% highlight c++ %}
long long pw(long long b, long long e) {
  long long ans = 1;
  while (e > 0) {
    if (e % 2 == 1) ans *= b;
    b *= b;
    e /= 2;
  }
  return ans;
}

int main () {
  int n, b, i;
  long long x = 0;
  scanf("%d %d", &n, &b);
  for (i = 0; i < n; i++) {
    int d;
    scanf("%d", &d);
    x += (long long)d * pw(b, n - i - 1);
  }
  scanf("%d %d", &n, &b);
  long long y = 0;
  for (i = 0; i < n; i++) {
    int d;
    scanf("%d", &d);
    y += (long long)d * pw(b, n - i - 1);
  }
  if (x > y) puts(">");
  else if (x < y) puts("<");
  else puts("=");
  return 0;
}
{% endhighlight %}


### [263A](http://codeforces.com/problemset/problem/263/A) Beatiful Matrix

El problema puede parecer difícil si pensamos que *realmente* hay intercambiar filas y columnas. Sin embargo, lo importante es notar que no necesitamos manipular nada. Cada *swap* nos permite mover la fila o columna del 1 por una posición. Por lo tanto, basta con calcular a cuántas filas y columnas estamos de la posición central.

También se puede interpretar la solución como la [distancia Manhattan](http://mathworld.wolfram.com/TaxicabMetric.html) entre el 1 y la posición central de la grilla de 5x5.

{% highlight c++ %}

int main () {
  int ans, i, j, x;
  for (i = 0; i < 5; i++) {
    for (j = 0; j < 5; j++){
      scanf("%d", &x);
      if (x == 1) ans = abs(j - 2) + abs(i - 2);
    }
  }
  printf("%d", &ans);
  return 0;
}
{% endhighlight %}

### [567A](http://codeforces.com/problemset/problem/567/A) Lineland Mail

Probablemente lo más difícil de este problema es entender qué nos pregunta realmente. Para cada ciudad que se encuentra en un punto $x_i$ del eje $x$, necesitamos encontrar el punto más cercano y lejano a él.

La clave es que se especifica que nos darán las coordenadas de forma ascendente (sino qué se podría hacer?). Por lo tanto, para un punto $x_i$, basta con revisar si su punto más cercano es el $x_{i-1}$ o el $x_{i+1}$; y si el más lejano es el $x_0$ o el $x_{n-1}$.

Se puede probar que basta con revisar aquellos puntos por contradicción de forma sencilla, y se deja como ejercicio para los interesados ;).

{% highlight c++ %}

int max(int a, int b) {
  return a > b ? a : b;
}

int min(int a, int b) {
  return a < b ? a : b;
}

int main () {
  int points[(int)1e5 + 3];
  int n, i;
  scanf("%d", &n);
  for (int i = 0; i < n; i++) {
    scanf("%d", &points[i]);
  }
  for (int i = 0; i < n; i++) {
    int closest, farthest;
    if (i == 0) { // extremo izquierdo
      closest = points[i+1] - points[i];
      farthest = points[n-1] - points[i];
    }
    else if (i < n - 1) {
      closest = min(points[i] - points[i-1], points[i+1] - points[i]);
      farthest = max(points[i] - points[0], points[n-1] - points[i]);
    }
    else if (i == n - 1) { // extremo derecho
      closest = points[i] - points[i - 1];
      farthest = points[i] - points[0];
    }
    printf("%d %d\n", closest, farthest);
  }
  return 0;
}
{% endhighlight %}