# Advanced Features in Scala

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/scala-adv-feat.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.
> first if you have not read them.

* **[Curry-Howard Correspondence (50 points)](#curry-howard-correspondence-50-points)**
  * [(Problem #1) `proof1` (10 points)](#problem-1-proof1-10-points)
  * [(Problem #2) `proof2` (10 points)](#problem-2-proof2-10-points)
  * [(Problem #3) `proof3` (10 points)](#problem-3-proof3-10-points)
  * [(Problem #4) `proof4` (10 points)](#problem-4-proof4-10-points)
  * [(Problem #5) `proof5` (10 points)](#problem-5-proof5-10-points)
* **[Type Classes (30 points)](#type-classes-30-points)**
  * [(Problem #6) `treeNums` (15 points)](#problem-6-treenums-15-points)
  * [(Problem #7) `matrixNums` (15 points)](#problem-7-matrixnums-15-points)
* **[Metaprogramming (20 points)](#metaprogramming-20-points)**
  * [(Problem #8) `unrollMul` (10 points)](#problem-8-unrollmul-10-points)
  * [(Problem #9) `unrollPow` (10 points)](#problem-9-unrollpow-10-points)


## Curry-Howard Correspondence (50 points)

**Curry-Howard correspondence** is a correspondence between **logic** and
**programming**. The key idea is to represent **propositions** as **types** as
follows:

| Proposition          | Type                                      |
|:--------------------:|:------------------------------------------|
| $\textsf{True}$      | `type True = Unit`                        |
| $\textsf{False}$     | `type False = Nothing`                    |
| $A \land B$          | `type /\[A, B] = (A, B)`                  |
| $A \lor B$           | `type \/[A, B] = Either[A, B]`            |
| $A \Rightarrow B$    | `A => B`                                  |
| $A \Leftrightarrow B$| `type <=>[A, B] = (A => B) /\ (B => A)`   |
| $\neg A$             | `type ![A] = A => False`                  |
| $\forall A. \; P(A)$ | `[A] => P[A]`                             |

And, we can represent **proofs** as **programs**. For example, $\textsf{True}$
is always **true**, and we can prove it by providing a value of type `Unit`.
However, $\textsf{False}$ is always **false**, and we cannot provide a value of
type `Nothing`. In addition, let's consider the following proposition:
$$
\forall A, B. \; A \land B \Rightarrow A
$$
This proposition is **true** because $A$ is always **true** if $A \land B$ is
**true**. We can **prove** this proposition by providing a value of type
`[A, B] => (A /\ B) => A` (i.e., `[A, B] => (A, B) => A`) as follows:
```scala
val proof: [A, B] => A /\ B => A = [A, B] => (pair: A /\ B) => pair._1
```

In this part, you need to implement the following **proofs** for each
proposition in `Implementation.scala`.


### (Problem #1) `proof1` (10 points)

The given **proposition** is:
$$
\forall A, B, C. \;
(A \Rightarrow B) \land (B \Rightarrow C) \Rightarrow (A \Rightarrow C)
$$
same as the following Scala **type**:
```scala
type Prop1 = [A, B, C] => (
  ((A => B) /\ (B => C)) => (A => C),
)
```

Please implement the following **proof** by providing a value of type `Prop1`:
```scala
val proof1: Prop1 = ???
```

### (Problem #2) `proof2` (10 points)

The given **proposition** is:
$$
\forall A, B, C, D. \;
(A \Rightarrow B) \land (B \Rightarrow C) \land (C \Rightarrow D) \land (D
\Rightarrow A) \Rightarrow (B \Leftrightarrow D)
$$
same as the following Scala **type**:
```scala
type Prop2 = [A, B, C, D] => (
  ((A => B) /\ (B => C) /\ (C => D) /\ (D => A)) => B <=> D,
)
```

Please implement the following **proof** by providing a value of type `Prop2`:
```scala
val proof2: Prop2 = ???
```

### (Problem #3) `proof3` (10 points)

The given **proposition** is:
$$
\forall P, Q. \;
(\forall X, Y. \; (P(X) \land P(Y)) \Rightarrow Q(X, Y)) \Rightarrow
(\forall Z. \; \neg Q(Z, Z) \Rightarrow \neg P(Z))
$$
same as the following Scala **type**:
```scala
type Prop3 = [P[_], Q[_, _]] => (
  ([X, Y] => ((P[X] /\ P[Y]) => Q[X, Y])) => ([Z] => ![Q[Z, Z]] => ![P[Z]]),
)
```

Please implement the following **proof** by providing a value of type `Prop3`:
```scala
val proof3: Prop3 = ???
```

### (Problem #4) `proof4` (10 points)

The given **proposition** is:
$$
(\forall A, B. \; (\neg A \land \neg B) \Rightarrow \neg (A \lor B)) \land
(\forall A, B. \; \neg (A \lor B) \Rightarrow (\neg A \land \neg B))
$$
same as the following Scala **type**:
```scala
type Prop4 =
  ([A, B] => (![A] /\ ![B]) => ![A \/ B]) /\
  ([A, B] => ![A \/ B] => (![A] /\ ![B]))
```

Please implement the following **proof** by providing a value of type `Prop4`:
```scala
val proof4: Prop4 = ???
```

### (Problem #5) `proof5` (10 points)

The given **proposition** is:
$$
\forall A, B, C. \;
((\neg A \land B) \lor (\neg A \land C)) \Rightarrow \neg (A \lor (\neg B \lor
\neg C))
$$
same as the following Scala **type**:
```scala
type Prop5 = [A, B, C] => (
  ((![A] /\ B) \/ (![A] /\ C)) => ![A \/ ![B \/ C]],
)
```

Please implement the following **proof** by providing a value of type `Prop5`:
```scala
val proof5: Prop5 = ???
```


## Type Classes (30 points)

In `Template.scala`, the `Nums[T]` **type class** is defined as follows:
```scala
trait Nums[T] {
  type Num = T
  val zero: Num
  val one: Num
  def add(x: Num, y: Num): Num
  def smul(x: Num, k: Int): Num
  def mul(x: Num, y: Num): Num
}
```
With the **type class** `Nums[T]`, 1) **implicit conversion** from `Int` to `T`
and 2) **extension methods** for `T` are defined as follows:
```scala
given [T](using nums: Nums[T]): Conversion[Int, T] = _ match
  case 0 => nums.zero
  case 1 => nums.one
  case n => nums.smul(nums.one, n)

extension [T](t: T)(using nums: Nums[T]) {
  def +(u: T): T = nums.add(t, u)
  def *(k: Int): T = nums.smul(t, k)
  def *(u: T): T = nums.mul(t, u)
}
```

For example, **type class** instances for `Int` and `Boolean` are defined as
follows:
```scala
given intNums: Nums[Int] with {
  val zero: Int = 0
  val one: Int = 1
  def add(x: Int, y: Int): Int = x + y
  def smul(x: Int, k: Int): Int = k * x
  def mul(x: Int, y: Int): Int = x * y
}

given boolNums: Nums[Boolean] with {
  val zero: Boolean = false
  val one: Boolean = true
  def add(x: Boolean, y: Boolean): Boolean = x || y
  def smul(x: Boolean, k: Int): Boolean = if (k % 2 == 0) false else x
  def mul(x: Boolean, y: Boolean): Boolean = x && y
}
```

In this part, we need to implement two **type class** instances for `Tree[T]`
and `Matrix[T]` in `Implementation.scala`.


### (Problem #6) `treeNums` (15 points)

The `Tree[T]` is a **tree** with values of type `T`. The `Tree[T]` is defined as
follows:
```scala
case class Tree[T](value: T, children: List[Tree[T]]) {
  override def toString: String =
    s"T($value${children.map(", " + _).mkString})"
}
object Tree {
  def apply[T](value: T, children: Tree[T]*): Tree[T] =
    Tree(value, children.toList)
}
```

Please implement the **type class** instance for `Tree[T]` as follows:
```scala
given treeNums[T: Nums]: Nums[Tree[T]] = ???
```

> [!NOTE]
> The `add` and `mul` operations take two `Tree[T]` values and return a new
> `Tree[T]` value. If there is **no corresponding subtree** in one of the trees,
> please **ignore** the subtree. For example, if we add two trees `Tree(1,
> Tree(2))` and `Tree(3, Tree(4), Tree(5))`, the result is `Tree(4, Tree(6))` by
> ignoring the right subtree `Tree(5)`.


### (Problem #7) `matrixNums` (15 points)

The `Matrix[T]` is a **square matrix** with values of type `T`. The `Matrix[T]`
is defined as follows:
```scala
opaque type Matrix[T] = Vector[Vector[T]]
extension [T](x: Matrix[T]) def apply(i: Int, j: Int): T = x(i)(j)
object Matrix:
  def apply[T](n: Int)(seq: T*): Matrix[T] = from(n)(seq)
  def from[T](n: Int)(seq: Seq[T]): Matrix[T] =
    if (seq.size != n * n) error("Invalid matrix data")
    else seq.toVector.grouped(n).toVector
```

Please implement the **type class** instance for `Matrix[T]` as follows:
```scala
def matrixNums[T: Nums](n: Int): Nums[Matrix[T]]
```
where `n` is the size of the square matrix.

> [!WARNING]
> Since the `Matrix[T]` is defined as an **opaque type**, you cannot access any
> members of the `Matrix[T]` directly. You can only access the element of the
> matrix by using the **extension method** `apply(i: Int, j: Int): T` where `i`
> and `j` are the row and column indices, respectively. And, you can create a
> new `Matrix[T]` by using the `apply` or `from` method in the companion object
> `Matrix`.



## Metaprogramming (20 points)

In this part, you need to implement two **inline** methods in
`Implementation.scala` to **unroll** the **multiplication** and **power**
operations.

### (Problem #8) `unrollMul` (10 points)

Please implement the following **inline** method to **unroll** the
**multiplication** operation only using two operations: 1) **increment** by left
operand (e.g., `+ x`) or 2) **double** (i.e., `* 2`) the value.
```scala
transparent inline def unrollMul(
  inline x: Int,
  inline n: Int,
): Int = ???
```

For example, the **expected inlined code** of the `unrollMul` method is as
follows:
```scala
unrollMul(25, 31)   // 775
val x = 2
unrollMul(x, 0),    // 0
unrollMul(x, 1),    // x
unrollMul(x, 15),   // ((x * 2 + x) * 2 + x) * 2 + x
```

### (Problem #9) `unrollPow` (10 points)

Please implement the following **inline** method to **unroll** the **power**
($x^n$) operation as the sequence of **multiplication** operations.
```scala
transparent inline def unrollPow(
  inline x: Int,
  inline n: Int,
  inline k: Int,
): Int = ???
```
where `k` denotes the **maximum** number of **multiplication** operations, and
it should directly calls the following `pow` method for the remaining
calculation:
```scala
def pow(x: Int, n: Int): Int = math.pow(x, n).toInt
```

For example, the **expected inlined code** of the `unrollPow` method is as
follows:
```scala
unrollPow(2, 10, 15)    // 1024
unrollPow(2, 5, 3)      // 8 * pow(2, 2)
unrollPow(2, 8, 0)      // pow(2, 8)
val x = 2
unrollPow(x, 5, 2)      // x * x * pow(x, 3)
unrollPow(x, 7, 5)      // x * x * x * x * x * pow(x, 2)
unrollPow(x, 7, 0)      // pow(x, 7)
```
