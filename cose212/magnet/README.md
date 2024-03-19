# `MAGNET` - Mutable Arithmetic Expressions with Generators and Exceptions

[English](./README.md) | [한국어](./README.ko.md)

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/magnet.g8
```

> [!WARNING]
>
> Read the [common instructions](https://github.com/ku-plrg-classroom/docs/blob/main/README.md) first if you have not read them.

The template source code contains the following files:
<pre><code>magnet
└─ src
   ├─ main/scala/kuplrg
   │  ├── MAGNET.scala ────────── The definition of the MAGNET and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `MAGNET` language is a toy language that stands for **M**utable
**A**rithmetic **G**e**N**erators with **E**xcep**T**ions. In this assignment,
you will implement two functions: `reduce` and `bodyOfSquares`.

## Specification of `MAGNET` language

See the [`magnet-spec.pdf`](./magnet-spec.pdf) for the syntax and semantics of
the `MAGNET` language.

### Run-time Errors

If the semantics of a given expression is not defined, the `reduce` function
should throw an exception using the `error` function.  For example, the
following code should throw an exception:
```scala
eval("1 + true")
```
However, you don't need to consider the specific error messages.  We will not
test error messages but only test whether the error is thrown by the `error`
function or not.


## (Problem #1) `reduce` (80 points)

The `eval` function is an interpreter of the `MAGNET` language. It takes an
expression in a string form and repeatedly calls the `reduce` function until the
continuation becomes empty to compute the final result:
```scala
def eval(str: String, debug: Boolean = false): String =
  def aux(st: State): Value =
    if (debug) println("\n" + "-" * 40 + "\n" + st.str)
    reduce(st) match
      case State(Nil, List(v), _, _) =>
        if (debug) println("\n" + "-" * 40 + "\n" + "Result: " + v)
        v
      case st => aux(st)
  val initSt = State(IEval(Map(), Expr(str)) :: Nil, Nil, Map(), Map())
  aux(initSt).str
```
> In addition, it supports the `debug` mode that prints the current state of the
> interpreter at each step.  For example, the following code prints the state of
> the interpreter at each step:
> ```scala
> eval("1 + 2 * 3", debug = true)
> ```
> After implementing the `reduce` function, it will print the following:
> ```
> ----------------------------------------
> * cont   : eval[_ |- (1 + (2 * 3))] :: []
> * stack  : []
> * handler:
> * mem    : []
> 
> ----------------------------------------
> * cont   : eval[_ |- 1] :: eval[_ |- (2 * 3)] :: (+) :: []
> * stack  : []
> * handler:
> * mem    : []
> 
> ...
> 
> ----------------------------------------
> Result: 7
> ```

The `reduce` function represents the reduction relation of the `MAGNET` language
as a function that reduces a state `st` to another state:
```scala
def reduce(st: State): State =
  val State(k, s, h, mem) = st
  ???
```

> The following helper functions are defined in the `Implementation.scala` file.
> You can use them in your implementation.
> ```scala
> def malloc(mem: Mem, n: Int): List[Addr] =
>   val a = malloc(mem)
>   (0 until n).toList.map(a + _)
>
> def malloc(mem: Mem): Addr = mem.keySet.maxOption.fold(0)(_ + 1)
>
> def lookup(env: Env, x: String): Addr =
>   env.getOrElse(x, error(s"free identifier: $x"))
>
> def lookup(handler: Handler, x: Control): KValue =
>   handler.getOrElse(x, error(s"invalid control operation: $x"))
>
> def eq(l: Value, r: Value): Boolean = (l, r) match
>   case (UndefV, UndefV) => true
>   case (NumV(l), NumV(r)) => l == r
>   case (BoolV(l), BoolV(r)) => l == r
>   case (IterV(l), IterV(r)) => l == r
>   case (ResultV(lv, ld), ResultV(rv, rd)) => eq(lv, rv) && ld == rd
>   case _ => false
> ```

**Please implement the `reduce` function in the `Implementation.scala` file.**


## (Problem #2) `bodyOfSquares` (20 points)

After passing all the tests of the `reduce` function, you will implement the
`bodyOfSquares` function.

> [!WARNING]
>
> If at least one test of the `reduce` function fails, the
> `bodyOfSquares` function will not be tested.  Therefore, you must pass all the
> tests of the `reduce` function before implementing the `bodyOfSquares`

The `bodyOfSquares` function returns a string that represents the body of the
`squares` function written in the `MAGNET` language:
```scala
def squareSumExpr(n: Int, m: Int): String = s"""
  function* squares(from, to) {
    $bodyOfSquares
  }
  var sum = 0;
  for (x of squares($n, $m)) { sum += x; };
  sum
"""

def bodyOfSquares: String = ???
```
The `squares` function is a **generator** function that generates the squares of
numbers from `from` to `to`. (Note that it returns an empty iterator if `from >
to`, and you could assume that it always takes two number arguments.)
```scala

> [!WARNING]
>
> Note that the `squares` function is a **generator** function added
> in the `MAGNET` language, not a normal function. Please try to understand the
> semantics of the generator function in the `MAGNET` language
> [specification](./magnet-spec.pdf) and given test cases in `Spec.scala`.

**Please implement the `bodyOfSquares` function in the `Implementation.scala`**
