---
layout: post
title:  "Soluciones Primera Sesión"
date: 2017-02-08 23:24:03 -0600
author: csssaz
categories: solucion editorial código
---

Los resultados del primer concurso de introducción fueron los siguientes:

![Scoreboard]({{site.url}}/assets/scoreboard1.png)

¡Felicidades a los que intentaron subir algún problema! 


En este post explicaremos brevemente la solución de los 3 problemas de la primer sesión.

### [1A](http://codeforces.com/problemset/problem/1/A) Theatre Square

La mayor dificultad de este problema es lidiar con los tipos de datos adecuados. Es importante tomar en cuenta
que un entero de 32 bits (int) puede almacenar un número tan grande como ~10^9. Por lo tanto, como en este problema tenemos que multiplicar dos de esos números (analizar qué pasa si a = 1), nuestro resultado sería a lo más 10^18. Por este motivo, es necesario utilizar el tipo long long que es un entero de 64 bits. Los errores ocasionados por overflow son clásicos y siempre hay que pensar en qué rangos de valores pueden estar nuestros cálculos.

{% highlight c++ %}
int main () {
  long long n, m, a;
  scanf("%lld %lld %lld", &n, &m, &a);
  long long ans = (long long) (ceil((double)n/a) * ceil((double)m/a));
  printf("%lld\n", ans);
  return 0;
}
{% endhighlight %}

### [4A](http://codeforces.com/problemset/problem/4/A) Watermelon

En matemáticas, se dice que un número par tiene *paridad par*, y un número impar, *paridad impar*. 

A partir de esta definición sencilla, es fácil notar dos observaciones importantes:

- La suma (o resta) de dos números con la misma paridad, resulta en un número con paridad par (par + par o impar + impar).
- La suma (o resta) de dos números con paridad distinta, resulta en un número con paridad impar (par + impar).

De esta forma, si el peso de la sandía es impar, significa que es resultado de la suma de dos números impares, y por lo tanto, no podemos dividirla en dos pedazos con peso par. 

Notar que el caso w = 2 es especial, puesto que en este caso no tiene solución, ya que si puede ser expresado como la suma de dos números impares, pero alguno de estos tendría que ser negativo (o cero!).

{% highlight c++ %}
int main () {
  int w; 
  scanf("%d", &w);
  if (w % 2 != 0 || w == 2) puts("NO");
  else puts("YES");
  return 0;
}
{% endhighlight %}

### [118A](http://codeforces.com/problemset/problem/118/A) String Task
Este es uno de esos problemas donde tienes que hacer exactamente lo que dice :). Este tipo de problemas son buenos para practicar habilidades de implementación.

{% highlight c++ %}
int is_consonant(char c) {
  c = tolower(c);
  return c != 'a' && c != 'e' && c != 'i' 
        && c != 'o' && c != 'u' && c != 'y';
}

int main () {
  char s[105];
  char ans[205]; // si todas son consonantes, tenemos a lo mas 200 caracteres
  scanf("%s", s);
  int n = strlen(s);
  int idx = 0;
  for (int i = 0; i < n; i++) {
    char c = s[i];
    if (is_consonant(c)) {
      ans[idx] = '.';
      idx++;
      ans[idx] = tolower(c);
      idx++;
    }
  }
  ans[idx] = '\0'; // no olvidar caracter nulo!
  puts(ans);
  return 0;
}
{% endhighlight %}