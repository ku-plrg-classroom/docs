# `MFAE` - `FAE` with Mutable Variables

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/mfae.g8
```

> [!WARNING]
>
> Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
<pre><code>mfae
└─ src
   ├─ main/scala/kuplrg
   │  ├── MFAE.scala ──────────── The definition of the MFAE and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `MFAE` language is an extension of the [`FAE`](../fae/README.md) language
with **mutable variables**. In this assignment, you will implement two
functions: `interp` and `interpCBR`.

## Specification of `MFAE` language

See the [`mfae-spec.pdf`](./mfae-spec.pdf) for the syntax and semantics of the
`MFAE` language.

### Run-time Errors

If the given expression meets the following conditions during evaluation, the
`interp` (or `interpCBR`) function should throw an exception using the `error`
function with corresponding error messages containing their error kinds:

| Error kind | Description |
|:-----------|:------------|
| `free identifier` | The given identifier is not bound in the environment. |
| `invalid operation` | The given operation is not defined for the given operands. |
| `not a function` | The expression does not evaluate to a function in the function application. |

## (Problem #1) `interp`

The `eval` function is a wrapper of the `interp` function. It 1) parses the
given string into an expression, 2) evaluates it with the empty environment and
the empty memory, and 3) returns only the value in the string format:
```scala
def eval(str: String): String =
  val (v, _) = interp(Expr(str), Map.empty, Map.empty)
  v.str
```

The `interp` function 1) evaluates the given expression `expr` with the given
environment `env` and memory `mem`, and 2) returns a pair of value and memory as
the result:
```scala
def interp(expr: Expr, env: Env, mem: Mem): (Value, Mem) = ???
```
**Please implement the `interp` function in the `Implementation.scala` file.**

## (Problem #2) `interpCBR`

The `evalCBR` function is a wrapper of the `interpCBR` function, and performs
the similar tasks as the `eval` function except that it uses the `interpCBR`:
```scala
def evalCBR(str: String): String =
  val (v, _) = interpCBR(Expr(str), Map.empty, Map.empty)
  v.str
```

The `interpCBR` function 1) evaluates the given expression `expr` with the given
environment `env` and memory `mem`, and 2) returns a pair of value and memory as
the result:
```scala
def interpCBR(expr: Expr, env: Env, mem: Mem): (Value, Mem) = ???
```
> [!WARNING]
>
> The original `interp` function uses the **call-by-value (CBV)**
> evaluation strategy. But, the `interpCBR` function uses the
> **call-by-reference (CBR)** evaluation strategy. Please see the Section 4.1 of
> the [spec](./mfae-spec.pdf) for the difference between the CBV and the CBR
> evaluation strategies.

**Please implement the `interpCBR` function in the `Implementation.scala` file.**
