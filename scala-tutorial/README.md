# Scala Tutorial

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/scala-tutorial.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md)
> first if you have not read them.

- **[Built-in Data Types (10 points)](#built-in-data-types-10-points)**
  - [(Problem #1) `sqsum` (5 points)](#problem-1-sqsum-5-points)
  - [(Problem #2) `concat` (5 points)](#problem-2-concat-5-points)
- **[Functions (15 points)](#functions-15-points)**
  - [(Problem #3) `subN` (5 points)](#problem-3-subn-5-points)
  - [(Problem #4) `twice` (5 points)](#problem-4-twice-5-points)
  - [(Problem #5) `compose` (5 points)](#problem-5-compose-5-points)
- **[Lists (15 points)](#lists-15-points)**
  - [(Problem #6) `sumOnlyOdd` (5 points)](#problem-6-sumonlyodd-5-points)
  - [(Problem #7) `foldWith` (5 points)](#problem-7-foldwith-5-points)
  - [(Problem #8) `toSet` (5 points)](#problem-8-toset-5-points)
- **[Maps and Sets (10 points)](#maps-and-sets-10-points)**
  - [(Problem #9) `getOrZero` (5 points)](#problem-9-getorzero-5-points)
  - [(Problem #10) `setMinus` (5 points)](#problem-10-setminus-5-points)
- **[Trees (25 points)](#trees-25-points)**
  - [(Problem #11) `has` (5 points)](#problem-11-has-5-points)
  - [(Problem #12) `maxDepthOf` (5 points)](#problem-12-maxdepthof-5-points)
  - [(Problem #13) `mul` (5 points)](#problem-13-mul-5-points)
  - [(Problem #14) `countLeaves` (5 points)](#problem-14-countleaves-5-points)
  - [(Problem #15) `postOrder` (5 points)](#problem-15-postorder-5-points)
- **[Boolean Expressions (25 points)](#boolean-expressions-25-points)**
  - [(Problem #16) `countLiterals` (5 points)](#problem-16-countliterals-5-points)
  - [(Problem #17) `countNots` (5 points)](#problem-17-countnots-5-points)
  - [(Problem #18) `depth` (5 points)](#problem-18-depth-5-points)
  - [(Problem #19) `eval` (5 points)](#problem-19-eval-5-points)
  - [(Problem #20) `getString` (5 points)](#problem-20-getstring-5-points)


## Built-in Data Types (10 points)

#### (Problem #1) `sqsum` (5 points)

It takes two integers `x` and `y` and returns the sum of the squares of `x` and
`y`.
```scala
test(sqsum(0, 0), 0)
test(sqsum(2, 3), 13)
test(sqsum(-3, 4), 25)
```

#### (Problem #2) `concat` (5 points)

It takes two strings `x` and `y` and returns their concatenation.
```scala
test(concat("Hello ", "World!"), "Hello World!")
test(concat("COSE", "212"), "COSE212")
test(concat("COSE", "215"), "COSE215")
```

## Functions (15 points)

#### (Problem #3) `subN` (5 points)

It takes an integer `n` and returns a function that subtracts `n` from an
integer.
```scala
test(subN(3)(5), 2)
test(subN(4)(13), 9)
test(subN(243)(-942), -1185)
```

#### (Problem #4) `twice` (5 points)

It takes a function `f` whose type is `Int => Int` and returns a function that
applies `f` twice.
```scala
test(twice(_ + 3)(1), 7)
test(twice(subN(3))(10), 4)
test(twice(_ * 10)(42), 4200)
```

#### (Problem #5) `compose` (5 points)

It takes two `Int => Int` functions `f` and `g` and returns their composition `f
∘ g`.
```scala
test(compose(_ + 3, _ * 2)(1), 5)
test(compose(_ * 10, _ + 1)(42), 430)
test(compose(subN(3), subN(2))(10), 5)
```

## Lists (15 points)

#### (Problem #6) `sumOnlyOdd` (5 points)

It takes an integer list `l` and returns the sum of the odd integers in `l`.
```scala
test(sumOnlyOdd(List(2)), 0)
test(sumOnlyOdd(List(1, 2, 3)), 4)
test(sumOnlyOdd(List(4, 2, 3, 7, 5)), 15)
```

#### (Problem #7) `foldWith` (5 points)

It takes a function `f` whose type is `(Int, Int) => Int` and produces a
function that takes an integer list `l` and returns the result of folding `l`
with `f` starting from `0`.
```scala
test(foldWith(_ + _)(List(1, 2, 3)), 6)
// 6 = 0 + 1 + 2 + 3
test(foldWith(_ - _)(List(5, 9, 2, 3)), -19)
// -19 = 0 - 5 - 9 - 2 - 3
test(foldWith(_ * 2 + _)(List(4, 7, 3, 2)), 68)
// 68 = (((0 * 2 + 4) * 2 + 7) * 2 + 3) * 2 + 2
```

#### (Problem #8) `toSet` (5 points)

It takes an integer list `l` and an integer `from` and returns a set containing
all integers in `l` whose indices are greater than or equal to `from`.
```scala
test(toSet(List(1, 5, 2, 7, 4, 2, 4), 0), Set(1, 2, 4, 5, 7))
// { 1, 5, 2, 7, 4, 2, 4 } = { 1, 2, 4, 5, 7 }
test(toSet(List(1, 5, 2, 7, 4, 2, 4), 2), Set(2, 4, 7))
// { 2, 7, 4, 2, 4 } = { 2, 4, 7 }
test(toSet(List(1, 5, 2, 7, 4, 2, 4), 4), Set(2, 4))
// { 4, 2, 4 } = { 2, 4 }
```

## Maps and Sets (10 points)

#### (Problem #9) `getOrZero` (5 points)

It takes a map `m` from strings to integers and a string `s` and returns the
integer corresponding to `s` if `m` has such mapping and `0` otherwise.
```scala
test(getOrZero(Map("Park" -> 3, "Kim" -> 5), "Park"), 3)
test(getOrZero(Map("Park" -> 3, "Kim" -> 5), "Lee"), 0)
test(getOrZero(Map("Park" -> 3, "Kim" -> 5), "Kim"), 5)
```

#### (Problem #10) `setMinus` (5 points)

It takes two sets `s1` and `s2` and returns the set containing all elements in
`s1` that are not in `s2`.
```scala
test(setMinus(Set(1, 2, 3), Set(2, 3, 4)), Set(1))
test(setMinus(Set(1, 2, 3), Set(4, 5, 6)), Set(1, 2, 3))
test(setMinus(Set(1, 2, 3), Set(1, 2, 3, 4)), Set())
```


## Trees (25 points)

The `Tree` type represents a tree.
A tree (`Tree`) is either 1) a leaf node (`Leaf`) with a value or 2) a non-leaf
node (`Branch`) with a value, left sub-tree, and right sub-tree.

```scala
enum Tree:
  case Leaf(value: Int)
  case Branch(value: Int, left: Tree, right: Tree)
```

> :warning: The `Tree` type is already defined in `Template.scala`. **DO NOT**
> define it again in `Implementation.scala`.

For example, the following trees are examples of `Tree`:
```scala
//  8
val tree1: Tree = Leaf(8)

//    4
//   / \
//  5   2
//     / \
//    8   3
val tree2: Tree = Branch(4, Leaf(5), Branch(2, Leaf(8), Leaf(3)))

//    7
//   / \
//  2   3
//     / \
//    5   1
//   / \
//  1   8
val tree3: Tree = Branch(7, Leaf(2), Branch(3, Branch(5, Leaf(1), Leaf(8)), Leaf(1)))
```

#### (Problem #11) `has` (5 points)

It takes an integer `value` and produces a function that takes a tree `t` and
returns `true` if `t` has a node with `value` and `false` otherwise.
```scala
test(has(8)(tree1), true)
test(has(7)(tree2), false)
test(has(1)(tree3), true)
```

#### (Problem #12) `maxDepthOf` (5 points)

It takes an integer `value` and produces a function that takes a tree `t` and
returns the maximum depth of a node with `value` in `t`. Note that the _depth_
of a node is the number of edges from the root to the node. If there is no node
with `value` in `t`, it should return `None`.
```scala
test(maxDepthOf(8)(tree1), Some(0))
test(maxDepthOf(7)(tree2), None)
test(maxDepthOf(1)(tree3), Some(3))
```

#### (Problem #13) `mul` (5 points)

It takes a tree `t` and returns the product of all values in `t`.
```scala
test(mul(tree1), 8)
test(mul(tree2), 960)
test(mul(tree3), 1680)
```

#### (Problem #14) `countLeaves` (5 points)

It takes a tree `t` and returns the number of its leaf nodes.
```scala
test(countLeaves(tree1), 1)
test(countLeaves(tree2), 3)
test(countLeaves(tree3), 4)
```

#### (Problem #15) `postOrder` (5 points)

It takes a tree `t` and returns a list containing all values in `t` in the
[post-order traversal order](https://en.wikipedia.org/wiki/Tree_traversal#Post-order,_LRN).
```scala
test(postOrder(tree1), List(8))
test(postOrder(tree2), List(5, 8, 3, 2, 4))
test(postOrder(tree3), List(2, 1, 8, 5, 1, 3, 7))
```


## Boolean Expressions (25 points)

The `BE` type represents a boolean expression.
A boolean expression (`BE`) is either a literal (`True` or `False`), a boolean
operation (`And` or `Or`), or a unary operation (`Not`).

```scala
enum BE:
  case True
  case False
  case And(left: BE, right: BE)
  case Or(left: BE, right: BE)
  case Not(expr: BE)
```

> :warning: The `BE` type is already defined in `Template.scala`. **DO NOT**
> define it again in `Implementation.scala`.

For example, the following boolean expressions are examples of `BE`:
```scala
// (true | false)
val be1: BE = Or(True, False)

// (!(true | false) & !(false | true))
val be2: BE = And(Not(Or(True, False)), Not(Or(False, True)))

// (!((false | !true) & false) & (true & !false))
val be3: BE = And(Not(And(Or(False, Not(True)), False)), And(True, Not(False)))
```

#### (Problem #16) `countLiterals` (5 points)

It takes a boolean expression `expr` and returns the number of literals in
`expr`.
```scala
test(countLiterals(be1), 2)
test(countLiterals(be2), 4)
test(countLiterals(be3), 5)
```

#### (Problem #17) `countNots` (5 points)

It takes a boolean expression `expr` and returns the number of `Not` operations
in `expr`.
```scala
test(countNots(be1), 0)
test(countNots(be2), 2)
test(countNots(be3), 3)
```

#### (Problem #18) `depth` (5 points)

It takes a boolean expression `expr` and returns the depth of `expr`. Note that
the _depth_ of an expression is the number of nested operations in the
expression.
```scala
test(depth(be1), 1)
test(depth(be2), 3)
test(depth(be3), 5)
```

#### (Problem #19) `eval` (5 points)

It takes a boolean expression `expr` and returns the result of evaluating
`expr`.
```scala
test(eval(be1), true)
test(eval(be2), false)
test(eval(be3), true)
```

#### (Problem #20) `getString` (5 points)

It takes a boolean expression `expr` and returns a string representation of
`expr`:
- `True` is represented as `true`.
- `False` is represented as `false`.
- `And(left, right)` is represented as `(<left> & <right>)`.
- `Or(left, right)` is represented as `(<left> | <right>)`.
- `Not(expr)` is represented as `!<expr>`.
```scala
test(getString(be1), "(true | false)")
test(getString(be2), "(!(true | false) & !(false | true))")
test(getString(be3), "(!((false | !true) & false) & (true & !false))")
```
