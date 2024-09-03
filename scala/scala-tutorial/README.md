# Scala Tutorial

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/scala-tutorial.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

- **[Basic Data Types (10 points)](#basic-data-types-10-points)**
  - [(Problem #1) `isEvenPair` (5 points)](#problem-1-isevenpair-5-points)
  - [(Problem #2) `validString` (5 points)](#problem-2-validstring-5-points)
- **[Functions (15 points)](#functions-15-points)**
  - [(Problem #3) `factorial` (5 points)](#problem-3-factorial-5-points)
  - [(Problem #4) `magic` (5 points)](#problem-4-magic-5-points)
  - [(Problem #5) `applyK` (5 points)](#problem-5-applyk-5-points)
- **[Collections (25 points)](#collections-25-points)**
  - [(Problem #6) `productPos` (5 points)](#problem-6-productpos-5-points)
  - [(Problem #7) `merge` (5 points)](#problem-7-merge-5-points)
  - [(Problem #8) `generate` (5 points)](#problem-8-generate-5-points)
  - [(Problem #9) `incKey` (5 points)](#problem-9-inckey-5-points)
  - [(Problem #10) `validSums` (5 points)](#problem-10-validsums-5-points)
- **[Trees (25 points)](#trees-25-points)**
  - [(Problem #11) `count` (5 points)](#problem-11-count-5-points)
  - [(Problem #12) `heightOf` (5 points)](#problem-12-heightof-5-points)
  - [(Problem #13) `min` (5 points)](#problem-13-min-5-points)
  - [(Problem #14) `sumLeaves` (5 points)](#problem-14-sumleaves-5-points)
  - [(Problem #15) `inorder` (5 points)](#problem-15-inorder-5-points)
- **[Boolean Expressions (25 points)](#boolean-expressions-25-points)**
  - [(Problem #16) `isLiteral` (5 points)](#problem-16-isliteral-5-points)
  - [(Problem #17) `countImply` (5 points)](#problem-17-countimply-5-points)
  - [(Problem #18) `literals` (5 points)](#problem-18-literals-5-points)
  - [(Problem #19) `getString` (5 points)](#problem-19-getstring-5-points)
  - [(Problem #20) `eval` (5 points)](#problem-20-eval-5-points)


## Basic Data Types (10 points)

#### (Problem #1) `isEvenPair` (5 points)

```scala
def isEvenPair(x: Int, y: Int): Boolean = ???
```

It takes two integers `x` and `y` between `-1000` and `1000` (inclusive) and
returns whether the sum of `x` and `y` is even.

```scala
test(isEvenPair(0, 2), true)    // 0 + 2 = 2 is even
test(isEvenPair(2, 3), false)   // 2 + 3 = 5 is odd
test(isEvenPair(-3, 5), true)   // -3 + 5 = 2 is even
test(isEvenPair(-4, -2), true)  // -4 + -2 = -6 is even
test(isEvenPair(1, -2), false)  // 1 + -2 = -1 is odd
```

#### (Problem #2) `validString` (5 points)

```scala
def validString(str: String, lower: Int, upper: Int): Boolean = ???
```

It takes a string `str` and two integers `lower` and `upper` and returns whether
the length of `str` is between `lower` and `upper` (inclusive).
```scala
test(validString("Hello", 2, 5), true)     // 5 is between 2 and 5
test(validString("COSE212", 3, 4), false)  // 7 is not between 3 and 4
test(validString("Scala", 4, 6), true)     // 5 is between 4 and 6
test(validString("Tutorial", 1, 3), false) // 8 is not between 1 and 3
test(validString("Test", 4, 6), true)      // 4 is between 4 and 6
```


## Functions (15 points)

#### (Problem #3) `factorial` (5 points)

```scala
def factorial(n: Int): Int = ???
```

It takes an integer `n` between `0` and `10` (inclusive) and returns the
factorial of `n` (i.e., `n! = 1 * 2 * ... * n`). Note that `0! = 1`.

```scala
test(factorial(0), 1)         // 0! = 1
test(factorial(2), 2)         // 2! = 1 * 2 = 2
test(factorial(5), 120)       // 5! = 1 * 2 * 3 * 4 * 5 = 120
test(factorial(7), 5040)      // 7! = 1 * 2 * ... * 7 = 5040
test(factorial(10), 3628800)  // 10! = 1 * 2 * ... * 10 = 3628800
```

#### (Problem #4) `magic` (5 points)

```scala
def magic(x: Int): Int => Int = ???
```

It takes an integer `x` between `2` and `10` (inclusive) and returns a function
that takes an integer `y` and returns `y / x` if `y` is divisible by `x` and
`(x + 1) * y + (x - y % x)` otherwise.

```scala
test(magic(2)(7), 22)    // (2 + 1) * 7 + (2 - 7 % 2) = 22
test(magic(3)(42), 14)   // 42 / 3 = 14
test(magic(5)(3), 20)    // (5 + 1) * 3 + (5 - 3 % 5) = 20
test(magic(7)(21), 3)    // 21 / 7 = 3
test(magic(10)(25), 280) // (10 + 1) * 25 + (10 - 25 % 10) = 280
```

#### (Problem #5) `applyK` (5 points)

```scala
def applyK(f: Int => Int, k: Int): Int => Int = ???
```

It takes a function `f` whose type is `Int => Int` and returns a function that
applies `f` `k` times.
```scala
test(applyK(_ + 3, 2)(1), 7)        // 2 -> 5 -> 7
test(applyK(_ + 2, 5)(7), 17)       // 7 -> 9 -> 11 -> 13 -> 15 -> 17
test(applyK(_ * 2, 10)(1), 1024)    // 1 -> 2 -> 4 -> 8 -> ... -> 512 -> 1024
test(applyK(_ * 10, 3)(42), 42000)  // 42 -> 420 -> 4200 -> 42000
test(applyK(magic(2), 6)(7), 26)    // 7 -> 22 -> 11 -> 34 -> 17 -> 52 -> 26
```

## Collections (25 points)

#### (Problem #6) `productPos` (5 points)

```scala
def productPos(l: List[Int]): Int = ???
```

It takes an integer list `l` and returns the product of all positive integers in
`l`. If there is no positive integer in `l`, it should return `1`.
```scala
test(productPos(List(1, 2, 3, 4, 5)), 120)    // 1 * 2 * 3 * 4 * 5 = 120
test(productPos(List(1, 2, -3, -4, -5)), 2)   // 1 * 2 = 2
test(productPos(List(1, -2, 3, -4, 5)), 15)   // 1 * 3 * 5 = 15
test(productPos(List(-1, 2, 3, 4, -5)), 24)   // 2 * 3 * 4 = 24
test(productPos(List(-1, -2, -3, -4, -5)), 1) // 1
```

#### (Problem #7) `merge` (5 points)

```scala
def merge(l: List[Int]): List[Int] = ???
```

It takes an integer list `l` and returns a list containing the sum of every two
consecutive elements in `l`. If the length of `l` is odd, the last element
should be appended as is.
```scala
test(merge(Nil), Nil)                               // []
test(merge(List(1)), List(1))                       // [1]
test(merge(List(1, 2)), List(3))                    // [1+2]
test(merge(List(1, 2, 3, 4, 5)), List(3, 7, 5))     // [1+2, 3+4, 5]
test(merge(List(1, 2, 3, 4, 5, 6)), List(3, 7, 11)) // [1+2, 3+4, 5+6]
```

#### (Problem #8) `generate` (5 points)

```scala
def generate(init: Int, f: Int => Int, n: Int): List[Int] = ???
```

It takes an integer `init`, a function `f` whose type is `Int => Int`, and a
non-negative integer `n` and returns a list containing the first `n` elements
generated by repeatedly applying `f` to `init`. It should return an empty list
(`Nil`) if `n` is `0`.
```scala
test(generate(1, _ + 2, 0), Nil)
test(generate(7, _ + 2, 6), List(7, 9, 11, 13, 15, 17))
test(generate(1, _ * 2, 5), List(1, 2, 4, 8, 16))
test(generate(42, _ * 10, 4), List(42, 420, 4200, 42000))
test(generate(7, magic(2), 7), List(7, 22, 11, 34, 17, 52, 26))
```

#### (Problem #9) `incKey` (5 points)

```scala
def incKey(map: Map[String, Int], key: String): Map[String, Int] = ???
```

It takes a mapping from strings to integers `map` and a string `key` and returns
a new mapping that increments the value associated with `key` by `1`. If `key`
is not in `map`, it should return `map` as is.
```scala
val m: Map[String, Int] = Map("A" -> 1, "B" -> 2, "C" -> 3)
test(incKey(m, "A"), Map("A" -> 2, "B" -> 2, "C" -> 3))
test(incKey(m, "B"), Map("A" -> 1, "B" -> 3, "C" -> 3))
test(incKey(m, "C"), Map("A" -> 1, "B" -> 2, "C" -> 4))
test(incKey(m, "D"), Map("A" -> 1, "B" -> 2, "C" -> 3))
test(incKey(incKey(m, "A"), "B"), Map("A" -> 2, "B" -> 3, "C" -> 3))
```

#### (Problem #10) `validSums` (5 points)

```scala
def validSums(
  l: List[Int],
  r: List[Int],
  f: (Int, Int) => Boolean,
): Set[Int] = ???
```

It takes two integer lists `l` and `r` and a function `f` whose type is `(Int,
Int) => Boolean` and returns a set containing all sums of elements `x` in `l`
and `y` in `r` such that `f(x, y)` is `true`.
```scala
test(validSums(List(1, 2), List(3, 4, 5), isEvenPair), Set(4, 6))
test(validSums(List(1, 2), List(3, 4, 5), _ + _ == 7), Set(7))
test(validSums(List(1, 2), List(3, 4, 5), !isEvenPair(_, _)), Set(5, 7))
test(validSums(List(1, 2), List(3, 4, 5), (_, _) => true), Set(4, 5, 6, 7))
test(validSums(List(1, 2), List(3, 4, 5), _ * _ > 3), Set(5, 6, 7))
```


## Trees (25 points)

The `Tree` type represents a **binary tree**, which is either 1) a leaf node
(`Leaf`) with a value or 2) a non-leaf node (`Branch`) with a value, left
sub-tree, and right sub-tree.

```scala
enum Tree:
  case Leaf(value: Int)
  case Branch(left: Tree, value: Int, right: Tree)
```

> [!WARNING]
> The `Tree` type is already defined in `Template.scala`. **DO NOT** define it
> again in `Implementation.scala`.

For example, the following trees are examples of `Tree`:
```scala
import Tree.*

//  8
val tree1: Tree = Leaf(8)

//    1
//   / \
//  2   3
val tree2: Tree = Branch(Leaf(2), 1, Leaf(3))

//    4
//   / \
//  5   2
//     / \
//    8   3
val tree3: Tree = Branch(Leaf(5), 4, Branch(Leaf(8), 2, Leaf(3)))

//    7
//   / \
//  2   3
//     / \
//    5   1
//   / \
//  1   8
val tree4: Tree =
  Branch(Leaf(2), 7, Branch(Branch(Leaf(1), 5, Leaf(8)), 3, Leaf(1)))

//      42
//     /  \
//    7    7
//   / \   / \
//  7   9 3   4
val tree5: Tree =
  Branch(Branch(Leaf(7), 7, Leaf(9)), 42, Branch(Leaf(3), 7, Leaf(4)))
```

#### (Problem #11) `count` (5 points)

```scala
def count(t: Tree, x: Int): Int = ???
```

It takes a tree `t` and an integer `x` and returns the number of nodes whose
value is `x` in `t`.
```scala
test(count(tree1, 8), 1)
test(count(tree2, 4), 0)
test(count(tree3, 2), 1)
test(count(tree4, 1), 2)
test(count(tree5, 7), 3)
```

#### (Problem #12) `heightOf` (5 points)

```scala
def heightOf(t: Tree): Int = ???
```

It takes a tree `t` and returns the height of `t`. Note that the _height_ of a
tree is the length of the longest path from the root to a leaf node. The height
of a leaf node is `0`.
```scala
test(heightOf(tree1), 0)
test(heightOf(tree2), 1)
test(heightOf(tree3), 2)
test(heightOf(tree4), 3)
test(heightOf(tree5), 2)
```

#### (Problem #13) `min` (5 points)

```scala
def min(t: Tree): Int = ???
```

It takes a tree `t` and returns the minimum value in `t`.
```scala
test(min(tree1), 8)
test(min(tree2), 1)
test(min(tree3), 2)
test(min(tree4), 1)
test(min(tree5), 3)
```

#### (Problem #14) `sumLeaves` (5 points)

```scala
def sumLeaves(t: Tree): Int = ???
```

It takes a tree `t` and the sum of all values in the leaf nodes of `t`.
```scala
test(sumLeaves(tree1), 8)   // 8
test(sumLeaves(tree2), 5)   // 2 + 3 = 5
test(sumLeaves(tree3), 16)  // 5 + 8 + 3 = 16
test(sumLeaves(tree4), 12)  // 2 + 1 + 8 + 1 = 12
test(sumLeaves(tree5), 23)  // 7 + 9 + 3 + 4 = 23
```

#### (Problem #15) `inorder` (5 points)

```scala
def inorder(t: Tree): List[Int] = ???
```

It takes a tree `t` and returns a list containing all values in `t` in the
[in-order traversal order](https://en.wikipedia.org/wiki/Tree_traversal#In-order,_LNR).
```scala
test(inorder(tree1), List(8))
test(inorder(tree2), List(2, 1, 3))
test(inorder(tree3), List(5, 4, 8, 2, 3))
test(inorder(tree4), List(2, 7, 1, 5, 8, 3, 1))
test(inorder(tree5), List(7, 7, 9, 42, 3, 7, 4))
```


## Boolean Expressions (25 points)

The `BE` type represents a boolean expression. A boolean expression (`BE`) is
either a boolean literal (`Literal`), a binary operation (`And`, `Or`, or
`Imply`), or a unary operation (`Not`).

```scala
enum BE:
  case Literal(value: Boolean)
  case And(left: BE, right: BE)
  case Or(left: BE, right: BE)
  case Imply(left: BE, right: BE)
  case Not(expr: BE)
```

> [!WARNING]
> The `BE` type is already defined in `Template.scala`. **DO NOT** define it
> again in `Implementation.scala`.

For example, the following boolean expressions are examples of `BE`:
```scala
import BE.*

// #t
val be1: BE = T

// (#t | #f)
val be2: BE = Imply(T, F)

// (!(#t | #f) & !(#f | #t))
val be3: BE = And(Not(Or(T, F)), Not(Or(F, T)))

// ((#t => #f) => (#f => #t))
val be4: BE = Imply(Imply(T, F), Imply(F, T))

// ((!#t => (#t & #f)) & (!#f => (#f | #t)))
val be5: BE = And(Imply(Not(T), And(T, F)), Imply(Not(F), Or(F, T)))
```

#### (Problem #16) `isLiteral` (5 points)

```scala
def isLiteral(expr: BE): Boolean = ???
```

It takes a boolean expression `expr` and returns whether `expr` is a literal.
```scala
test(isLiteral(be1), true)
test(isLiteral(be2), false)
test(isLiteral(be3), false)
test(isLiteral(be4), false)
test(isLiteral(be5), false)
```

#### (Problem #17) `countImply` (5 points)

```scala
def countImply(expr: BE): Int = ???
```

It takes a boolean expression `expr` and returns the number of `Imply`
operations in `expr`.
```scala
test(countImply(be1), 0)
test(countImply(be2), 1)
test(countImply(be3), 0)
test(countImply(be4), 3)
test(countImply(be5), 2)
```

#### (Problem #18) `literals` (5 points)

```scala
def literals(expr: BE): List[Boolean] = ???
```

It takes a boolean expression `expr` and returns a list containing all boolean
literals in `expr` in the order of the
[in-order traversal order](https://en.wikipedia.org/wiki/Tree_traversal#In-order,_LNR).
```scala
test(literals(be1), List(true))
test(literals(be2), List(true, false))
test(literals(be3), List(true, false, false, true))
test(literals(be4), List(true, false, false, true))
test(literals(be5), List(true, true, false, false, false, true))
```

#### (Problem #19) `getString` (5 points)

```scala
def getString(expr: BE): String = ???
```

It takes a boolean expression `expr` and returns a string representation of
`expr`:
- `Literal(true)` is represented as `#t`.
- `Literal(false)` is represented as `#f`.
- `And(left, right)` is represented as `(<left> & <right>)`.
- `Or(left, right)` is represented as `(<left> | <right>)`.
- `Imply(left, right)` is represented as `(<left> => <right>)`.
- `Not(expr)` is represented as `!<expr>`.
```scala
test(getString(be1), "#t")
test(getString(be2), "(#t => #f)")
test(getString(be3), "(!(#t | #f) & !(#f | #t))")
test(getString(be4), "((#t => #f) => (#f => #t))")
test(getString(be5), "((!#t => (#t & #f)) & (!#f => (#f | #t)))")
```

#### (Problem #20) `eval` (5 points)

```scala
def eval(expr: BE): Boolean = ???
```

It takes a boolean expression `expr` and returns the result of evaluating
`expr`. Note that `(A => B)` is equivalent to `(!A | B)` (i.e., `A => B` is
`true` if `A` is `false` or `B` is `true`).
```scala
test(eval(be1), true)
test(eval(be2), false)
test(eval(be3), false)
test(eval(be4), true)
test(eval(be5), true)
```
