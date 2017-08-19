---
title:  "Introducción a los Google OR-Tools con Python"
excerpt: "Breve introducción a la suite de herramientas de optimización de Google."
---
## Google OR-Tools
Las Herramientas de Optimización de Google (OR-Tools) es una suite de software Open Source enfocada en resolver
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

Supongamos que estamos en un rancho y en un corral vemos 56 patas y 20 cabezas.
Entonces, ¿A cuántas vacas y a cuantas gallinas estamos viendo?

Con OR-Tools, lo podríamos resolver de la siguiente manera:

{% highlight python linenos %}
from ortools.constraint_solver import pywrapcp

# Creamos un solucionador de optimización restrictiva y le ponemos Corral
solver = pywrapcp.Solver('Corral')

# Tenemos como mínimo 0 vacas y como máximo tenemos 20
vacas = solver.IntVar(0, 20, 'Vacas')
# Tenemos como mínimo 0 gallinas y como máximo tenemos 20
gallinas = solver.IntVar(0, 20, 'Gallinas')

# Agregamos la restricción: Entre las vacas y gallinas deben ser 56 patas
solver.Add((vacas * 4) + (gallinas * 2) == 56)
# Agregamos la restricción: Entre las vacas y gallinas deben ser 20 cabezas
solver.Add(vacas + gallinas == 20)

# Creamos un constructor de decisiones que dicta como se asignan los diferentes
# valores a las variables
constructor_decisiones = solver.Phase([vacas, gallinas],
                                      solver.CHOOSE_FIRST_UNBOUND,
                                      solver.ASSIGN_MIN_VALUE)
# Solucionamos el problema con nuestro constructor e imprimimos las soluciones
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
  Veo 8 vacas
  Veo 12 gallinas

Aunque este fue un problema algo simple, sirve como ejemplo básico de cómo modelar un problema
pensando en variables y restricciones.
En futuras ocasiones subiremos un poco el nivel de complejidad de los problemas para demostrar
a tope las capacidades de los OR-Tools de Google.

¡Felices Fiestas! 
