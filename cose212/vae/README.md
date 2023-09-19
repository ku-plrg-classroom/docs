# `VAE` - Arithmetic Expressions with Variables

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/vae.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
<pre><code>vae
└─ src
   ├─ main/scala/kuplrg
   │  ├── VAE.scala ───────────── The definition of the VAE and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `VAE` language is a simple arithmetic expression language with variables,
and it supports addition and multiplication of integers. In this assignment, you
will implement five functions: `interp`, `freeIds`, `bindingIds`, `boundIds`,
and `shadowedIds`.

## Specification of `VAE` language

See the [`vae-spec.pdf`](./vae-spec.pdf) for the syntax and semantics
of the `VAE` language.

## (Problem #1) `interp` (20 points)

The `interp` function evaluates the given expression and returns the result:
```scala
def interp(expr: Expr, env: Env): Value = ???
```
Please implement the `interp` function in the `Implementation.scala` file.

You should throw an exception using the `error` function if the given expression
contains a free identifier. For example, the following test case checks whether
the `interp` function throws an exception containing the message `unknown
identifier: y` because the expression `val x = 1; y` contains a free identifier
`y`:

```scala
testExc(eval("val x = 1; y"), "unknown identifier: y")
```

## (Problem #2) `freeIds` (20 points)

The `freeIds` function returns the set of free identifiers in the given
expression:
```scala
def freeIds(expr: Expr): Set[String] = ???
```
Please implement the `freeIds` function in the `Implementation.scala` file.

## (Problem #3) `bindingIds` (20 points)

The `bindingIds` function returns the set of binding identifiers in the given
expression:
```scala
def bindingIds(expr: Expr): Set[String] = ???
```
Please implement the `bindingIds` function in the `Implementation.scala` file.

## (Problem #4) `boundIds` (20 points)

The `boundIds` function returns the set of bound identifiers in the given
expression:
```scala
def boundIds(expr: Expr): Set[String] = ???
```
Please implement the `boundIds` function in the `Implementation.scala` file.

## (Problem #5) `shadowedIds` (20 points)

The `shadowedIds` function returns the set of shadowed identifiers in the given
expression:
```scala
def shadowedIds(expr: Expr): Set[String] = ???
```
Please implement the `shadowedIds` function in the `Implementation.scala` file.
