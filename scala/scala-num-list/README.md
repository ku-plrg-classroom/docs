# Infinite Lists for Number Data Types in Scala

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/scala-num-list.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

* **[Number Data Types (50 points)](#number-data-types-50-points)**
  * [(Problem #1) `bigIntNums` (10 points)](#problem-1-bigintnums-10-points)
  * [(Problem #2) `lengthNums` (10 points)](#problem-2-lengthnums-10-points)
  * [(Problem #3) `moduloNums` (10 points)](#problem-3-modulonums-10-points)
  * [(Problem #4) `binaryNums` (10 points)](#problem-4-binarynums-10-points)
  * [(Problem #5) `matrixNums` (10 points)](#problem-5-matrixnums-10-points)
* **[Infinite Lists (50 points)](#infinite-lists-50-points)**
  * [(Problem #6) `incs` (10 points)](#problem-6-incs-10-points)
  * [(Problem #7) `squares` (10 points)](#problem-7-squares-10-points)
  * [(Problem #8) `powers` (10 points)](#problem-8-powers-10-points)
  * [(Problem #9) `repeat` (10 points)](#problem-9-repeat-10-points)
  * [(Problem #10) `weights` (10 points)](#problem-10-weights-10-points)


## Number Data Types (50 points)

In `Template.scala`, the `Nums` trait contains one **abstract type member** and
**five abstract methods** for number data types.
* `type Num` represents the **number** type.
* `zero` represents **zero**.
* `one` represents **one**.
* `add(x, y)` returns the **sum** of two numbers.
* `smul(k, x)` returns the **scaled** number by a scalar.
* `mul(x, y)` returns the **product** of two numbers.

```scala
/** Number data types */
trait Nums:
  type Num
  val zero: Num
  val one: Num
  def add(x: Num, y: Num): Num
  def smul(k: BigInt, x: Num): Num
  def mul(x: Num, y: Num): Num
```

> [!NOTE]
>
> You need to understand path-dependent types to implement the `Nums` trait.

 You need to implement these methods in five different implementations in
`Implementation.scala`.


### (Problem #1) `bigIntNums` (10 points)

The `bigIntNums` is an implementation of the `Nums` trait satisfying the
following requirements:

```scala
def bigIntNums: Nums { type Num = BigInt }
```

* `Num` is `BigInt`
* `zero` is `0`
* `one` is `1`
* `add(x, y)` returns the sum of two big integers.
* `smul(k, x)` returns the scaled big integer by a scalar.
* `mul(x, y)` returns the product of two big integers.


### (Problem #2) `lengthNums` (10 points)

The `lengthNums` is an implementation of the `Nums` trait satisfying the
following requirements:

```scala
def lengthNums: Nums { type Num = String }
```

* `Num` is `String`
* `zero` is `""`
* `one` is any string whose length is `1`
* `add(x, y)` returns the concatenation of two strings.
* `smul(k, x)` returns the repeated string `x` `k` times.
* `mul(x, y)` returns the any string whose length is the product of the lengths
    of two strings.


### (Problem #3) `moduloNums` (10 points)

The `moduloNums` is a method that takes a **modulus** `m` of type `BigInt` and
returns an implementation of the `Nums` trait satisfying the following
requirements:

```scala
def moduloNums(m: BigInt): Nums { type Num = BigInt }
```

* `Num` is `BigInt` modulo `m`
* `zero` is `0`
* `one` is `1`
* `add(x, y)` returns the sum of two big integers modulo `m`.
* `smul(k, x)` returns the scaled big integer by a scalar and then takes the
    modulo `m`.
* `mul(x, y)` returns the product of two big integers and then takes the modulo
    `m`.

> [!NOTE]
>
> If the modulo `m` is less than `2`, it should throw an error using `error`
> function. Since the given template code already handles this case, do not
> delete the existing code.


### (Problem #4) `binaryNums` (10 points)

In `Template.scala`, the `Binary` type is a algebraic data type (ADT)
representing **binary**: `Zero` and `One`.

```scala
/** Binary data type */
enum Binary { case Zero, One }
```

The `binaryNums` is an implementation of the `Nums` trait satisfying the
following requirements:

```scala
def binaryNums: Nums { type Num = Vector[Binary] }
```

* `Num` is `Vector[Binary]` representing a binary number.
* `zero` is `Vector()`
* `one` is `List(One)`
* `add(x, y)` returns the sum of two binary numbers.
* `smul(k, x)` returns the scaled binary number by a scalar.
* `mul(x, y)` returns the product of two binary numbers.

> [!NOTE]
>
> The `Vector[Binary]` type represents a binary number as follows:
> * `Vector(One, One, Zero, Zero)` represents `1100` in binary number, which is
>   `8 + 4 = 12` in decimal number.


### (Problem #5) `matrixNums` (10 points)

The `matrixNums` is a method that takes a **matrix size** `n` of type `Int` and
returns an implementation of the `Nums` trait satisfying the following
requirements:

```scala
def matrixNums(n: Int): Nums { type Num = Vector[Vector[Int]] }
```

* `Num` is `Vector[Vector[Int]]` representing a matrix of size `n x n`.
* `zero` is a zero matrix of size `n x n`.
* `one` is a identity matrix of size `n x n`.
* `add(x, y)` returns the sum of two matrices.
* `smul(k, x)` returns the scaled matrix by a scalar.
* `mul(x, y)` returns the product of two matrices.



## Infinite Lists (50 points)

Using above `Nums` data types you implemented, you need to implement the
following infinite lists using `LazyList` structure in `Implementation.scala`.


### (Problem #6) `incs` (10 points)

The `incs` is a method that takes a `Nums` implementation `nums` and a starting
number `from` of type `nums.Num`, and returns a `LazyList` of all numbers
starting from `from` and incrementing by `nums.one`.

```scala
def incs(nums: Nums)(from: nums.Num): LazyList[nums.Num]
```


### (Problem #7) `squares` (10 points)

The `squares` is a method that takes a `Nums` implementation `nums` and returns
a `LazyList` of all square numbers starting from `nums.zero`.

```scala
def squares(nums: Nums): LazyList[nums.Num]
```


### (Problem #8) `powers` (10 points)

The `powers` is a method that takes a `Nums` implementation `nums` and a base
number `base` of type `nums.Num`, and returns a `LazyList` of all powers of the
base number (e.g., `base^0`, `base^1`, `base^2`, ...).

```scala
def powers(nums: Nums)(base: nums.Num): LazyList[nums.Num]
```


### (Problem #9) `repeat` (10 points)

The `repeat` is a method that takes a `Nums` implementation `nums`, a number
`x` of type `nums.Num`, and a boolean `cache`, and returns a `LazyList` of
repeating the number `x` infinitely. If `cache` is `true`, it should access `x`
only once and cache the result. Otherwise, it should access `x` every time.

```scala
def repeat(nums: Nums)(x: => nums.Num, cache: Boolean): LazyList[nums.Num]
```


### (Problem #10) `weights` (10 points)

The `weights` is a method that takes a `Nums` implementation `nums`, two numbers
`x` and `y` of type `nums.Num`, and returns a `LazyList` of all weighted sums
of `x` and `y` with integer-weighted coefficients.

```scala
def weights(nums: Nums)(
  x: nums.Num,
  y: nums.Num,
): LazyList[(BigInt, BigInt, nums.Num)]
```

For example, `weights(bigIntNums)(3, 2)` consists of the following elements in
this order:
* `(0, 0, 0)` (0 * 3 + 0 * 2 = 0)
* `(1, 0, 3)` (1 * 3 + 0 * 2 = 3)
* `(0, 1, 2)` (0 * 3 + 1 * 2 = 2)
* `(2, 0, 6)` (2 * 3 + 0 * 2 = 6)
* `(1, 1, 5)` (1 * 3 + 1 * 2 = 5)
* `(0, 2, 4)` (0 * 3 + 2 * 2 = 4)
* `(3, 0, 9)` (3 * 3 + 0 * 2 = 9)
* `(2, 1, 8)` (2 * 3 + 1 * 2 = 8)
* `(1, 2, 7)` (1 * 3 + 2 * 2 = 7)
* `(0, 3, 6)` (0 * 3 + 3 * 2 = 6)
* ...
