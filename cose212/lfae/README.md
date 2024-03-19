# `LFAE` - `FAE` with Lazy Evaluation

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/lfae.g8
```

> [!WARNING]
>
> Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
<pre><code>lfae
└─ src
   ├─ main/scala/kuplrg
   │  ├── LFAE.scala ──────────── The definition of the LFAE and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `LFAE` language is an extension of the [`FAE`](../fae/README.md) language
with **lazy evaluation**. In this assignment, you will implement the `interp`
function.

## Specification of `LFAE` language

See the [`lfae-spec.pdf`](./lfae-spec.pdf) for the syntax and semantics of the
`LFAE` language.

### Run-time Errors

If the given expression meets the following conditions during evaluation, the
`interp` function should throw an exception using the `error` function with
corresponding error messages containing their error kinds:

| Error kind | Description |
|:-----------|:------------|
| `free identifier` | The given identifier is not bound in the environment. |
| `invalid operation` | The given operation is not defined for the given operands. |
| `not a function` | The expression does not evaluate to a function in the function application. |

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
