+++
title = "haskell pyramid"
slug = "2018-08-06-haskell-pyramid"
date = 2018-08-06
[taxonomies]
+++

Since my last post I have been climbing what I recently heard 
[Julie Moronuki](https://twitter.com/argumatronic) call the 
[Haskell Pyramid](https://youtu.be/iuwUUlDfHcw?t=47m32s). I have been learning
about the bread and butter abstractions which Haskell programmers use all the time
and serve as useful tools to describe and compose  programs. These are not only 
required knowledge to be able to use most of the really cool
Haskell features that excite me (which I will cover in a later post) but also
useful ways to model computation in general, even if one is not using
Haskell.

I have been learning about SemiGroups, Monoids, Functors, Applicatives, Monads, Foldables, 
Traversables and Reader from the [Haskell Book](https://haskellbook.com/), which
Julie co-authored. I figured that now would be a good time to take stock of what
I have learned. This is meant to be a fly by of these topics, more of a cheatsheet
than a tour.  If you want to learn more about these topics, I recommend checking
out [Haskell Book](https://haskellbook.com/). All the exercises I worked through 
to gain an intuition about these abstractions can be found 
[here](https://github.com/nicksanford/RC_haskell)


SemiGroup, Monoid, Functor, Applicative, and Monad are abstractions,
which have laws, which concrete implementations must conform to in order to be 
a valid instances of that abstraction. While learning about these abstractions, 
I used  QuickCheck, a property 
based testing library, to verify that the instances I wrote conformed
to the appropriate laws.  I found this to be an extremely effective way to learn
new concepts because my test suite was able to serve as a constant feedback loop.


#### SemiGroup
As described on the [Hackage SemiGroup package](https://hackage.haskell.org/package/semigroups), a SemiGroup is an algebraic structure, consisting of a set together with an associative, binary operation (`<>`).
the 
I know this function `<>` is called `mappend` if on Monoid, but I don't know 
what it is called on SemiGroup.

In order for something to be a valid SemiGroup it's `<>` function must be binary
(take two arguments) and associative:
```haskell
a <> (b <> c) == (a <> b) <> c
```

Some things that exhibit SemiGroup behavior include non empty lists, and getting
the first and last elements from a sequence of (`<>`) calls:
```haskell
First 1 <> First 2 <> First 3
=> First 1
```
```haskell
Last 1 <> Last 2 <> Last 3
=> Last 3
```

#### Monoid

Monoids are a SemiGroup that has an identity value (described by `mempty`) such that:
```haskell
mempty <> x == x
x <> mempty == x
```

All Monoids also have the binary associative function `<>` which can be inherited
from their SemiGroup instance, and is called `mappend` (i.e. `<> == mappend` if it is
being applied to a Monoid).

Numbers (both over multiplication and addition) boolean conjunction (`and`) 
disjunction (`or`), (`a && b && c`) and (`a || b || c`) patterns, list concatenation
and zipping all exhibit Monoidal behavior (i.e. they have an identity value and 
a binary associative operation).

[Lucy Zhang](https://github.com/lucy-zhang) also showed me that Groups (which
I came across when researching [Kademlia](https://en.wikipedia.org/wiki/Kademlia),
the [DHT](https://en.wikipedia.org/wiki/Distributed_hash_table) protocol used 
in BitTorrent and IPFS) are Monoids that have an inverse for every value. An 
example of groups would be integers over addition, where zero is it's own 
inverse, and negative numbers and positive numbers are inverses of each other.

Monoids have kind `*`.

#### Functor

Functors have some structure that is not able to be altered, but which can allow 
for transformations on values that are not part of the structure.
List, Maybe, Either, Tuples, and functions are all functors.

They implement `fmap`, which can also be written as `<$>`.

```haskell
fmap :: Functor f => (a -> b) -> f a -> f b
```

and have the following laws which they must obey to preserve
their structure:

##### identity
```haskell
fmap id == id
```

##### composition
```haskell
fmap (f . g) == fmap f . fmap g
```

If you try to change the structure of a functor it will violate the composition 
law, as applying `fmap` multiple times will yield a different result, than just 
applying it once and composing two functions.

Functors must have kind `* -> *` as they must have a type inhabitant which is not
part of the structure, such as the `Right` values in the case of `Either`, or 
list elements in the case of Lists .

Lists, functions, and `IO` are all examples of functors. Function composition, `.`, is `fmap` on functions:

```haskell
(.) :: (b -> c) -> (a -> b) -> (a -> c)
```

where `f` is `(-> a)`.

Something I found profoundly beautiful is when one applies `fmap` to itself (as
in the example below)

```haskell
(fmap . fmap) show [Just 1, Just 3, Nothing] 
=> [Just "1", Just "3", Nothing]
```

the way the types work out requires algebraic substitution of the types:

```haskell
(.) :: (b -> c) -> (a -> b) -> (a -> c)
fmap1 :: Functor f => (a -> b) -> f a -> f b
fmap2 :: Functor g => (c -> d) -> g c -> g d
```

In `fmap . fmap` the type signature expands out to 

```haskell
((a -> b) -> (f a -> f b)) -> ((c -> d) -> (g c -> g d))
```

Because we know that the `b` in compose 
```haskell
(b -> c) -> (a -> b) -> (a -> c)
```
is the same as: 
```haskell
(a -> b)
```

in the first fmap, and
```haskell
(g c -> g d)
```

in the second fmap

we know that `f a` from the first fmap can be rewritten as `f (g c)` and 
`f b` from the second can be rewritten as `f (g d)`,

which means we can rewrite our type signature for `fmap . fmap` as
```haskell
(c -> d) -> (f (g c) -> f (g d))
```
which describes at the type level transforming a value within a functor, within a functor.

It absolutely blew my mind that you could work that out from the types alone without
needing to think about what is happening at the value level at all.

On `Maybe` or `Either`, `Nothing` and `Left` values are ignored by `fmap` because they are
part of the structure, which must be left intact in order to comply with the functor
laws.

Functor (unlike Monoid) only has a single valid instance for a given type.

Functors are very useful for transforming values inside of `IO`. I used
this repeatedly in my QuickCheck testing to transform lists generated by QuickCheck
into other data-structures which I needed for testing.

#### Applicative

As described in the Haskell Book: 

"Applicatives are monoidal functors:

1. Monoids give us a way to mash data-structures together. 
2. Functors give us a way do do function application in a structure.
3. Applicatives give us away to apply functions which are themselves in a structure over another structure and then mash the structures together."

Applicatives have 2 operations:

```haskell
pure :: Applicative f => a -> f a
```
which takes a value of any type and puts it into an applicative context

```haskell
(<*>) :: Applicative f => f (a -> b) -> f a -> f b
```
which applies a function in an applicative structure to values in an applicative 
structure, and combines the two applicative structures.

They must together follow the following laws:

##### Identity
```haskell
pure id <*> v = v
```

##### Composition
```haskell
pure (.) <*> u <*> v <*> w = u <*> (v <*> w)
```
##### Homomorphism
```haskell
pure f <*> pure x = pure (f x)
```

##### Interchange
```haskell
u <*> pure y = pure ($ y) <*> u
```

If we compare, `$` (which is just function application), `<$>` (`fmap`) and `<*>`
```haskell
$   ::(a -> b) ->   a ->   b
<$> ::(a -> b) -> f a -> f b
<*> ::(a -> b) -> f a -> f b
```

we see that `fmap` is jut function application over a functor, and applicative
application is just functorial application, where the function is within a 
structure, which implies that the structures must be combined in some way.

`<*>` lets us do function application on functions in a structure like so:

```haskell
Just (*2) <*> Just 3
=> Just 6

```
```haskell
Nothing <*> Just 3
=> Nothing
```

Wrapping a function in `pure` and applying `<*>` is the same as just fmapping using
the function.
```haskell
pure f <*> y == f <$> y
```

Applicatives have their structure combined using a monoidal pattern, however
because there are multiple possible monoidal patterns on many types, there is no
guarantee the applicative will use any specific Monoid instance for that type. 
Also, there is no typeclass constraint on Applicatives that they must be Monoids.

The function application component will be the same as `fmap` though, as there
is only one valid functorial instance for a given type.

#### Monad

As [Nick Collins](https://github.com/ncollins) explained during our 
Haskell Study group, Monad's bind (`>>=`) is nothing more than flatMap.

Definition:
```haskell
class Applicative m => Monad m where
(>>=) :: m a -> (a -> m b) -> m b
(>>) :: m a -> m b -> m b
return :: a -> m a
```

I also learned about the fish operator:
```haskell
(>=>) Monad m -> (a -> m b) -> (b -> m c) -> a -> m c
```

I don't have much to say about monads other than if you just treat them like
things which support flatMap (whereby you are able to flatten out structures 
generated from calling bind, while adding new resulting values to the new structure) I find that they 
become a lot easier to work with and reason about.

#### Foldable:

Foldable (which is a typeclass which allows your 
data-structure to be folded over), requires only either
`foldr` or  or `foldMap` to be defined. If you define `foldMap`
you need for the type to also have a Monoid instance so you are able to use
`mappend` / `mconcat` to summarize the Monoid structures you are folding over.

By defining a foldable instance for your type, you are also able to get a number
of convenience functions which can be written using folding functions inferred automatically including:
```haskell
foldl' :: (b -> a -> b) -> b -> t a -> b
toList :: t a -> [a]
null :: t a -> Bool
length :: t a -> Int
elem :: Eq a => a -> t a -> Bool 
maximum :: forall a. Ord a => t a -> a 
minimum :: forall a. Ord a => t a -> a 
sum :: Num a => t a -> a 
product :: Num a => t a -> a 
```
While learning about Foldable with [Santiago Gepigon](https://github.com/sgepigon)
during the Haskell Study Group, we also learned about the `ScopedTypeVariables` 
language extension and how it is needed to define type signatures for
polymorphic functions defined in where clauses.

#### Traversable:
As described in the book:

"Traversable allows you to transform elements in a structure (like functor) 
while producing applicative effects along the way, and lift the (potentially 
multiple) applicative effects outside of the traversable structure."

Has 2 functions:
```haskell
sequenceA :: (Traversable t, Applicative f) => t (f a) -> f (t a)
```

```haskell
traverse :: (Traversable t, Applicative f) => (a -> f b) -> t a -> f (t b)
```

Here are some examples of how they work:

```haskell
sequenceA  [Just 1, Just 2, Just 3] 
=> Just [1,2,3]
sequenceA  [Just 1, Just 2, Nothing]
=> Nothing
sequenceA :: (Traversable t, Applicative f)
=> -> t (f a) -> f (t a)
traverse :: (Traversable t, Applicative f)
=> (a -> f a) -> t a -> f (t a)
```

`traverse` must follow the following laws:

##### Naturality
```haskell
t . traverse f = traverse (t . f)
```
##### Identity
```haskell
traverse Identity = Identity
```

##### Composition
```haskell
traverse (Compose . fmap g . f) = Compose . fmap (traverse g) . traverse f
```

`sequenceA` must follow the following laws:

##### Naturality
```haskell
t . sequenceA = sequenceA . fmap t
```
##### Identity
```haskell
sequenceA . fmap Identity = Identity
```

##### Composition
```haskell
sequenceA . fmap Compose = Compose . fmap sequenceA . sequenceA
```

Traverse is really useful when you have a traversable and you need to transform
it into an applicative of a traversable, while converting each element to an 
applicative and applying `<*>` to the intermediate values.

#### Reader

As mentioned above, functions (`(->) r`) are functors and `(.) == fmap`. The `r` is part of the functoral
structure, and therefor the only thing that can change with `fmap` application is
the return value of the function. 

(`Reader r a`) is just a wrapper around  function composition.

The `r` from `Reader` here is part of the `f` structure, so it gets applied with the first argument:
```haskell
pure :: a -> f a
pure :: a  -> (r -> a)

(<*>) :: f (a -> b) -> f a -> f b
(<*>) :: (r -> a -> b) -> (r -> a) -> (r -> b)
```

