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

We can create the Shape of our choice by specifying the type as shown below

```
s = Square 4
r = Rectangle 8

```

### Generic Types

It is possible that the behaviour of a function can be applied across many data types. For example lets take the case of **==** that we saw earlier, it had a type  **_Eq a => a -> a -> Bool_**. You many notice that the function takes in types "a" and returns back a "Bool"

Here "a" denotes a generic type.


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

### Optional

We started looking into the Exercises that we checked out from [github](https://github.com/data61/fp-course) and the first exercise that we looked into was Optionals defined inside _Optional.hs_ file

Optionals are structures that helps us deal with data that can be empty or have a value. Effectively in comparing it with Java paradigm, it helps us prevent any null pointer exceptions, helping us with a very safe handling of data.

I'll try to document all the various steps we took while trying to solve the exerised in the Optionals.hs


We had a pre-defined Type for optional which was defined as below.

```
data Optional a =
  Full a
  | Empty
  deriving (Eq, Show)
```

This indicates that we could either have a Full object, which is an object with a value or an Empty one. Notice the generic types the Optional takes, which just helps us wrap all the types with an optionals.

#### Exercise 1 : implement mapOptional

This is a function that is used to transform the value inside the context of Optional which takes a function type as shown below.

```
mapOptional :: (a -> b) -> Optional a -> Optional b
--             Function | transform from | transform to
```

To implement the same,  we use pattern matching. In this case,

1. For any function, if the Optional a is _Empty_ , then Optional b would always be _Empty_
2. For every other case, we apply the function on a to transform it to b, wrap it again in an Optional

The implementation is as follows.

```
mapOptional _  Empty = Empty
mapOptional f (Full a) =  Full (f a)
```

This can also be implemented using case as shown below


```
mapOptional f = \v -> case v of
                        Empty -> Empty
                        Full a -> Full (f a)
```


#### Exercise 2: Implement bindOptional

Bind is similar to Map, but the function transformer returns an Optional too.
The function type is defined as shown below.

```
bindOptional :: (a -> Optional b) -> Optional a -> Optional b
```

The implementation goes as below,  ( we just apply the function and return its result.)

```
bindOptional _ Empty= Empty
bindOptional f (Full a) = f a
```

#### Exercise 3: Implement  (??)

This brings us to an interesting piece. When the given function name is a non alphabet, it will be considered by default as an infix operation.  In this example we implement the **??** that helps in returning a value from 2 inputs depeneding on whether or not the first one exists.

The function type goes as below.

```
(??) :: Optional a -> a -> a
```

Implementation would be as below.

```
(??) Empty b = b
(??) (Full a) _ = a
```
#### Exercise 4: Implement (<+>)

Similar to the previous exercise, we return back an optional for Optional inputs, the type is as shown below.

```
(<+>) ::Optional a -> Optional a -> Optional a
```

The implementation is similar to the previous one as shown below

```
(<+>) Empty v = v
(<+>) v _ = v
```


### List

Haskell has an inbuilt list, but we worked on building our own list data structure.  Essentially its a recursive data structure which can be Empty ( in this case we define it as Nil), or it could have some value recursively.

To give a better view of a list
* An Empty list would be having an element [Nil], which denotes the end of the list too.
* A single element would look like [v1, Nil]
* More elements added would mean we will have then as [v1, v2, Nil]

As we grow we keep adding more and more elements to the top and for our case we use an operator __:.__

So in short, the list would be formed as below.

_v1 :. v2 :. v3 :. Nil_

The type to define a list would be defined as below.

```
data List t =
  Nil
  | t :. List t
  deriving (Eq, Ord)
```

Notice the recursive structure that says t :. List t.

#### Head and Tail of the list

In a list head denotes the 1st element and Tail the rest of it.
For E.g. in a list [1 :. 2 :. 3:. 4:. 5 :. Nil]
head would be **1** and tail would be **[2 :. 3:. 4:. 5 :. Nil]**

#### Exercise 1 : headOr

An headOr function helps in returning the head of the list. In case the list if empty, it would return an alternate value.

The function is defined as below.

```
headOr :: a -> List a -> a
```

The implementation for the same would look like below.

```
headOr a Nil = a
headOr _ (h :. _) = h
```
Notice the beauty of pattern matching in the second line. When the list has some value, we completely disregard the first input and then represent the 2nd input as a pattern that consists of "h" which is the head, the :. and the rest of the list which as mentioned before is the tail.

In this case, we also do not need the tail and its ignored.

#### Exercise 2: product

We consider a list of Integers and the result of the function would give the product of all the numbers in the list.
Notice that we need to define it only for Integers. the Function definition would be as shown below.

```
product :: List Int -> Int
```

The implementation would involve multiplying all the individual numbers  as shown below.

```
product Nil = 1
product (h :. t) =  h * product t
```

Notice that we are doing a recursion of the product function, where we call it repeatedly until the list becomes empty and fulfils the Nil condition flow.

#### Exercise 2 : product using foldLeft

We were introduced to the foldLeft function that helps in running through the function from the left to right and apply a function on each of the members.  Below implementation is an alternate implementation of the same using foldLeft

```
product l = foldLeft (\a b -> a*b) 1 l
```
This above call passes a function that products the 2 numbers, a starting aggregator and finally the list

This function can be simplified as show below.
```
product = foldLeft (* ) 1
```
Lets see how.

1.  the product could be defined as a lambda expression
    product = \l -> foldLeft (\a b -> a*b) 1 l
2. This can further be simplified by removing the l from each of the lambda and removing the -> as shown below  
    product = foldLeft (\a b -> a*b) 1
3. The lambda \a b -> a * b can be further simplified and expressed as (* ) which finally makes the expression
    product = foldLeft (* ) 1

#### Exercise 3: sum
Similar to product, this function returns sum.

The definition and implementation would look like below.

```
sum :: List Int -> Int
sum = foldLeft (+) 0
```    

#### Exercise 4: length

The length would give the length of the list and its type is

```
length :: List a -> Int
```
Lets first do a recursion based implementation

```
length Nil= 0
length (_ :. t) = 1 + length t
```
A foldLeft implementation would be

```
length = foldLeft (\a _-> a+1 ) 0
```
You may notice that we only care about incrementing the accumulator.  This can further be reduced as shown below.

```
length = foldLeft (const . (1 +)) 0
```

const is a function that taken in 2 inputs and always return the 1st one.  defined by the type

```
const :: a -> b -> a
```

#### Exercise 5: map
Similar to the mapOptional in Optional, the map function takes in a function and applied it on all the individual members of the list.  

```
map :: (a -> b) : List a : List b
```

Lets do it recursively first.

```
map _ Nil= Nil
map f (h :. t) = (f h) :.  (map f t)
```
This can be implemented using a function called foldRight.
The foldRight f p list.

* from the list it replaced all the :. with f
* it replaces the Nil at the end of the list with p

The final outcome would look like this.

```
map f = foldRight ((:.) . f ) Nil

```

#### Exercise 6: Filter
 Filter would help you filter based on a condition and goes by the type
 ```
 filter :: (a -> Bool) -> List a -> List a
 ```
Lets do the recursion version.

```
filter _ Nil = Nil
filter p (h :. t)= case (p h) of
                    True -> h :. (filter p t)
                    False -> (filter p t)

```

We can use foldRight here too and the function implementation would be as shown below.
<TODO the fold Right version as we do not have the understanding of how its done>


#### Exercise 7: Implementing (++)

Its a simple implementation using foldRight function. If you remember the definition of foldRight,
we replace the (:.) with a function f and the Nil with the value passed.

Thus the definition and implementation would look like below,

```
(++) :: List a -> List a -> List a
(++) a b = foldRight (:.) b a
```

#### Exercise 8: flatten

Flatten takes a nested list and flattens it into a single list.
Applying the same principle, we get the implementation to be as shown below.

```
flatten :: List (List a) -> List a
flatten = foldRight (++) Nil
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
