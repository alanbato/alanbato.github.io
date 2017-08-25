---
title:  "Comprendiendo la Comprensión de Listas en Python"
excerpt: "."
---
## Comprensión de Listas en Python
Python es reconocido por su alta versatilidad y su facilidad para manejar datos
de todo tipo. Una de las razones por las cuales es tan bueno manipulando información
es porque tiene una poderosa navaja multiusos bajo la manga, la comprensión de listas.

### La lista de compras
Para demostrar qué son y para que se usan, comenzaremos con un ejemplo en el cual
tenemos una lista de compras y queremos reescribir cada artículo en mayúsculas.
{% highlight python %}
lista_compras = ['Huevos',
                 'Leche',
                 'Galletas',
                 'Tomate',
                 'Cebolla',
                 'Manzana',
                 'Papaya',
                 ]
lista_grande = []
for articulo in lista_compras:
    lista_grande.append(articulo.upper())

print(lista_grande)
{% endhighlight %}
> ['HUEVOS', 'LECHE', 'GALLETAS', 'TOMATE', 'CEBOLLA', 'MANZANA', 'PAPAYA']

El ejemplo anterior lo debería poder entender cualquiera que tenga al menos un
conocimiento básico de Python, y es perfectamente válido. Sin embargo, un ciclo
`for` puede ser utilizado para una infinidad de cosas. Sin embargo, aquí la estamos
utilizando para crear una lista nueva en base a otra, pero para esto existe algo
mucho mejor: La comprensión de listas, que su único propósito es crear una nueva lista.

Ahora veamos el ejemplo anterior pero esta vez utilizando la comprensión de listas:
{% highlight python %}
lista_grande = [articulo.upper() for articulo in lista_compras]
print(list_grande)
{% endhighlight %}
> ['HUEVOS', 'LECHE', 'GALLETAS', 'TOMATE', 'CEBOLLA', 'MANZANA', 'PAPAYA']

Como se puede ver, obtenemos el mismo resultado con menos líneas de código y de
una manera más elegante y pythónica.

## Generando precios
En en ejemplo anterior estamos modificando datos ya existentes y creando una
nueva lista con ellos. Por otro lado, también podemos generar datos nuevos a
partir de otros con comprensión de listas. A continuación veremos como calcular
el precio de los artículos en nuestra lista consultando una función que calcula
el costo en base al artículo que se proporciona como parámetro.
{% highlight python %}
def cotiza(articulo):
    return sum(ord(letra) for letra in articulo)

precios = [cotiza(articulo) for articulo in lista_compras]
print(precios)
{% endhighlight %}
> [634, 481, 813, 618, 690, 710, 604]

También podríamos utilizar comprensión de listas para generar una que contenga
los pares de cada producto con su precio utilizando la función [zip](https://docs.python.org/3/library/functions.html#zip):
{% highlight python %}
lista_con_precios = [par for par in zip(lista_compras, precios)]
print(lista_con_precios)
{% endhighlight %}
> [('Huevos', 634), ('Leche', 481), ('Galletas', 813), ('Tomate', 618),
   ('Cebolla', 690), ('Manzana', 710), ('Papaya', 604)]

## Manipulación y filtrado
Para manipular collecciones en Python, existen funciones como `map` y `filter`
que se utilizaban antes de la implementación del [PEP202](https://www.python.org/dev/peps/pep-0202/)
para la alteración y filtrado de datos respectivamente. En el siguiente ejemplo
compararemos ambas maneras de implementar la búsqueda de los precios de los
artículo más baratos utilizando únicamente la lista de compras original.
{% highlight python %}
lista_baratos_antes = list(filter(lambda costo: costo < 650,
                                  map(cotiza, lista_compras)))
print(lista_baratos_antes)

lista_baratos_ahora = [cotiza(articulo) for articulo in lista_compras
                       if cotiza(articulo) < 650]
print(lista_baratos_ahora)
{% endhighlight %}
> [634, 481, 618, 604]

Tal vez una mejor manera de representar los pares de valores sería utilizando
diccionario, que es la colección en Python de tipo *llave->valor*. Afortunadamente,
así como existen las comprensiones de listas también existen las comprensiones de
diccionarios (y sus primos los sets).
{% highlight python %}
dict_precios = {articulo:costo for articulo, costo
                in zip(lista_compras, precios)}
print(dict_precios)
{% endhighlight %}
> {'Papaya': 604, 'Galletas': 813, 'Huevos': 634, 'Tomate': 618,
   'Manzana': 710, 'Cebolla': 690, 'Leche': 481}

Pero cabe resaltar que la comprensión de tuplas **no existe** en Python.
{% highlight python %}
tupla_compras = (articulo for articulo in lista_compras)
print(tupla_compras)
{% endhighlight %}
> <generator object <genexpr> at 0x7f....>

Lo que obtenemos es un *generator expression*, y su explicación merece un
futuro artículo propio.

¡Felices Fiestas!


{% highlight python %}

{% endhighlight %}
