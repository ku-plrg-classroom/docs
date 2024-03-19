# `FVAE` - `VAE` with First-Class Functions

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/fvae.g8
```

> [!WARNING]
>
> Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
<pre><code>fvae
└─ src
   ├─ main/scala/kuplrg
   │  ├── FVAE.scala ──────────── The definition of the FVAE and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `FVAE` language is an extension of the [`VAE`](../vae/README.md) language
with **first-class functions**. In this assignment, you will implement two
functions: `interp` and `interpDS`.

## Specification of `FVAE` language

See the [`fvae-spec.pdf`](./fvae-spec.pdf) for the syntax and semantics of the
`FVAE` language.

### Run-time Errors

If the given expression meets the following conditions during evaluation, the
`interp` (or `interpDS`) function should throw an exception using the `error`
function with corresponding error messages containing their error kinds:

| Error kind | Description |
|:-----------|:------------|
| `free identifier` | The given identifier is not bound in the environment. |
| `invalid operation` | The given operation is not defined for the given operands. |
| `not a function` | The expression does not evaluate to a function in the function application. |

## (Problem #1) `interp` (50 points)

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

## (Problem #2) `interpDS` (50 points)

The `evalDS` function is a wrapper of the `interpDS` function, and performs the
similar tasks as the `eval` function except that it uses the `interpDS`:
```scala
def evalDS(str: String): String = interpDS(Expr(str), Map.empty).str
```

The `interpDS` function evaluates the given expression `expr` with the given
environment `env` and returns the result:
```scala
def interpDS(expr: Expr, env: Env): Value = ???
```
> [!WARNING]
>
> The original `interp` function uses the **static scoping** (i.e.,
> lexical scoping). But, the `interpDS` function uses the **dynamic scoping**.
> Please see the Section 4.1 of the [spec](./fvae-spec.pdf) for the difference
> between the static scoping and the dynamic scoping.

**Please implement the `interpDS` function in the `Implementation.scala` file.**
