# `MiniFSharp` - A Small Subset of F# Language

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/mini-fsharp.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>mini-fsharp
└─ src
   ├─ main/scala/kuplrg
   │  ├── MiniFSharp.scala ────────── The definition of the mini-fsharp and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `MiniFSharp` language is a toy language inspired by the [F# programming
language](https://fsharp.org/). The name `MiniFSharp` indicates that it is a
minimal subset of the F# language.

## Specification of `MiniFSharp` language

See the [`mini-fsharp-spec.pdf`](./mini-fsharp-spec.pdf) for the syntax and
semantics of the `MiniFSharp` language.

### Run-time Errors

If the given expression meets the following conditions during evaluation, the
`interp` function should throw an exception using the `error` function with
corresponding error messages containing their error kinds:

| Error kind | Description |
|:-----------|:------------|
| `free identifier` | The given identifier is not bound in the environment. |
| `invalid operation` | The given operation is not defined for the given operands. |
| `not a function` | The expression does not evaluate to a function in the function application. |
| `not a boolean` | The expression does not evaluate to a boolean in the conditional expression. |
| `not a list` | The expression does not evaluate to a list in the list operation. |
| `pattern match failure` | The pattern does not match the given value. |
| `unmatched value` | The value does not match any of the cases in the match expression. |

Note that this language's semantics do not sometimes define the order of
subexpressions. For example, the order between evaluating the subexpressions of
the addition operation is not defined. Therefore, the following expression can
throw two different errors depending on the order of subexpressions:
```scala
interp(Expr("(1 + x) + (1 + true)")) // `free identifier` or `invalid operation`
```
They are both valid errors, and you can throw either of them. It means that you
do not need to consider the order of subexpressions, and you can assume any
convenient order. Also, we will not test such cases in this assignment.


## (Problem #1) `interp` (80 points)

The `eval` function is a wrapper of the `interp` function. It parses the given
string into an expression and evaluates it with the empty environment:

```scala
def eval(str: String): String = interp(Expr(str), Map.empty).str
```

The `interp` function evaluates the given expression `expr` with the given
environment `env` and returns the result:
```scala
def interp(expr: Expr, env: Env): Value = ???
```
**Please implement the `interp` function in the `Implementation.scala` file.**

## (Problem #2) `hanoiMovesBody` (20 points)

In this problem, you will use `MiniFSharp` to implement the solution of the
[Tower of Hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi) problem, which is
a classic problem that involves moving a set of disks from one peg to another,
following specific rules.

- You have three pegs and a number of disks of different sizes that can slide
  onto any peg.
- The puzzle starts with the disks stacked in ascending order of size on source
  peg, the smallest at the top, thus making a conical shape.
- The objective of the puzzle is to move the entire stack to the target peg,
  obeying the following rules:
  1. Only one disk can be moved at a time.
  2. Each move consists of taking the upper disk from one of the stacks and
     placing it on top of another stack or on an empty peg.
  3. No larger disk may be placed on top of a smaller disk.


The `hanoiMoves` function is a `MiniFSharp` function that takes the following
four parameters and returns a list of moves to solve the Towers of Hanoi
problem:
- `n`: an integer representing the number of disks,
- `source`: an integer representing the source peg,
- `temp`: an integer representing the temporary peg, and
- `target`: an integer representing the target peg.

```scala
def hanoiMovesExpr(n: Int, source: Int, temp: Int, target: Int): String = s"""
  let hanoiMoves n source temp target = $hanoiMovesBody in
  hanoiMoves $n $source $temp $target
"""
```

The `hanoiMovesBody` is the body of the `hanoiMoves` function that you need to
implement.

```scala
def hanoiMovesBody: String = ???
```

For example, the following test case checks whether the `hanoiMoves` function
correctly computes the sequence of moves to transfer three disks from peg 0 to
peg 2 using peg 1 as a temporary peg.
```scala
test(
  afterAllPass(eval(hanoiMovesExpr(3, 0, 1, 2))),
  "[(0, 2); (0, 1); (2, 1); (0, 2); (1, 0); (1, 2); (0, 2)]",
  weight = 50,
)
```
The first move `(0, 2)` indicates moving the top disk from peg 0 to peg 2, and
so on.

> [!WARNING]
>
> If at least one test of the `interp` function fails, the `sqsumIfExpr`
> function will not be tested.  Therefore, you must pass all the tests of the
> `interp` function before implementing the `sqsumIfExpr`

**Please implement the `hanoiMovesBody` in the `Implementation.scala` file.**
