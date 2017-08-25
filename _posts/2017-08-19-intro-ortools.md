---
title:  "Introducción a los Google OR-Tools con Python"
excerpt: "Breve introducción a la suite de herramientas de optimización de Google OR-Tools."
image:
    feature: "chickens.jpg"
    thumb: "chickens-thumb.jpg"
---
## Google OR-Tools
Las Herramientas de [Optimización de Google (OR-Tools)](https://developers.google.com/optimization/) es una suite de software [Open Source](https://github.com/google/or-tools) enfocada en resolver
problemas de optimización combinatorios. Algunos ejemplos de este tipo de problemas son el problema del viajero
y el problema de la mochila, y ambos problemas pueden ser resueltos utilizando la suite de OR-Tools.
Aunque OR-Tools fue escrito en C++ por ingenieros de Google, este también puede ser usado con Java, C# y Python.

La librería incluye diversos tipos de algoritmos solucionadores, pero en esta ocasión nos enfocaremos en el
solucionador de optimización restrictiva.

## Optimización Restrictiva
La optimización restrictiva, o programación restrictiva, es una técnica que se utiliza para encontrar soluciones
factibles dentro de un conjunto muy grande de soluciones posibles. Se caracteriza por el planteamiento de relaciones
y restricciones entre las variables,y estas deben ser respetadas por la solución para que ésta pueda ser considerada
como factible.

La optimización restrictiva trata de encontrar soluciones factibles más que soluciones óptimas, ya que se enfoca
en las restricciones entre variables en vez de una función objetivo. Incluso, puede que un problema de programación
restrictiva ni siquiera tenga una función objetivo que optimizar, por lo que sólo se busca encontrar un conjunto de
soluciones que cumplan con todas las restricciones.

### El Corral
Para poner un ejemplo de cómo utilizar esta librería, se propone el siguiente problema:

> Supongamos que estamos en un rancho y en un corral vemos 56 patas y 20 cabezas de ganado.
> Entonces, ¿A cuántas vacas y a cuantas gallinas estamos viendo?

Con OR-Tools, lo podríamos resolver de la siguiente manera:

Primero, creamos un solucionador de optimización restrictiva default y le ponemos de nombre `Corral`
{% highlight python %}
from ortools.constraint_solver import pywrapcp

solver = pywrapcp.Solver('Corral')
{% endhighlight %}

Luego, definimos nuestras restricciones:
- Tenemos como mínimo 0 vacas y como máximo tenemos 20.
- De la misma manera, hay como mínimo 0 gallinas y como máximo 20.
Para esto, las definimos como enteros de magnitud variable.
{% highlight python %}
vacas = solver.IntVar(0, 20, 'Vacas')
gallinas = solver.IntVar(0, 20, 'Gallinas')
{% endhighlight %}

Ahora, agregamos las siguientes restricciones y relaciones:
- Entre las vacas y gallinas deben de ser 56 patas en total.
- Además, entre ambos animales deben de sumar 20 cabezas.
{% highlight python %}
# 4 patas por cada vaca, y 2 patas por cada gallina
solver.Add((vacas * 4) + (gallinas * 2) == 56)
# Una cabeza por cada una
solver.Add(vacas + gallinas == 20)
{% endhighlight %}

Lo siguiente es crear un constructor de decisiones que es el que dicta cómo se asignan los diferentes valores a las variables durante la ejecución del solucionador. Para esto creamos un constructor de decisiones de tipo `Phase` con los siguientes parámetros:
- Una lista con las variables de las cuales queremos encontrar su valor
- La forma en la que el solucionador tomará la siguiente variable a la cual asignarle un valor, en este caso elegirá la primera que no tenga valor en el orden en el cual aparecen en la lista previa.
- La manera en la que se eligirá el siguiente valor a asignar a cierta variable, en este caso, será el mínimo valor posible que no se ha intentado aún.
{% highlight python %}
constructor = solver.Phase([vacas, gallinas],
                           solver.CHOOSE_FIRST_UNBOUND,
                           solver.ASSIGN_MIN_VALUE)
{% endhighlight %}

Ahora sólo nos queda solucionar el problema con nuestro constructor e imprimir las soluciones que encuentre.
{% highlight python %}
solver.Solve(constructor_decisiones)
num_solucion = 0
while solver.NextSolution():
    num_solucion += 1
    print(f'Solución {num_solucion}')
    print(f'Veo {vacas.Value()} vacas')
    print(f'Veo {gallinas.Value()} gallinas')
{% endhighlight %}

La solución encontrada por el solucionador es la siguiente:
> Solución 1
>
> Veo 8 vacas
>
> Veo 12 gallinas

Aquí está el programa completo:
{% highlight python %}
from ortools.constraint_solver import pywrapcp

solver = pywrapcp.Solver('Corral')

# Tenemos como mínimo 0 y como máximo 20 de cada animal
vacas = solver.IntVar(0, 20, 'Vacas')
gallinas = solver.IntVar(0, 20, 'Gallinas')
# 4 patas por cada vaca, y 2 patas por cada gallina
solver.Add((vacas * 4) + (gallinas * 2) == 56)
# Una cabeza por cada una
solver.Add(vacas + gallinas == 20)
# Creamos el constructor de decisiones
constructor = solver.Phase([vacas, gallinas],
                           solver.CHOOSE_FIRST_UNBOUND,
                           solver.ASSIGN_MIN_VALUE)
# Imprimimos todas las soluciones encontradas
solver.Solve(constructor_decisiones)
num_solucion = 0
while solver.NextSolution():
    num_solucion += 1
    print(f'Solución {num_solucion}')
    print(f'Veo {vacas.Value()} vacas')
    print(f'Veo {gallinas.Value()} gallinas')
{% endhighlight %}

Aunque este fue un problema algo simple, sirve como ejemplo básico de cómo modelar un problema
pensando en variables y restricciones.
En futuras ocasiones subiremos un poco el nivel de complejidad de los problemas para demostrar
a tope las capacidades de los OR-Tools de Google.

¡Felices Fiestas!
