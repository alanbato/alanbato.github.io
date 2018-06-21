---
title:  "SF Python OR-Tools Workshop"
excerpt: "I gave a workshop on OR-Tools and here is all the info"
---
Yesterday (20/Jun/18) I gave a workshop at the SF Python Hack Night at [Holberton School](https://www.holbertonschool.com/) about the [Google OR-Tools](https://developers.google.com/optimization/) library. I'm posting the slides on [SpeakerDeck](https://speakerdeck.com/alanbato/constraint-programming-with-python) and as [Direct download]({{ "/assets/files/ortools_workshop.pdf | absolute_url }}) along with the code solutions here.

## Solution 1.1 The Farm
{% highlight python %}
from ortools.constraint_solver import pywrapcp

solver = pywrapcp.Solver('Farm')

# Since we see 20 heads,
# we have at least 0 and no more than 20 of each animal.
cows = solver.IntVar(0, 20, 'Cows')
chickens = solver.IntVar(0, 20, 'Chickens')
# Each cow has 4 legs, and each chicken has 2 legs
solver.Add((cows * 4) + (chickens * 2) == 56)
# Each animal has only one head
solver.Add(cows + chickens == 20)
# We create the decision builder
farm_phase = solver.Phase([cows, chickens],
                           solver.CHOOSE_FIRST_UNBOUND,
                           solver.ASSIGN_MIN_VALUE)
# Solve the problem!
solver.Solve(farm_phase)
# Print the solution
solver.NextSolution():
print(f'I see {cows.Value()} cows and {chickens.Value()} chickens')
{% endhighlight %}

## Solution 2 Sudoku
{% highlight python %}
from ortools.constraint_solver import pywrapcp

solver = pywrapcp.Solver('Sudoku')
# Create the board of 9x9 cells
sudoku_board = [
    [solver.IntVar(1, 9, 'Cell') for i in range(9)] for j in range(9)
]

# Each row must have unique numbers from 1 to 9
for row in sudoku_board:
    solver.Add(solver.AllDifferent(row))
# Each column should have unique numbers too
for col in zip(*matrix):
    solver.Add(solver.AllDifferent(list(col)))

# We define a helper function to constrain a sudoku quadrant (3x3)
def contrain_quadrant(row, col):
    quadrant = []
    for i in range(3):
        for j in range(3):
            quadrant.append(sudoku_board[row+i][col+j])
    solver.Add(solver.AllDifferent(quadrant))

# We loop through the 9 quadrant's top left corners
# and place constraints
for i in range(0, 9, 3):
    for j in range(0, 9, 3):
        constrain_quadrant(i, j)

# To build the phase, we need a 1D list of IntVars
from itertools import chain
cells = chain.from_iterable(sudoku_board)
sudoku_phase = solver.Phase(cells, solver.CHOOSE_FIRST_UNBOUND, solver.ASSIGN_RANDOM_VALUE)
# Solve the problem!
solver.Solve(sudoku_phase)
# Now we can get all the Sudoku solutions we want...
solver.NextSolution()
for row in sudoku_board:
    for cell in row:
        print(cell.Value(), end=' ')
    print('\n')
# Subsequent calls to NextSolution() will generate new solutions...ad infinitum!
# (Not really infinite, just a lot of them)
{% endhighlight %}

And the solution to the third problem deserves a blog post on its own, so you'll have to wait
for that one. But with the code presented here and some thinking you can solve it on your own!

Happy days!