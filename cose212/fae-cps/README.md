# `FAE-cps` - `FAE` with Continuation-Passing Style

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/fae-cps.g8
```

> :warning: Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
<pre><code>fae
└─ src
   ├─ main/scala/kuplrg
   │  ├── FAE.scala ───────────── The definition of the FAE and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `FAE-cps` language is exactly the same as the [`FAE`](../fae/README.md)
language, but it uses the **continuation-passing style** (CPS) for the
interpreter. In this assignment, you will implement the `interp` function.

## Specification of `FAE-cps` language

The specification of the `FAE-cps` language is exactly the same as the
[`FAE`](../fae/README.md) language. Thus, see the
[`fae.pdf`](../fae/fae-spec.pdf) for the syntax and semantics of the `FAE`
language.

### Run-time Errors

If the given expression meets the following conditions during evaluation, the
`interp` function should throw an exception using the `error` function with
corresponding error messages containing their error kinds:

| Error kind | Description |
|:-----------|:------------|
| `free identifier` | The given identifier is not bound in the environment. |
| `invalid operation` | The given operation is not defined for the given operands. |
| `not a function` | The expression does not evaluate to a function in the function application. |

## (Problem #1) `interp` (100 points)

The `eval` function is a wrapper of the `interp` function. It parses the given
string into an expression and evaluates it with the empty environment and the
identity function as the continuation:

```scala
def eval(str: String): String = interp(Expr(str), Map.empty, v => v).str
```

The `interp` function evaluates the given expression `expr` with the given
environment `env` and the continuation `k` to produce a value:
```scala
def interp(expr: Expr, env: Env, k: Cont): Value = ???
```
**Please implement the `interp` function in the `Implementation.scala` file.**
