# C1. Introduction

El autor dice: 

> "Haskell is elegant and concise. Because it uses a lot of high level concepts, Haskell programs are usually shorter than their imperative equivalents. And shorter programs are easier to maintain than longer ones and have less bugs."

Los creadores de Java al principio consideraron que ser MUY conciso podría ser un problema de comprensión al leer el programa. Además, obligan a nombrar cada indirección y cada "dato" (por eso no tienen tuplas). Así que esta afirmación es "discutible".

# C2. Starting out

* Tiene inferencia de tipos (no hay que declarar los tipos en las definiciones, parámetros o tipos de vuelta)
* El operador de "desigualdad" es `\=`, no `!=`
* Con los Strings el operador `==` es equivalente a equals. En Haskell preguntar por la "identidad" de un objeto no tiene sentido.
* No hay conversión implícita de tipos, así que no se puede concatener un número y un string con el operador +.
* Es un error intentar comparar dos valores de distinto tipo
* La llamada a una función es de la forma `func_name param1 param2`, no se ponen paréntesis
* La llamada a una función se llama "function application"
* En el libro se define la "if statement", cuando es claramente el "Elvis Operator" ?: de Java. Es decir, cuando se usa el "if" se está definiendo una expresión, no una sentencia.
* Las funciones no pueden empezar con mayúsculas
* Una función sin parámetros se denomina "definición" o "nombre" (porque siempre devuelve el mismo valor)

### Listas 

* Las listas son como los arrays de Java y se declaran de forma literal con su misma sintaxis, `[1,3,4]`)
* Se parecen a los streams porque son perezosos (pueden no estar completos en memoria)
* Sus valores tienen que ser del mismo tipo
* El operador `++` concatena listas

`[2]++[3,4,5] == [2,3,4,5]`

* Un string es una lista de caracteres, por eso el operador `++` concatena strings.

`['H','o'] ++ [ 'l', 'a'] == "Hola"`

* El operador `:` añade un elemento a una lista 

`2:[3,4,5] == [2,3,4,5]`

* El operador `!!` es como el método `get(...)`

`"Steve Buscemi" !! 6 == 'B'`

* Obtener el primer elemento: `head`

`head [5,4,3,2,1] == 5`

* Obtener el último elemento: `last`

`last [5,4,3,2,1] == 1`

* Obtener la lista quitando el primer elemento: `tail` (cola)

`tail [5,4,3,2,1] == [4,3,2,1]`

* Obtener la lista quitando el último elemento: `init`

`init [5,4,3,2,1] == [5,4,3,2]`

* Obtener el tamaño de la lista: `length`

`length [5,4,3,2,1] == 5`

* Saber si la lista está vacía: `null`

`null [5,4,3,2,1] == False`

* Saber si el elemento está contenido en la lista (contains): `elem`

``4 `elem` [3,4,5,6] == True``

* Obtener los N primeros elementos de la lista:

`take 3 [1,2,3,4,5] == [1,2,3]`

* Quitar los N primeros elementos de la lista:

`drop 3 [1,2,3,4,5] == [4,5]`

### Tuplas

Son como clases o registros pero cuyos atributos no tienen nombre. 

`("Christopher", "Walken", 55)`

`fst (8,11) == 8`

`fst ("Wow", False) == "Wow"`

`snd (8,11) == 11`

```
ghci> zip [1 .. 5] ["one", "two", "three", "four", "five"]
[(1,"one"),(2,"two"),(3,"three"),(4,"four"),(5,"five")]
```

# C3. Types and Typeclasses

Los nombres de los tipos empiezan en mayúsculas.

Tipos primitivos:
* Int, Integer
* Float, Double
* Bool (True, False)
* Char

Tipos no primitivos
* String ([Char])
* [Int]
* (Char, String)

Tipos de funciones

```
addThree :: Int -> Int -> Int -> Int
addThree x y z = x + y + z
```

Ojo que la función tiene tres parámetros Int y devuelve Int, pero no hay ningún símbolo que los diferencie.

### Type variables

Las variables de tipos empiezan en minúsculas (y por convención suelen ser `a`,`b`,`c`...)

A las funciones que tienen una variable de tipo se las denomina "polimórficas", que equivalen a los "generics" de Java.

```
ghci> :t fst
fst :: (a, b) -> a
```

### Typeclasses

Como en Java existe polimorfismo, se puede definir un parámetro con el tipo de una superclase (o interfaz) y los valores usados en las llamadas pueden ser instancias de objetos de cualquier clase hija (gracias al polimorfismo).

En Haskell esa capacidad se obtiene con las "type varibles" (equivalente a los Java generics). Pero para limitar qué tipos se admiten en esas "type variables" se usan las `type classes`, que actúan como las Wildcards de los parámetros genéricos de Java (por ejemplo `List<? extends Foo>`).

Por ejemplo, el tipo de la funcion `elem` que busca un elemento en un lista. El tipo debe ser "comparable por igualdad", es decir, pertenecer a la typeclass `Eq`.
```
(Eq a) => a -> [a] -> Bool
```

Ojo a la sintaxis, las restricciones de typeclass se incluyen al principio del tipo, entre paréntesis y separadas con `=>` de los parámetros y el tipo de retorno (que se separan con `->` entre ellos). 

Algunas Typeclasses del propio lenguaje:

* `Eq`: Comparable por igualdad
* `Ord`: Comparable por orden
* `Show`: Pintable (representable como String)
* `Read`: Parseable (desde un String)
* `Enum`: Se pueden enumerar (usar en list ranges)
* `Bounded`: Tiene valor máximo y mínimo
* `Num`: Actúa como números (Int, Integer, Float, Double)


### Read y las etiquetas de tipo

Haskell tiene inferencia de tipos "contextual". Similar a como Java se basa en el "target functional interface" para definir el tipo de una expresión lambda.

Pero ese tipo se puede forzar cuando es ambiguo:

```
ghci> read "4"
<interactive>:1:0:
    Ambiguous type variable `a' in the constraint:
      `Read a' arising from a use of `read' at <interactive>:1:0-7
    Probable fix: add a type signature that fixes these type variable(s)
```

```
ghci> read "5" :: Int
5
ghci> read "5" :: Float
5.0
```



