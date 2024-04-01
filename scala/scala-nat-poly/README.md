# Natural Numbers and Polynomials in Scala

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/scala-nat-poly.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.
> first if you have not read them.

* **[Natural Numbers (50 points)](#natural-numbers-50-points)**
  * [(Problem #1) `toInt` (10 points)](#problem-1-toint-10-points)
  * [(Problem #2) `+` (10 points)](#problem-2--10-points)
  * [(Problem #3) `*` (10 points)](#problem-3--10-points)
  * [(Problem #4) `**` (10 points)](#problem-4--10-points)
  * [(Problem #5) `isEven` and `isOdd` (10 points)](#problem-5-iseven-and-isodd-10-points)
* **[Polynomials (50 points)](#polynomials-50-points)**
  * [(Problem #6) `eval` (10 points)](#problem-6-eval-10-points)
  * [(Problem #7) `+` (10 points)](#problem-7--10-points)
  * [(Problem #8) `*` with an Integer (10 points)](#problem-8--with-an-integer-10-points)
  * [(Problem #9) `*` with a Polynomial (10 points)](#problem-9--with-a-polynomial-10-points)
  * [(Problem #10) `normalize` (10 points)](#problem-10-normalize-10-points)


## Natural Numbers (50 points)

The `Nat` type represents **natural numbers** as an algebraic data type (ADT).
* `Z` represents **zero**.
* `S(n)` represents the **successor** of `n`.

```scala
/** Natural numbers (Z for zero, S for successor) */
enum Nat extends NatOps:
  case Z
  case S(n: Nat)
```

> [!NOTE]
>
> The `Nat` enum type supports another constructor using `apply` method of its
> companion object. For example, `Nat(3)` is equivalent to `S(S(S(Z)))`.

The `NatOps` trait defines **six abstract methods** for natural numbers. You
need to implement these methods in `Implementation.scala`.

### (Problem #1) `toInt` (10 points)

The `toInt` method returns the **integer value** of a natural number. For
example, it should pass the following tests:

```scala
test(Z.toInt, 0)
test(S(Z).toInt, 1)
test(S(S(Z)).toInt, 2)
test(S(S(S(Z))).toInt, 3)
test(S(S(S(S(Z)))).toInt, 4)
```

### (Problem #2) `+` (10 points)

The `+` method is a binary operator for natural numbers. It returns the **sum**
of two natural numbers. For example, it should pass the following tests:

```scala
test(Z + Z, Z)
test(Z + S(Z), S(Z))
test(S(Z) + Z, S(Z))
test(S(S(S(Z))) + S(S(Z)), Nat(5))
test(S(S(S(S(Z)))) + S(S(S(S(Z)))), Nat(8))
```

### (Problem #3) `*` (10 points)

The `*` method is a binary operator for natural numbers. It returns the
**product** of two natural numbers. For example, it should pass the following
tests:

```scala
test(Z * Z, Z)
test(S(Z) * S(S(Z)), S(S(Z)))
test(S(S(Z)) * Z, Z)
test(S(S(S(Z))) * S(S(Z)), Nat(6))
test(S(S(S(S(Z)))) * S(S(S(Z))), Nat(12))
```

### (Problem #4) `**` (10 points)

The `**` method is a binary operator for natural numbers. It returns the
**exponentiation** of two natural numbers. For example, it should pass the
following tests:

```scala
test(Z ** Z, S(Z))
test(S(Z) ** S(S(Z)), S(Z))
test(S(S(Z)) ** Z, S(Z))
test(S(S(S(Z))) ** S(S(Z)), Nat(9))
test(S(S(S(S(Z)))) ** S(S(S(Z))), Nat(64))
```

### (Problem #5) `isEven` and `isOdd` (10 points)

The `isEven` and `isOdd` methods return `true` if a natural number is **even**
or **odd**, respectively. For example, they should pass the following tests:

```scala
test(Z.isOdd, false)
test(S(Z).isEven, false)
test(S(S(Z)).isEven, true)
test(S(S(S(Z))).isOdd, true)
test(S(S(S(S(Z)))).isEven, true)
```



## Polynomials (50 points)

The `Poly` type represents **polynomials** as an algebraic data type (ADT).
* `coefficients` is a list of integers representing the **coefficients** of the
    polynomial.

```scala
/** Polynomials */
case class Poly(coefficients: List[Int]) extends PolyOps:
```

For example, the following polynomials are examples of `Poly`:

```scala
Poly(List(1, 2, 3))             // 1 + 2x + 3x^2
Poly(List(-5, 0, 0, 0, 1))      // -5 + x^4
```

> [!NOTE]
>
> The `Poly` case class supports `toString` method to represent a polynomial as
> a string. For example, `Poly(List(1, 2, 3)).toString` returns `"1 + 2x +
> 3x^2"`. And, it also supports `apply` method of its companion object to
> create a polynomial from a variable argument list of integers. For example,
> `Poly(1, 2, 3)` is equivalent to `Poly(List(1, 2, 3))`.

The `PolyOps` trait defines **five abstract methods** for polynomials. You need
to implement these methods in `Implementation.scala`.

### (Problem #6) `eval` (10 points)

The `eval` method **evaluates** a polynomial at a given integer `x`. For
example, it should pass the following tests:

```scala
test(Poly().eval(3), 0)
test(Poly(1).eval(2), 1)
test(Poly(1, 2).eval(4), 9)
test(Poly(1, 2, 3).eval(1), 6)
test(Poly(1, 2, 3, 4).eval(5), 586)
test(Poly(1, 2, 3, 4, 5).eval(2), 129)
test(Poly(5, 3, 4, 2).eval(5), 370)
test(Poly(5, 3, 4, 2, 5).eval(7), 12913)
test(Poly(5, 3, 4, 2, 9, 7).eval(10), 792435)
test(Poly(5, 3, 6, 2, 3, 4, 2).eval(2), 355)
```

### (Problem #7) `+` (10 points)

The `+` method is a binary operator for polynomials. It returns the **sum** of
two polynomials. For example, it should pass the following tests:

```scala
test(Poly() + Poly(), Poly())
test(Poly() + Poly(1), Poly(1))
test(Poly(1) + Poly(1, 2, 3), Poly(2, 2, 3))
test(Poly(1, 2) + Poly(1, 2, 3), Poly(2, 4, 3))
test(Poly(1, 2, 3) + Poly(1), Poly(2, 2, 3))
test(Poly(1, 2, 3) + Poly(1, 2), Poly(2, 4, 3))
test(Poly(1, 2, 3) + Poly(1, 2, 3), Poly(2, 4, 6))
test(Poly(-1, 0, 0, 1) + Poly(1, 0, 0, -1), Poly())
test(Poly(1, 2, 3) + Poly(1, 2, -3), Poly(2, 4))
test(Poly(1, 2, 3, 4) + Poly(2, 4, -3, -4), Poly(3, 6))
```

### (Problem #8) `*` with an Integer (10 points)

The `*` method is a binary operator for polynomials. It returns the **product**
of a polynomial and an integer. For example, it should pass the following tests:

```scala
test(Poly() * 0, Poly())
test(Poly(1) * 0, Poly())
test(Poly(1, 2, 3) * 0, Poly())
test(Poly() * 1, Poly())
test(Poly(1) * 1, Poly(1))
test(Poly(1, 2, 3) * 1, Poly(1, 2, 3))
test(Poly() * 2, Poly())
test(Poly(1) * 2, Poly(2))
test(Poly(1, 2, 3) * 2, Poly(2, 4, 6))
test(Poly(1, 5, 4, 6, 2, 3) * 10, Poly(10, 50, 40, 60, 20, 30))
```

### (Problem #9) `*` with a Polynomial (10 points)

The `*` method is a binary operator for polynomials. It returns the **product**
of two polynomials. For example, it should pass the following tests:

```scala
test(Poly() * Poly(), Poly())
test(Poly() * Poly(1), Poly())
test(Poly(-2, 1) * Poly(2, 1), Poly(-4, 0, 1))
test(Poly(1) * Poly(), Poly())
test(Poly(1, 2) * Poly(1, 2), Poly(1, 4, 4))
test(Poly(1, 2) * Poly(1, 2, 3), Poly(1, 4, 7, 6))
test(Poly(1, 2, 3) * Poly(1, 2), Poly(1, 4, 7, 6))
test(Poly(1, 2, 3) * Poly(1, 2, 3), Poly(1, 4, 10, 12, 9))
test(Poly(1, 2, 3, 4) * Poly(1, 2), Poly(1, 4, 7, 10, 8))
test(Poly(3, 5, 2, 5) * Poly(3, 5, 2, 3), Poly(9, 30, 37, 44, 44, 16, 15))
```

### (Problem #10) `normalize` (10 points)

The `normalize` method returns a polynomial with **zero coefficients removed**
from the end. For example, it should pass the following tests:

```scala
test(Poly().normalize, Poly())
test(Poly(0).normalize, Poly())
test(Poly(0, 0).normalize, Poly())
test(Poly(0, 0, 0).normalize, Poly())
test(Poly(1).normalize, Poly(1))
test(Poly(1, 0).normalize, Poly(1))
test(Poly(1, 0, 0).normalize, Poly(1))
test(Poly(1, 0, 0, 0).normalize, Poly(1))
test(Poly(3, 0, 5, 0, 5).normalize, Poly(3, 0, 5, 0, 5))
test(Poly(5, 0, 6, 0, 0, 0, 0).normalize, Poly(5, 0, 6))
```
