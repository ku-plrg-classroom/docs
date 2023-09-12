# `AE` - Arithmetic Expressions

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/ae.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
<pre><code>ae
└─ src
   ├─ main/scala/kuplrg
   │  ├── AE.scala ────────────── The definition of the AE and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `AE` language is a simple arithmetic expression language that supports
addition and multiplication of integers. In this assignment, you will implement
two functions: `interp` and `countNums`.

## Specification of `AE` language

See the [`ae-spec.pdf`](./ae-spec.pdf) for the syntax and semantics
of the `AE` language.

## (Problem #1) `interp` (50 points)

The `interp` function evaluates the given expression and returns the result:
```scala
def interp(expr: Expr): Value = ???
```
Please implement the `interp` function in the `Implementation.scala` file.


## (Problem #2) `countNums` (50 points)

The `countNums` function counts the number of `Num` nodes in the given
expression:
```scala
def countNums(expr: Expr): Int = ???
```
Please implement the `countNums` function in the `Implementation.scala` file.
