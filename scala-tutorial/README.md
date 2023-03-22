# Scala Tutorial

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/scala-tutorial.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

- [Primitives (10 points)](#primitives-10-points)
  - [(Problem #1) `volumeOfCuboid` (5 points)](#problem-1-volumeofcuboid-5-points)
  - [(Problem #2) `concat` (5 points)](#problem-2-concat-5-points)
- [Functions (15 points)](#functions-15-points)
  - [(Problem #3) `mulN` (5 points)](#problem-3-muln-5-points)
  - [(Problem #4) `twice` (5 points)](#problem-4-twice-5-points)
  - [(Problem #5) `compose` (5 points)](#problem-5-compose-5-points)
- [Lists (10 points)](#lists-10-points)
  - [(Problem #6) `double` (5 points)](#problem-6-double-5-points)
  - [(Problem #7) `product` (5 points)](#problem-7-product-5-points)
- [Maps (5 points)](#maps-5-points)
  - [(Problem #8) `getOrNotFound` (5 points)](#problem-8-getornotfound-5-points)
- [Algebraic Data Types (60 points)](#algebraic-data-types-60-points)
  - [(Problem #9) `depth` (15 points)](#problem-9-depth-15-points)
  - [(Problem #10) `sum` (15 points)](#problem-10-sum-15-points)
  - [(Problem #11) `countLeaves` (15 points)](#problem-11-countleaves-15-points)
  - [(Problem #12) `flatten` (15 points)](#problem-12-flatten-15-points)

## Primitives (10 points)

### (Problem #1) `volumeOfCuboid` (5 points)

It takes three non-negative integers `a`, `b`, and `c` denoting the lengths of
three sides and returns the volume of the cuboid.
```scala
test(volumeOfCuboid(1, 3, 5), 15)
test(volumeOfCuboid(2, 3, 4), 24)
```

### (Problem #2) `concat` (5 points)

It takes two strings `x` and `y` and returns their concatenation.
```scala
test(concat("x", "y"), "xy")
test(concat("abc", "def"), "abcdef")
```

## Functions (15 points)

### (Problem #3) `mulN` (5 points)

It takes an integer `n` and returns a function that multiplies a given integer
by `n`.
```scala
test(mulN(3)(5), 15)
test(mulN(4)(5), 20)
```

### (Problem #4) `twice` (5 points)

It takes a function `f` whose type is `Int => Int` and returns a function that
applies `f` twice.
```scala
test(twice(mulN(3))(5), 45)
test(twice(mulN(4))(5), 80)
```

### (Problem #5) `compose` (5 points)

It takes two `Int => Int` functions `f` and `g` and returns their composition `f
∘ g`.
```scala
test(compose(mulN(3), mulN(4))(5), 60)
test(compose(mulN(4), mulN(5))(5), 100)
```

## Lists (10 points)

### (Problem #6) `double` (5 points)

It takes an integer list `l` and returns a list whose elements are the doubles
of the elements of `l`.
```scala
test(double(List(1, 2, 3)), List(2, 4, 6))
test(double(double(List(1, 2, 3, 4, 5))), List(4, 8, 12, 16, 20))
```

### (Problem #7) `product` (5 points)

It takes an integer list `l` and returns the product of the elements of `l`.
```scala
test(product(List(1, 2, 3)), 6)
test(product(List(4, 2, 3, 7, 5)), 840)
```

## Maps (5 points)

### (Problem #8) `getOrNotFound` (5 points)

It takes a map `m` from strings to integers and a string `s` and returns the
integer corresponding to `s` if `m` has such mapping and throws an exception
with a message containing `"Not Found"` otherwise.

> :warning: You cannot use `throw` directly. Instead, please use `error` defined in `Template.scala` and refer to the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) for more details.

```scala
val m: Map[String, Int] = Map("Park" -> 1, "Kim" -> 2)
test(getOrNotFound(m, "Park"), 1)
test(getOrNotFound(m, "Kim"), 2)
testExc(getOrNotFound(m, "Ryu"), "Not Found")
testExc(getOrNotFound(m, "Hong"), "Not Found")
```

## Algebraic Data Types (60 points)

The `Tree` type represents a tree.
A tree (`Tree`) is either a non-leaf node (`Branch`) or a leaf node (`Leaf`).

- A non-leaf node (`Branch`) has an integer value (`value: Int`) and a list of
children (`children: List[Tree]`).
- A leaf node (`Leaf`) has an integer value (`value: Int`).

> :warning: Assume that a non-leaf node (`Branch`) has at least one child (`branch.children.length >= 1`).

```scala
sealed trait Tree
case class Branch(value: Int, children: List[Tree]) extends Tree
case class Leaf(value: Int) extends Tree
```

> :warning: The `Tree` type is already defined in `Template.scala`. **DO NOT** define it again in `Implementation.scala`.

For example, the following trees are examples of `Tree`:
```scala
//     1
//   / | \
//  2  3  4
val t1: Tree = Branch(1, List(Leaf(2), Leaf(3), Leaf(4)))

//    1
//   / \
//  2   3
//     / \
//    4   5
val t2: Tree = Branch(1, List(Leaf(2), Branch(3, List(Leaf(4), Leaf(5)))))

//    5
//    |
//    4
//    |
//    3
//   / \
//  2   1
val t3: Tree = Branch(5, List(Branch(4, List(Branch(3, List(Leaf(2), Leaf(1)))))))
```

### (Problem #9) `depth` (15 points)

It takes a tree `t` and returns the depth of `t`. The depth of a tree is the
length of the longest path from the root to a leaf. For example, the depth of
`t1` is 1, the depth of `t2` is 2, and the depth of `t3` is 3.
```scala
test(depth(t1), 1)
test(depth(t2), 2)
test(depth(t3), 3)
```

### (Problem #10) `sum` (15 points)

It takes a tree `t` and returns the sum of the values of its leaf nodes. For
example, the sum of `t1` is 10, the sum of `t2` is 15, and the sum of `t3` is 15.
```scala
test(sum(t1), 10)
test(sum(t2), 15)
test(sum(t3), 15)
```

### (Problem #11) `countLeaves` (15 points)

It takes a tree `t` and returns the number of its leaf nodes. For example, the
number of leaf nodes of `t1` is 3, the number of leaf nodes of `t2` is 3, and
the number of leaf nodes of `t3` is 2.
```scala
test(countLeaves(t1), 3)
test(countLeaves(t2), 3)
test(countLeaves(t3), 2)
```

### (Problem #12) `flatten` (15 points)

It takes a tree `t` and returns a list containing the value of each node in `t`
with the order of pre-order traversal. For example, the flattened list of `t1`
is `List(1, 2, 3, 4)`, the flattened list of `t2` is `List(1, 2, 3, 4, 5)`, and
the flattened list of `t3` is `List(5, 4, 3, 2, 1)`.
```scala
test(flatten(t1), List(1, 2, 3, 4))
test(flatten(t2), List(1, 2, 3, 4, 5))
test(flatten(t3), List(5, 4, 3, 2, 1))
```
