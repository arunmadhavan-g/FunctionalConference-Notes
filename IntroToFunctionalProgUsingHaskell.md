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
