# `TIFAE` - `TRFAE` with Type Inference

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/tifae.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
<pre><code>tifae
└─ src
   ├─ main/scala/kuplrg
   │  ├── TIFAE.scala ─────────── The definition of the TIFAE and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `TIFAE` language is an extension of the [`TRFAE`](../trfae/README.md) language
with **type inference**.  In this assignment, you will implement two
functions: `typeCheck` and `interp`.

## Specification of `TIFAE` language

See the [`tifae-spec.pdf`](./tifae-spec.pdf) for the syntax, type system, and
semantics of the `TIFAE` language.

### Type Errors

If the type checker finds a type error in a given expression, the `typeCheck`
function should throw an exception using the `error` function:
```scala
testExc(eval("x => 1 + true"))
```

### Run-time Errors

Similarly, if the semantics of the given expression is not defined, the `interp`
function should throw an exception using the `error` function:
```scala
testExc(eval("1 / 0"))
```
However, you don't need to consider the specific error messages for both type
errors and run-time errors.  We will not test error messages but only test
whether the error is thrown by the `error` function or not.

## The `eval` function

The `eval` function is a wrapper of the `typeCheck` and `interp` functions and
performs the following three steps:

1. It parses the given string into an expression.
1. It checks the type of the expression with the empty type environment and 
   the empty solution using the `typeCheck` function.
1. It converts the final type into a polymorphic type.
1. It evaluates the expression with the empty environment using the `interp`
   function

Finally, it returns the string representation of the pair of the resulting value
and the type of the expression:
```scala
def eval(str: String): String =
  val expr = Expr(str)
  val t = PolyType.from(typeCheck(expr, Map.empty, Map.empty))
  val v = interp(expr, Map.empty)
  s"${v.str}: ${t.str}"
```

## (Problem #1) `typeCheck`

The `typeCheck` function checks the type of the given expression `expr` with 1)
the given type environment `tenv` and 2) the given solution `sol` of type
variables, and it returns the pair of the resulting type and the updated
solution.
```scala
def typeCheck(
  expr: Expr,
  tenv: TypeEnv,
  sol: Solution
): (Type, Solution) = ???
```
**Please implement the `typeCheck` function in the `Implementation.scala`
file.**

## (Problem #2) `interp`

The `interp` function evaluates the given expression `expr` with the given
environment `env` and returns the result:
```scala
def interp(expr: Expr, env: Env): Value = ???
```
**Please implement the `interp` function in the `Implementation.scala` file.**
