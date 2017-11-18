# Introduction To Functional Programming Using Haskell
<p>
  <b> Tony Morris and Alios Cochard </b>
</p>

The session started off with the basics of Haskell.
We quickly went through how to open the GHCI and Load and Re-Load files

<b> To Load </b>
```
 > :load 'FirstProg.hs'
 > :l 'FirstProg.hs'
```
<b> To Re-Load </b>
```
  > :reload
  > :r
```

We Learnt about creating a function and how function can be a value.
<br> Example
 ```
 add a b = a + b
 incrementByOne a  = add 1
 incrementByTwo a = add 2
 ```

### Types

Haskell is a statically typed language with type inference at compile time. A type can be defined using the **data** construct.

```
data Shape  =
 Circle Integer | Square Integer | Rectangle Integer Integer
```

Note the Parameter type each of those Shapes take. like __Circle *Integer*__. The Integer denotes the parameter type it takes while creating a Circle. This can be done like below,

```
> c = Circle 3
> s = Square 4
> r = Rectangle 3 4

```


Though the language is going to do type inference, It was always advised to define it type, as it will remove ambiguity from the program and can lead to 0 defects.

<br>

We can Check the type by using the **type** or just **t** as shown below

```
func1 :: Integer -> Integer
func1 i = i + x

>:t func1
func1 :: Integer -> Integer
>
```

### TypeClasses

We were introduced to the concept of TypeClasses. These are constructs that helps in defining behaviours.
Some of the existing TypeClasses in Haskell that we discussed during the session were

* Eq
* Show
* Ord

A type class is applied to a function to behave as a constraint. This way the behaviour is consistent.
<br>
For example we looked into the type class of == (equals) then we would see the following

```
> :t (==)
(==) :: Eq a => a -> a -> Bool

```
Above snippet shows the **==** is constrained by **Eq**.   

While Defining our own Type, and while trying to display the value of an initialised type, we would not be able to show it in the console. This is because it would use **show** function internally and the type that we defined does not have an implementation on how to handle the same.   

The simplest way to handle the same is to derive the type as shown below.  

```
data Shape  =
 Circle Integer | Square Integer | Rectangle Integer Integer
 deriving (Eq, Show)
```  
In the above snippet, we see that the TypeClasses Eq and Show are derived which means we would be able to compare and display the values as required.  

### Pattern Matching

Haskell provides an amazing way of pattern matching and we were introduced to it by helping us define functions for the types that we just defined.  

Lets for example take the case of calculating Perimeter for the Shapes that we have defined. We would write functions for the same as shown below.  

```
perimeter:: Shape -> Integer
perimeter (Circle r) = r*2*pie
perimeter (Square s) = s*4
perimeter (Rectangle w h) = (w+h)*2

```

As you can see, we have the same function name but with different Types of Shape. What happens is when anyone wants to find the perimeter of a square, it would come from the top, check if any of the implementation is Square, and when it matches the square, it would go into the function and execute it.

```
> perimeter (Square 3)
12
> perimeter (Rectangle 4  3)
14
>
```

Pattern matching is used extensively and is the way to perform loops using recursion. For example to find the sum of first n numbers the code would look like below.

```
sumOfN :: Integer -> Integer
sumOfN 0 = 0
sumOfN n = n + sumOfN (n-1)
```

In the above code, if the value is pattern matched to "0" then it would return 0, else you would recurse until 0 and keep adding the sum.  

This effectively would lead to a condition free implementation with if ... else.  

### Recursive Types

We looked into a very interesting structure while defining types. The representation of Natural Numbers as shown below.

```
data Natural = Zero | Successor Natural
  deriving (Eq, Show)
```

To represent any natural number, one would express them as below.

```
four = Successor(Successor(Successor(Successor(Zero))))
```

You can derive other natural numbers from  as successor of already defined ones. For Example

```
six = Successor(Successor(four))
```

We would define an add function as below

```
add :: Natural -> Natural -> Natural
add Zero  x = x
add (Successor q) x =  Successor (add q x)

> four `add` six
Successor (Successor (Successor (Successor (Successor (Successor (Successor (Successor (Successor (Successor Zero)))))))))

```




### References Suggested
|Topics | Links |
| ---- | ----|
| Github Used For Exercises | https://github.com/data61/fp-course |
| Reference Links in the Bottom on this page | https://qfpl.io/ |
| Convert Long Lambdas to Shorter More Concise Forms | http://pointfree.io/ |
| API Search Engine for Haskell | https://www.haskell.org/hoogle/ |
| TypeClass and Patterns in Haskell | https://wiki.haskell.org/Typeclassopedia |
| What Does Monad Mean - Tony Morris ( Video) | https://vimeo.com/8729673 |
### Suggested Reading
 * Applicative programming with Effects
 * Quick Check - Property Based Testing
 * Theorems for free - Philip
 * Essence of Iterative patterns
 * Applicative functor
 * SICP. - 1960 book
