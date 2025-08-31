# Scala Tutorial

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/scala-tutorial.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

- **[Basic Data Types (10 points)](#basic-data-types-10-points)**
  - [(Problem #1) `clamp` (5 points)](#problem-1-clamp-5-points)
  - [(Problem #2) `validName` (5 points)](#problem-2-validname-5-points)
- **[Functions (15 points)](#functions-15-points)**
  - [(Problem #3) `collatzLength` (5 points)](#problem-3-collatzlength-5-points)
  - [(Problem #4) `fixpoint` (5 points)](#problem-4-fixpoint-5-points)
  - [(Problem #5) `applyK` (5 points)](#problem-5-applyk-5-points)
- **[Collections (25 points)](#collections-25-points)**
  - [(Problem #6) `sumEven` (5 points)](#problem-6-sumeven-5-points)
  - [(Problem #7) `double` (5 points)](#problem-7-double-5-points)
  - [(Problem #8) `generate` (5 points)](#problem-8-generate-5-points)
  - [(Problem #9) `join` (5 points)](#problem-9-join-5-points)
  - [(Problem #10) `subsets` (5 points)](#problem-10-subsets-5-points)
- **[Trees (25 points)](#trees-25-points)**
  - [(Problem #11) `heightOf` (5 points)](#problem-11-heightof-5-points)
  - [(Problem #12) `max` (5 points)](#problem-12-max-5-points)
  - [(Problem #13) `postorder` (5 points)](#problem-13-postorder-5-points)
  - [(Problem #14) `count` (5 points)](#problem-14-count-5-points)
  - [(Problem #15) `merge` (5 points)](#problem-15-merge-5-points)
- **[Boolean Expressions (25 points)](#boolean-expressions-25-points)**
  - [(Problem #16) `isImply` (5 points)](#problem-16-isimply-5-points)
  - [(Problem #17) `noAnd` (5 points)](#problem-17-noand-5-points)
  - [(Problem #18) `subExprs` (5 points)](#problem-18-subexprs-5-points)
  - [(Problem #19) `getString` (5 points)](#problem-19-getstring-5-points)
  - [(Problem #20) `eval` (5 points)](#problem-20-eval-5-points)


## Basic Data Types (10 points)

#### (Problem #1) `clamp` (5 points)

```scala
def clamp(lower: Int, x: Int, upper: Int): Int = ???
```

It takes three integers `lower`, `x`, and `upper` and returns `x` if `x` is
between `lower` and `upper` (inclusive). It returns `lower` if `x` is less than
`lower` and `upper` if `x` is greater than `upper`.

```scala
test(clamp(2, 3, 5), 3)         // 2 <= 3 <= 5
test(clamp(2, 1, 5), 2)         // 1 < 2
test(clamp(2, 7, 5), 5)         // 7 > 5
test(clamp(-1, 0, 3), 0)        // -1 <= 0 <= 3
test(clamp(-3, -5, -1), -3)     // -5 < -3
```

#### (Problem #2) `validName` (5 points)

```scala
def validName(name: String): Boolean = ???
```

It takes a string `name` and returns whether `name` is a valid name. A valid
name is a non-empty string that starts with an uppercase letter and its length
is less than or equal to `10`.

```scala
test(validName(""), false)               // empty string
test(validName("Alice"), true)
test(validName("Overlylongname"), false) // length > 10
test(validName("With Space"), true)
test(validName("lowercase"), false)      // starts with a lowercase letter
```


## Functions (15 points)

#### (Problem #3) `collatzLength` (5 points)

```scala
def collatzLength(n: Int): Int = ???
```

It takes a positive integer `n` and returns the length of the Collatz sequence
starting from `n`. The Collatz sequence is defined as follows:
- If `n` is `1`, the sequence ends.
- If `n` is even, the next number is `n / 2`.
- If `n` is odd and greater than `1`, the next number is `3 * n + 1`.

```scala
test(collatzLength(1), 1)   // 1
test(collatzLength(2), 2)   // 2 -> 1
test(collatzLength(3), 8)   // 3 -> 10 -> 5 -> 16 -> 8 -> 4 -> 2 -> 1
test(collatzLength(4), 3)   // 4 -> 2 -> 1
test(collatzLength(5), 6)   // 5 -> 16 -> 8 -> 4 -> 2 -> 1
```

#### (Problem #4) `fixpoint` (5 points)

```scala
def fixpoint(f: Int => Int): Int => Int = n => ???
```

It takes a function `f` whose type is `Int => Int` and returns a function that
applies `f` repeatedly starting from an integer `n` until the result does not
change anymore (i.e., `f(x) == x`).

> [!NOTE]
> You may assume that `f` always has a fixpoint for a given `n`.

```scala
test(fixpoint(_ / 2)(20), 0)            // 20 -> 10 -> 5 -> 2 -> 1 -> 0
test(fixpoint(_.abs)(-15), 15)          // -15 -> 15
test(fixpoint(gcd(48, _))(18), 6)       // 18 -> 6
test(fixpoint(sumDigits)(123456), 3)    // 123456 -> 21 -> 3
test(fixpoint(productDigits)(327), 8)   // 327 -> 42 -> 8
```
where the following helper functions are defined:
```scala
def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
def sumDigits(n: Int): Int = n.toString.map(_.asDigit).sum
def productDigits(n: Int): Int = n.toString.map(_.asDigit).product
```

#### (Problem #5) `applyK` (5 points)

```scala
def applyK(f: Int => Int, k: Int): Int => Int = ???
```

It takes a function `f` whose type is `Int => Int` and a positive integer `k`
and returns a function that applies `f` `k` times.
```scala
test(applyK(_ + 3, 2)(1), 7)           // 1 -> 4 -> 7
test(applyK(_ + 2, 5)(7), 17)          // 7 -> 9 -> 11 -> 13 -> 15 -> 17
test(applyK(_ * 2, 10)(1), 1024)       // 1 -> 2 -> 4 -> 8 -> ... -> 512 -> 1024
test(applyK(_ * 10, 3)(42), 42000)     // 42 -> 420 -> 4200 -> 42000
test(applyK(productDigits, 2)(327), 8) // 327 -> 42 -> 8
```

## Collections (25 points)

#### (Problem #6) `sumEven` (5 points)

```scala
def sumEven(l: List[Int]): Int = ???
```

It takes an integer list `l` and returns the sum of all even integers in `l`.

```scala
test(sumEven(Nil), 0)                       // no numbers
test(sumEven(List(1, 3, 5)), 0)             // no even numbers
test(sumEven(List(2, 4, 6)), 12)            // 2 + 4 + 6 = 12
test(sumEven(List(1, 2, 3, 4, 5)), 6)       // 2 + 4 = 6
test(sumEven(List(1, 2, 3, 4, 5, 6)), 12)   // 2 + 4 + 6 = 12
```

#### (Problem #7) `double` (5 points)

```scala
def double(l: List[Int]): List[Int] = ???
```

It takes an integer list `l` and returns a list in which each element of `l` is
duplicated consecutively, preserving order.

```scala
test(double(Nil), Nil)
test(double(List(42)), List(42, 42))
test(double(List(1, 2, 3)), List(1, 1, 2, 2, 3, 3))
test(double(List(5, 5, 5)), List(5, 5, 5, 5, 5, 5))
test(double(List(1, 2, 3, 4)), List(1, 1, 2, 2, 3, 3, 4, 4))
```

#### (Problem #8) `generate` (5 points)

```scala
def generate(f: Int => Int): Int => List[Int] = ???
```

It takes a function `f` whose type is `Int => Int` and returns a function that
generates a list of integers starting from a given integer while applying `f`
repeatedly until the result is not changed anymore (i.e., `f(x) == x`).

```scala
test(generate(_ / 2)(20), List(20, 10, 5, 2, 1, 0))
test(generate(_.abs)(-15), List(-15, 15))
test(generate(gcd(48, _))(18), List(18, 6))
test(generate(sumDigits)(123456), List(123456, 21, 3))
test(generate(productDigits)(327), List(327, 42, 8))
```
with the helper functions defined in [Problem #4](#problem-4-fixpoint-5-points).

#### (Problem #9) `join` (5 points)

```scala
def join(l: Map[String, Int], r: Map[String, Int]): Map[String, Int] = ???
```

It takes two mappings from strings to integers `l` and `r` and returns a new
mapping that contains all keys in `l` and `r` whose values are summed if a key
is in both `l` and `r`. If a key is only in `l` or `r`, it should be included in
the result as is.

```scala
test(join(m1, Map()), m1)
test(join(Map(), m1), m1)
test(join(m1, m2), Map("A" -> 1, "B" -> 5, "C" -> 7, "D" -> 5))
test(join(m1, m3), Map("A" -> 3, "B" -> 2, "C" -> 4))
test(join(m2, m3), Map("A" -> 2, "B" -> 3, "C" -> 5, "D" -> 5))
```
where the following helper mappings are defined:
```scala
val m1: Map[String, Int] = Map("A" -> 1, "B" -> 2, "C" -> 3)
val m2: Map[String, Int] = Map("B" -> 3, "C" -> 4, "D" -> 5)
val m3: Map[String, Int] = Map("A" -> 2, "C" -> 1)
```

#### (Problem #10) `subsets` (5 points)

```scala
def subsets(set: Set[Int]): List[Set[Int]] = ???
```

It takes a set of integers `set` and returns a list of all non-empty subsets of
`set`. Each subset is considered as a list of its elements sorted in ascending
order, and the resulting subsets are ordered lexicographically based on these
lists as follows:
* `[1, 2] < [3]` because at the first position `1 < 3`.
* `[1, 2, 3] < [1, 3, 4]` because `2 < 3` at the second position.
* `[1, 2] < [1, 2, 3]` because the first two numbers are the same, but the first
  list is shorter.

```scala
test(subsets(Set()), Nil)
test(subsets(Set(1)), List(Set(1)))
test(subsets(Set(1, 2)), List(Set(1), Set(1, 2), Set(2)))
test(subsets(Set(3, 2, 1)), List(
  Set(1), Set(1, 2), Set(1, 2, 3), Set(1, 3), Set(2), Set(2, 3), Set(3),
))
test(subsets(Set(1, 2, 3, 4)), List(
  Set(1), Set(1, 2), Set(1, 2, 3), Set(1, 2, 3, 4), Set(1, 2, 4), Set(1, 3),
  Set(1, 3, 4), Set(1, 4), Set(2), Set(2, 3), Set(2, 3, 4), Set(2, 4), Set(3),
  Set(3, 4), Set(4),
))
```


## Trees (25 points)

The `Tree` type represents a _binary tree_, which is either 1) a leaf node
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

#### (Problem #11) `heightOf` (5 points)

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

#### (Problem #12) `max` (5 points)

```scala
def max(t: Tree): Int = ???
```

It takes a tree `t` and returns the maximum value in `t`.

```scala
test(max(tree1), 8)
test(max(tree2), 3)
test(max(tree3), 8)
test(max(tree4), 8)
test(max(tree5), 42)
```

#### (Problem #13) `postorder` (5 points)

```scala
def postorder(t: Tree): List[Int] = ???
```

It takes a tree `t` and returns a list containing all values in `t` in the
[post-order traversal order](https://en.wikipedia.org/wiki/Tree_traversal#Post-order,_LRN).

```scala
test(postorder(tree1), List(8))
test(postorder(tree2), List(2, 3, 1))
test(postorder(tree3), List(5, 8, 3, 2, 4))
test(postorder(tree4), List(2, 1, 8, 5, 1, 3, 7))
test(postorder(tree5), List(7, 9, 7, 3, 4, 7, 42))
```

#### (Problem #14) `count` (5 points)

```scala
def count(t: Tree, f: Int => Boolean): Int = ???
```

It takes a tree `t` and a predicate function `f` whose type is `Int => Boolean`
and returns the number of nodes in `t` whose values satisfy `f`.

```scala
test(count(tree1, _ % 2 == 0), 1)    // 8
test(count(tree2, _ % 2 == 1), 2)    // 1, 3
test(count(tree3, _ >= 4), 3)        // 4, 5, 8
test(count(tree4, _ < 5), 4)         // 2, 1, 3, 1
test(count(tree5, _ == 7), 3)        // 7, 7, 7
```

#### (Problem #15) `merge` (5 points)

```scala
def merge(left: Tree, right: Tree): Tree = ???
```

It takes two trees `left` and `right` and merges them into a new tree as
follows:
* If both `left` and `right` are branches, create a new branch whose value is the
  sum of the values of `left` and `right`, and whose left and right sub-trees
  are obtained by merging the left and right sub-trees of `left` and `right`,
  respectively.
* Otherwise, create a new leaf whose value is the sum of the values of `left`
  and `right`. (Note that it ignores the sub-trees of `left` and `right` in this
  case.)

```scala
test(merge(tree1, tree1), Leaf(16))
test(merge(tree1, tree2), Leaf(9))
test(merge(tree2, tree3), Branch(Leaf(7), 5, Leaf(5)))
test(merge(tree3, tree4), Branch(Leaf(7), 11, Branch(Leaf(13), 5, Leaf(4))))
test(merge(tree4, tree5), Branch(Leaf(9), 49, Branch(Leaf(8), 10, Leaf(5))))
```


## Boolean Expressions (25 points)

The `BE` type represents a _boolean expression_. A boolean expression (`BE`) is
either a boolean literal (`Literal`), a variable (`Variable`), a binary
operation (`And`, `Or`, or `Imply`), or a unary operation (`Not`).

```scala
enum BE:
  case Literal(value: Boolean)
  case Variable(name: String)
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

val T = Literal(true)
val F = Literal(false)
val X = Variable("x")
val Y = Variable("y")

// #t
val be1: BE = T

// (x => #f)
val be2: BE = Imply(X, F)

// (!(#t || x) && !(y || #f))
val be3: BE = And(Not(Or(T, X)), Not(Or(Y, F)))

// ((#t && (x => #f)) || (x => (#t => y)))
val be4: BE = Or(And(T, Imply(X, F)), Imply(X, Imply(T, Y)))

// ((!(#t => (x && y)) && (!#f => (#f || x))) => y)
val be5: BE = Imply(And(Not(Imply(T, And(X, Y))), Imply(Not(F), Or(F, X))), Y)
```

#### (Problem #16) `isImply` (5 points)

```scala
def isImply(expr: BE): Boolean = ???
```

It takes a boolean expression `expr` and returns whether `expr` is an `Imply`.
```scala
test(isImply(be1), false)
test(isImply(be2), true)
test(isImply(be3), false)
test(isImply(be4), false)
test(isImply(be5), true)
```

#### (Problem #17) `noAnd` (5 points)

```scala
def noAnd(expr: BE): Boolean = ???
```

It takes a boolean expression `expr` and returns whether `expr` does not contain
any `And` operations.

```scala
test(noAnd(be1), true)
test(noAnd(be2), true)
test(noAnd(be3), false)
test(noAnd(be4), false)
test(noAnd(be5), false)
```

#### (Problem #18) `subExprs` (5 points)

```scala
def subExprs(expr: BE): Set[BE] = ???
```

It takes a boolean expression `expr` and returns a set containing all
sub-expressions of `expr`. A sub-expression of `expr` is either `expr` itself
or a sub-expression of any direct sub-expressions of `expr`.

```scala
test(subExprs(be1), Set(T))
test(subExprs(be2), Set(X, F, be2))
test(subExprs(be3), Set(
  T, F, X, Y, Or(T, X), Or(Y, F), Not(Or(T, X)), Not(Or(Y, F)), be3
))
test(subExprs(be4), Set(
  T, F, X, Y, Imply(X, F), Imply(T, Y), And(T, Imply(X, F)),
  Imply(X, Imply(T, Y)), be4,
))
test(subExprs(be5), Set(
  T, F, Y, X, Not(F), Or(F, X), And(X, Y), Imply(T, And(X, Y)),
  Imply(Not(F), Or(F, X)), Not(Imply(T, And(X, Y))),
  And(Not(Imply(T, And(X, Y))), Imply(Not(F), Or(F, X))), be5,
))
```

#### (Problem #19) `getString` (5 points)

```scala
def getString(expr: BE): String = ???
```

It takes a boolean expression `expr` and returns a string representation of
`expr`:
- `Literal(true)` is represented as `#t`.
- `Literal(false)` is represented as `#f`.
- `Variable(name)` is represented as `name`.
- `And(left, right)` is represented as `(<left> && <right>)`.
- `Or(left, right)` is represented as `(<left> || <right>)`.
- `Imply(left, right)` is represented as `(<left> => <right>)`.
- `Not(expr)` is represented as `!<expr>`.
```scala
test(getString(be1), "#t")
test(getString(be2), "(x => #f)")
test(getString(be3), "(!(#t || x) && !(y || #f))")
test(getString(be4), "((#t && (x => #f)) || (x => (#t => y)))")
test(getString(be5), "((!(#t => (x && y)) && (!#f => (#f || x))) => y)")
```

#### (Problem #20) `eval` (5 points)

```scala
def eval(expr: BE, env: Map[String, Boolean]): Boolean = ???
```

It takes a boolean expression `expr` and an environment `env` that maps variable
names to boolean values, and returns the result of evaluating `expr` under
`env`. If the environment does not contain a mapping for a variable in `expr`,
it should be treated as `false`.

```scala
test(eval(be1, env1), true)
test(eval(be2, env1), true)
test(eval(be3, env2), false)
test(eval(be4, env2), false)
test(eval(be5, env2), false)
```
where the following helper environments are defined:
```scala
val env1: Map[String, Boolean] = Map("x" -> false, "y" -> true)
val env2: Map[String, Boolean] = Map("x" -> true)
```
