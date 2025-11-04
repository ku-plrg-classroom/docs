# `MiniPython` - A Small Subset of Python Language

Please download the template code as follows:
```bash
sbt new ku-plrg-classroom/mini-python.g8
```

> [!WARNING]
>
> Read the [common instructions](/scala.md) first if you have not read them.

The template source code contains the following files:
<pre><code>mini-python
└─ src
   ├─ main/scala/kuplrg
   │  ├── MiniPython.scala ────── The definition of the mini-python and parsers
   │  ├── Implementation.scala ── <b style='color:red;'>[[ IMPLEMENT AND SUBMIT THIS FILE ]]</b>
   │  ├── Template.scala ──────── The templates of target functions
   │  └── error.scala ─────────── The definition of the `error` function
   └─ test/scala/kuplrg
      ├─ Spec.scala ───────────── <b style='color:red;'>[[ ADD YOUR OWN TESTS ]]</b>
      └─ SpecBase.scala ───────── The base class of test cases</code></pre>

The `MiniPython` language is a toy language inspired by the [Python programming
language](https://www.python.org/). The name `MiniPython` indicates that it is a
minimal subset of the Python language.

## Specification of `MiniPython` language

See the [`mini-python-spec.pdf`](./mini-python-spec.pdf) for the syntax and
semantics of the `MiniPython` language.

### Run-time Errors

If the given program stops with a run-time error with an error kind (i.e.,
`Error` class in `MiniPython.scala`), the `reduce` function should throw an
exception using the `error` function with the string form (i.e., `_.str`) of the
error kind.

For example, the following code should throw a `NameError` exception:
```python
# MiniPython
x
```
because the reduction rule for variable lookup raises a `NameError` in the
[`mini-python-spec.pdf`](./mini-python-spec.pdf) for this case but there is no
`try`-`except` block to catch it.



## The `eval` function

The `eval` function is an interpreter of the `MiniPython` language. It takes a
program in a string form and repeatedly calls the `reduce` function until the
continuation becomes empty to compute the final result:
```scala
def eval(str: String, debug: Boolean = false): String =
  def aux(st: State): String =
    if (debug) log(st.str)
    st match
      case State(Nil, List(v), _, mem) =>
        val result = v.str(mem)
        if (debug) log("Result: " + result)
        result
      case _ => aux(reduce(st))
  def log(msg: String): Unit = println("-" * 80 + "\n" + msg)
  val Program(stmts, expr) = Program(str)
  val xs = if (stmts.isEmpty) Nil else locals(Block(stmts)).toList
  val addrs = (0 until xs.size)
  val mem = addrs.map(_ -> Value.NoneV).toMap
  val env = (xs zip addrs).toMap
  val initK = stmts.map(IStmt(env, _)) ::: IExpr(env, expr) :: Nil
  val initSt = State(initK, Nil, Map.empty, mem)
  aux(initSt)
```

> In addition, it supports the `debug` mode that prints the current state of the
> interpreter at each step.  For example, the following code prints the state of
> the interpreter at each step:
> ```scala
> eval("1 + 2 * 3", debug = true)
> ```
> After implementing the `reduce` function, it will print the following:
> ```
> --------------------------------------------------------------------------------
> * env    :
> * cont   : eval[<env> |- (1 + (2 * 3))] :: []
> * stack  : []
> * handler:
> * mem    : []
> --------------------------------------------------------------------------------
> * env    :
> * cont   : eval[<env> |- 1] :: eval[<env> |- (2 * 3)] :: (+) :: []
> * stack  : []
> * handler:
> * mem    : []
> --------------------------------------------------------------------------------
>
> ...
>
> --------------------------------------------------------------------------------
> * cont   : []
> * stack  : 7 :: []
> * handler:
> * mem    : []
> --------------------------------------------------------------------------------
> Result: 7
> ```


## The `reduce` function

The `reduce` function represents the reduction relation of the `MiniPython`
language as a function that reduces a state `st` to another state:
```scala
def reduce(st: State): State =
  val State(k, s, h, m) = st
  ???
```

**Please implement the `reduce` function in the `Implementation.scala` file.**


## The `locals` function

The `locals` function takes a block of statements and returns the set of local
variable names defined in the block.

```scala
def locals(block: Block): Set[String] = ???
```

The foraml definition of the `locals` function and other helper functions are
provided in the [`mini-python-spec.pdf`](./mini-python-spec.pdf).

**Please implement the `locals` function in the `Implementation.scala` file.**
