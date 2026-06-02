# `OccurTy` - Occurrence Typing for a Simple Functional Language

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/occur-ty.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>occur-ty
└─ src
   ├─ main/scala/kuplrg
   │  ├── Lang.scala ──────────── The definition of the language and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

It is a template for implementing an **occurrence typing** system for
a simple functional language. In this assignment, you will implement the
`tycheck` function.

## Specification of Language

The language semantics strictly follows the implementation of the `interp`
function in the `Template.scala` file.

## (Problem #1) `tycheck` function

The `tycheck` function performs type checking for the language:
```scala
def tycheck(expr: Expr): Ty
```
It takes an abstract syntax tree (AST) of an expression (i.e., `Expr`) and
returns its type (i.e., `Ty`). First, you need to implement the basic type
check for the language. Then, you need to advance the type checking by
implementing occurrence typing.

### Soundness of Type Checker

Your type checker should produce a **sound** type result, which means that the
type of an expression should be a supertype of the type of its evaluation
result.

The following function checks the soundness of a type result:

```scala
// check that a value conforms to a type
def validate(v: Option[Value], ty: Option[Ty]): Boolean = (v, ty) match
  case (_, None)           => true
  case (Some(v), Some(ty)) => typeOf(v) <= ty
  case _                   => false
```

If an interpreter throws an error (i.e., `v` is `None`), the type checker must
reject the expression (i.e., `ty` is `None`). If an interpreter returns a value
(i.e., `v` is `Some(v)`), the type checker must return a type that is a
supertype of the value's type (`typeOf(v) <= ty`) or reject the expression.


### Precision of Type Checker

Using the occurrence typing, your type checker should produce a precise type
result, which means that the type of an expression should be a subtype of the
given expected type in the test cases.


### Test Cases

The `Spec.scala` file contains some test cases for the `tycheck` function using
the `test` function.

For example, the following test case checks that the type of the expression
`true` should be a subtype of `Boolean`:

```scala
check(test("true", "Boolean"))
```

If you return a wrong type `Number` for the expression `true`, it fails with the
following soundness error:

```
[ERROR] unsound value true of type Number
```

If you return an imprecise type `Top` for the expression `true`, it fails with
the following precision error:

```
[ERROR] imprecise type Top but expected Boolean
```


**Please implement the `tycheck` function in the `Implementation.scala` file.**
