---
title:  "Introduction to the Google OR-Tools in Python"
excerpt: "A short introduction to the OR-Tools optimization suite"
---
## Google OR-Tools
The [Google Optimization Tools (OR-Tools)](https://developers.google.com/optimization/) is an [Open Source](https://github.com/google/or-tools) software suite focused on solving combinatorial optimization problems. Some examples of this type of problem are the traveling salesman problem and the knapsack problem, and both problems can be solved using the OR-Tools suite.
Although OR-Tools was written in C++ by Google engineers, it can also be used with wrappers for Java, C# and Python.

The library includes several types of solving algorithms, but this time we will focus on the
constraint optimization solver.

## Constraint Optimization
Constraint optimization, or constraint programming (CP), is a technique used to find feasible solutions within a very large set of possible solutions. It is characterized by the establishment of relationships
and constraints between the variables. These constraints must be respected by the solution so that it can be considered feasible.

Constraint optimization tries to find feasible solutions rather than optimal solutions, since it focuses
in the constraints between variables instead of an objective function. Moreover, a constraint programming problem does not even need an objective function to optimize, in which case we only look for the set of solutions that meet all constraints.

### The Farm
To illustrate the usage of the Python module, the following problem is proposed:

> Suppose we are in a farm and in a pen we see 56 legs and 20 heads of cows and chickens.
> Then, how many cows and how many chickens are we seeing?

You can install OR-Tools with `pip install ortools` for python2 and `pip install py3-ortools` for the python3 version

Using OR-Tools, we could solve the problem in the following way:

First, we import the module and create a default CP solver named `Farm`.
{% highlight python %}
from ortools.constraint_solver import pywrapcp

solver = pywrapcp.Solver('Farm')
{% endhighlight %}

Then, we start defining our constraints based on the problem description:
- Since we see 20 heads, we have at least 0 cows and no more than 20.
- Likewise, we have at least 0 chickens and no more than 20.

With this information, we can define the number of cows and chickens as Integers which current value we don't know yet, but we can put bounds on the values it can take (also known as domain).
{% highlight python %}
cows = solver.IntVar(0, 20, 'Cows')
chickens = solver.IntVar(0, 20, 'Chickens')
{% endhighlight %}

Ahora, agregamos las siguientes restricciones y relaciones:
Now, we add the following constraints and relationships.
- The total amount of legs of both cows and chickens must be 56.
- The total amount of animals must be equal to 20.
{% highlight python %}
# Each cow has 4 legs, and each chicken has 2 legs
solver.Add((cows * 4) + (chickens * 2) == 56)
# Each animal has only one head
solver.Add(cows + chickens == 20)
{% endhighlight %}

Next step is creating the decision builder, the part of the solution that specifies how the different values will be assigned to the `IntVars` during the solution search. To do this, we use the `Phase` decision builder with the following parameters:
- A list with the variables we want a value to be found and recorded.
- The way we want the solver to choose the next variable to assign. In this case, it will choose the next one with no value assigned following the order in the list of variables.
- The way in which the next value to assign to a certain variable will be chosen. Here, we'll use the minimum value available that has not been used yet for that variable.
{% highlight python %}
decision_builder = solver.Phase([cows, chickens],
                                 solver.CHOOSE_FIRST_UNBOUND,
                                 solver.ASSIGN_MIN_VALUE)
{% endhighlight %}

Now what's left to do is to actually solve the problem and print out the solutions found.
{% highlight python %}
# Solve the problem!
solver.Solve(decision_builder)
# Print the solutions
num_solutions = 0
while solver.NextSolution():
    num_solutions += 1
    print(f'Solution {num_solution}')
    print(f'I see {cows.Value()} cows and {chickens.Value()} chickens')
{% endhighlight %}

This is the solution found by the solver:
> Solution 1
>
> I see 8 cows and 12 chickens

Here is the full program:
{% highlight python %}
from ortools.constraint_solver import pywrapcp

solver = pywrapcp.Solver('Farm')

# Since we see 20 heads,
# we have at least 0 no more than 20 of each animal.
cows = solver.IntVar(0, 20, 'Cows')
chickens = solver.IntVar(0, 20, 'Chickens')
# Each cow has 4 legs, and each chicken has 2 legs
solver.Add((cows * 4) + (chickens * 2) == 56)
# Each animal has only one head
solver.Add(cows + chickens == 20)
# We create the decision builder
decision_builder = solver.Phase([cows, chickens],
                                 solver.CHOOSE_FIRST_UNBOUND,
                                 solver.ASSIGN_MIN_VALUE)
# Solve the problem!
solver.Solve(decision_builder)
# Print the solutions
num_solutions = 0
while solver.NextSolution():
    num_solutions += 1
    print(f'Solution {num_solution}')
    print(f'I see {cows.Value()} cows and {chickens.Value()} chickens')
{% endhighlight %}

Even though this was a simple riddle, it serves as a basic example of how one can model a problem thinking about variables and their constraints.
In future posts we will gradually level up the complexity to fully show the capacties of the Google OR-Tools.

Happy Days!
