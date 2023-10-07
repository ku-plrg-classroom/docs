# `COBALT` - Comprehension-supported Boolean and Arithmetic Expression with Lists and Tuples

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/cobalt.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

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
**L**ists and **T**uples. In this assignment, you will implement two `interp`
functions.

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
| `not a list` | The expression does not evaluate to a list in the list operation. |
| `empty list` | The list is empty in the list head or tail operation. |
| `out of bounds` | The index is out of bounds in the tuple projection. |
| `not a tuple` | The expression does not evaluate to a tuple in the tuple projection. |

## (Problem #1) `interp`

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
def length(list: Value): BigInt = ???
def map(list: Value, fun: Value): Value = ???
def join(list: Value): Value = ???
def filter(list: Value, fun: Value): Value = ???
def app(fun: Value, args: List[Value]): Value = ???
```
