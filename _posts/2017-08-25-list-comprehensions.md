---
title:  "Exploration of list comprehensions in Python"
excerpt: "A showcase of list comprehensions use cases."
---
## List Comprehensions in Python
Python is recognized for its high versatility and how easy it is to handle all types of data with it.
One of the reasons why it's so good at data manipulation is because it has
a swiss-army knife under its sleeve, list comprehensions.

### A Shopping List
To demonstrate what they are and how they are used, we will start with a simple shopping list.
For reasons beyond this tutorial, we want to convert all items to all caps.
A first approach would be creating a new list and then appending the converted items to it.
{% highlight python %}
shopping_list = ['Eggs',
                 'Milk',
                 'Cookies',
                 'Tomatoes',
                 'Onions',
                 'Apples',
                 'Papayas',
                 ]
shouting_list = []
for item in shopping_list:
    shouting_list.append(item.upper())

print(shouting_list)
{% endhighlight %}
> ['EGGS', 'MILK', 'COOKIES', 'TOMATOES', 'ONIONS', 'APPLES', 'PAPAYAS']

The previous example can be easily understood by anyone with a basic understanding of Python,
and it's a perfectly valid way of doing it. However, we know a `for` loop can be used for a lot of things and purpouses. Nevertheless, in this case we are using it to create a new list based on another one, and for that exact purpouse we have a much better tool. You guessed, this is the reason list comprehensions exist.

Ahora veamos el ejemplo anterior pero esta vez utilizando la comprensión de listas:
Let's revisit the previous example, but using a list comprehension instead.
{% highlight python %}
shouting_list = [item.upper() for item in shoppint_list]
lista_grande = [articulo.upper() for articulo in lista_compras]
print(shouting_list)
{% endhighlight %}
> ['EGGS', 'MILK', 'COOKIES', 'TOMATOES', 'ONIONS', 'APPLES', 'PAPAYAS']

The basic formula for a list comprehension is the following:
`[<new_value> for <value> in <collection>]`, where `new_value` is our modified `value`.
As we can see, the result can be achieved with fewer lines of code and in a more elegant and Pythonic way.

## Calculating prices
In the previous example we are transforming preexisting data and creating a new list with them.
Moreover, we can also generate new data based on other data with list comprehensions.
Imagine the shop we are shopping at assigns prices to items based on the letters of their name.
To obtain the list of prices of the items we have in our shopping list, we would do the following:

{% highlight python %}
def calc_price(item):
    return sum([ord(letter) for letter in item])

prices = [calc_price(item) for item in shopping_list]
print(prices)
{% endhighlight %}
> [390, 397, 717, 844, 630, 613, 719]

## Mapping and filtering
To manipulate collections in Python, there are functions like `map` and `filter` which were used heavily before the implementation of [PEP202](https://www.python.org/dev/peps/pep-0202/) for transformating and filtering data respectively.
Imagine we don't have that much cash at hand so we will only be buying cheap items, thus needing a list with the price of the items that cost less than $650 to see if we can afford those. In our next code same we will compare both methods for doing this.
{% highlight python %}
cheap_items_before = list(filter(lambda price: price < 650,
                                 map(calc_price, shopping_list)))
print(cheap_items_before)

cheap_items_now = [calc_price(item) for item in shopping_list
                   if calc_price(item) < 650]
print(cheap_items_now)
{% endhighlight %}
> [390, 397, 630, 613]

## Other Comprehensions
You can also use list comprehensions to generate a list that contains the items paired
with their price using the builtin function [zip](https://docs.python.org/3/library/functions.html#zip):
{% highlight python %}
items_with_prices = [info for info in zip(shopping_list, prices)]
print(shopping_list)
{% endhighlight %}
> [('Eggs', 390), ('Milk', 397), ('Cookies', 717), ('Tomatoes', 844), ('Onions', 630), ('Apples', 613), ('Papayas', 719)]

But maybe a better way of representing those pair of values would be using a dictionary.
Fortunately, Python provides us with dict comprehensions, which also work in a similar way.
(et comprehensions also exist, by the way)
The regular form for dict comprehensions is `{key: value for key,value in collection}
{% highlight python %}
item_prices = {item:price for item, price in zip(items, prices)}
print(item_prices)
{% endhighlight %}
> {'Cookies': 717, 'Eggs': 390, 'Papayas': 719, 'Apples': 613, 'Onions': 630, 'Milk': 397, 'Tomatoes': 844}

It's worth noting that tuple comprehension **does not exist** in Python.
{% highlight python %}
item_tuple = (item for item in shopping_list)
print(item_tuple)
{% endhighlight %}
> <generator object <genexpr> at 0x7f....>

What we get instead is a *generator expression*, and its explanation deserves a blogpost on its own.

Happy Days!
