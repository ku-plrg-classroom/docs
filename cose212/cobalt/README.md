# `COBALT` - Comprehension-supported Boolean and Arithmetic Expression with Lists and Tuples

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/cobalt.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>cobalt
└─ src
   ├─ main/scala/kuplrg
   │  ├── COBALT.scala ────────── The definition of the COBALT and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `COBALT` language is a toy language that stands for
**CO**mprehension-supported **B**oolean and **A**rithmetic expressions with
**L**ists and **T**uples. In this assignment, you will implement the `interp`
function.

## Specification of `COBALT` language

See the [`cobalt-spec.pdf`](./cobalt-spec.pdf) for the syntax and semantics of the
`COBALT` language.

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
| `not a boolean` | The given function for `filter` does not evaluate to a boolean. |
| `not a list` | The expression does not evaluate to a list in the list operation. |
| `empty list` | The list is empty in the list head or tail operation. |
| `out of bounds` | The index is out of bounds in the tuple projection. |
| `not a tuple` | The expression does not evaluate to a tuple in the tuple projection. |
| `arity mismatch` | The number of arguments does not match the number of parameters. |

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

You can implement and utilize the following helper functions, but it is not
mandatory. You can also implement your own helper functions.
```scala
def eq(left: Value, right: Value): Boolean = ???
def map(list: Value, func: Value): Value = ???
def join(list: Value): Value = ???
def filter(list: Value, func: Value): Value = ???
def foldLeft(list: Value, init: Value, func: Value): Value = ???
def app(func: Value, args: List[Value]): Value = ???
```

## (Problem #2) `sqsumIfExpr` (20 points)

In this problem, you will implement the `subExpr1` and `subExpr2` functions to
complete the `sqsumIfExpr` function.

> [!WARNING]
>
> If at least one test of the `interp` function fails, the `sqsumIfExpr`
> function will not be tested.  Therefore, you must pass all the tests of the
> `interp` function before implementing the `sqsumIfExpr`


The `sqsumIfExpr` function takes
- a string `lists` that represents a list of lists of integers, and
- a string `pred` that represents a predicate function that takes an integer and
  returns a boolean.
and returns a string that computes the sum of the squares of the integers in the
lists that satisfy the predicate function.

The `subExpr1` and `subExpr2` are its subexpressions that you need to implement.

```scala
def sqsumIfExpr(lists: String, pred: String): String = s"""
  val sumIf = (lists, pred) => {
    (for {
      $subExpr1
    } yield $subExpr2)
      .foldLeft(0, (x, y) => x + y)
  };
  sumIf($lists, $pred)
"""

def subExpr1: String = ???

def subExpr2: String = ???
```

> [!WARNING]
>
> Note that the generated expression utilizes **for-comprehension** language
> feature added in the `COBALT` language. Please try to understand the
> **for-comprehension** feature by reading the `COBALT` language
> [specification](./cobalt-spec.pdf) and given test cases in `Spec.scala`.

**Please implement the `subExpr1` and `subExpr2` functions in the
`Implementation.scala` file.**
