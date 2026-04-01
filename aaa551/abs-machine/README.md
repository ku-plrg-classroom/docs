# `AbsMachine` - An Abstract Machine for a Simple Functional Language

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/abs-machine.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>abs-machine
└─ src
   ├─ main/scala/kuplrg
   │  ├── Lang.scala ──────────── The definition of the language and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

It is a simple functional language defined in an **abstract machine** (a variant
of a CEK machine) style. In this assignment, you will implement the `reduce`
function.

## Specification of Language

See the [`lang-spec.pdf`](./lang-spec.pdf) for the syntax and semantics of the
language.

### Run-time Errors

If the given expression meets the following conditions during evaluation, the
`reduce` function should throw an exception using the `error` function.

## (Problem #1) `reduce`

The `eval` function is an interpreter that takes an expression in a string form
and repeatedly calls the `reduce` function until the continuation becomes empty
to compute the final result:
```scala
def eval(str: String): String =
  import Cont.*
  def aux(k: Cont, s: Stack): Value = reduce(k, s) match
    case (EmptyK, List(v)) => v
    case (k, s) => aux(k, s)
  aux(EvalK(Map.empty, Expr(str), EmptyK), List.empty).str
```

The `reduce` function represents the transition rules of the language as a
function that reduces a state `(k, s)` consisting of a continuation `k` and a
stack `s` into a new state `(k', s')`:
```scala
def reduce(k: Cont, s: Stack): (Cont, Stack) = (k, s) match
  case (EvalK(env, expr, k), s) =>
    expr match
      case Def(n, p, b, e) =>
        lazy val fenv: Env = env + (n -> CloV(p, b, () => fenv))
        (EvalK(fenv, e, k), s)
      case _ => ???
  case _ => ???
```

**Please implement the `reduce` function in the `Implementation.scala` file.**
